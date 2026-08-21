# TickForge Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build TickForge — a real-time crypto market platform: Rust ingestion of Binance public streams into Kafka, in-memory L2 order books with µs-level updates, incremental candles/indicators, an event-sourced paper-trading matching engine, TimescaleDB tick store, FastAPI/WebSocket layer and a live trading-desk dashboard, deployable to Kubernetes via Helm.

**Architecture:** Rust-led core (ingestor → Kafka → market engine ⇄ gRPC ⇄ matching engine); Python FastAPI exposes REST + WS fan-out; TimescaleDB persists trades/candles; React dashboard renders live charts; end-to-end latency measured ingest→render via t0 envelope timestamps.

**Tech Stack:** Rust stable ≥1.82 workspace (tokio 1.x, tokio-tungstenite 0.24, rdkafka 0.36, tonic 0.12 + prost, dashmap, serde, thiserror, tracing, criterion) · Python 3.11+ FastAPI + SQLAlchemy async + asyncpg + aiokafka · PostgreSQL 16 + TimescaleDB · React 18 + Vite 5 + TS + lightweight-charts 4 + TailwindCSS · k6 · Helm 3 / kind · GitHub Actions.

## Global Constraints

- One-command demo contract: fresh clone → `docker compose up` → dashboard :5173, API :8000, Grafana :3001. Kafka runs KRaft single-node (`apache/kafka:3.9.0`), no Zookeeper.
- All market payloads carry `t0_unix_ms` (set at WS receive) enabling the ingest→render latency report.
- Order book keys use scaled integers (price×1e8) — float map keys forbidden.
- Money/qty for trading math uses f64 quantities but PnL persisted as numeric(20,8) in PG.
- No exchange API key required (public market streams only).
- Docs in English; conventional commits with crate scopes (`feat(ingestor):`, `feat(engine):`…); TDD per task.
- Target repo: new repo at `~/Documents/Desarrollo/tickforge` (Task 1 creates it).

---

## Phase T1 — Ingestion: Binance WS → Kafka

### Task 1: Workspace scaffold + compose infra + metrics endpoint

**Files:**
- Create: root `Cargo.toml` (workspace members crates/*), `crates/{market-types,ingestor,engine,matching}/Cargo.toml` (+src/lib.rs or main.rs stubs), `proto/market/v1/market.proto`, `docker-compose.yml`, `Makefile`, `.github/workflows/ci.yml`
- Test: `crates/ingestor/src/main.rs` smoke `/metrics`

**Interfaces:**
- Produces: workspace `tickforge-*` crates; compose services kafka(timescale later) healthy; axum `/metrics` on :9100 in ingestor returning prometheus text.

```toml
# root Cargo.toml
[workspace]
resolver = "2"
members = ["crates/market-types","crates/ingestor","crates/engine","crates/matching"]

[workspace.dependencies]
tokio = { version = "1", features = ["full"] }
tonic = "0.12"; prost = "0.13"; serde = { version="1", features=["derive"] }
serde_json = "1"; rdkafka = { version="0.36", features=["cmake-build"] }
dashmap = "6"; thiserror = "1"; tracing = "0.1"
tracing-subscriber = { version="0.3", features=["env-filter"] }
axum = "0.8"; prometheus = "0.13"
tokio-tungstenite = { version="0.24", features=["rustls-tls-webpki-roots"] }
```

compose: kafka env `KAFKA_NODE_ID=1, KAFKA_PROCESS_ROLES=broker,controller, KAFKA_LISTENERS=PLAINTEXT://:9092,CONTROLLER://:9093, KAFKA_CONTROLLER_QUORUM_VOTERS=1@kafka:9093, KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1`; healthcheck `kafka-broker-api-versions --bootstrap-server localhost:9092`.

- [ ] Steps: scaffold files → failing test hitting `/metrics` via reqwest after spawning server (tokio test) → minimal axum server with prometheus default registry → `cargo test -p tickforge-ingestor` PASS → `docker compose up -d kafka && docker compose ps` healthy → commit `feat(scaffold): rust workspace, kafka kraft and metrics`.

### Task 2: Normalized market types + fixture-driven normalization tests

**Files:**
- Create: `crates/market-types/src/lib.rs`, `crates/ingestor/tests/fixtures/{trade.json,depth.json}`
- Test: `crates/market-types/tests/normalize.rs`

**Interfaces:**
- Produces:

```rust
pub struct Envelope<T> { pub t0_unix_ms: i64, pub payload: T }
#[derive(Clone, Copy)] pub enum TradeSide { Buy, Sell }
pub struct Trade { pub symbol: String, pub price: f64, pub qty: f64,
                   pub side: TradeSide, pub ts_exchange_ms: i64 }
pub struct BookDiff { pub symbol: String, pub first_update_id: i64,
    pub final_update_id: i64, pub prev_final_update_id: i64,
    pub bids: Vec<(f64,f64)>, pub asks: Vec<(f64,f64)> } // qty==0 → delete level
pub fn parse_trade(symbol:&str, binance_json:&str) -> Result<Trade, ParseError>;
pub fn parse_depth(symbol:&str, binance_json:&str) -> Result<BookDiff, ParseError>;
```

Binance trade JSON fields used: `{"p":price,"q":qty,"T":ts,"m":isBuyerMaker}` (m=true ⇒ side Sell). Depth partial stream `{"u":final,"U":first,"pu":prevFinal,"b":[[p,q]..],"a":[[..]]}`.

- [ ] Step 1: failing tests parsing both fixtures asserting exact field mapping incl. side inversion and pu propagation.
- [ ] Step 2–4: implement with serde `rename` attrs → PASS.
- [ ] Step 5: commit `feat(types): normalized trade and book diff with fixtures`.

### Task 3: Stream URL builder + reconnect backoff state machine

**Files:**
- Create: `crates/ingestor/src/{stream_url.rs,reconnect.rs}`
- Test: unit tests inside each module

**Interfaces:**
- Produces: `build_stream_url(symbols:&[&str]) -> String` → `wss://stream.binance.com:9443/stream?streams=btcusdt@trade/btcusdt@depth20@100ms` (lowercased, joined by `/`); `Backoff{attempt:u32}::next(&mut self)->Duration` = min(250ms·2^n,30s)+jitter(0..100ms), `reset()` on successful message.

- [ ] Steps: table-driven URL tests (single, multi, uppercase input) + backoff sequence assertions (250→500→…→30000 cap, jitter bounded, reset returns 250ms) → implement → PASS → commit `feat(ingestor): url builder and jittered backoff`.

### Task 4: Gap detection

**Files:**
- Create: `crates/ingestor/src/gap.rs`
- Test: `crates/ingestor/tests/gap.rs`

**Interfaces:**
- Produces: `pub struct BookSequencer{last_final:i64}` with `check(&mut self, d:&BookDiff) -> Result<(), GapDetected>` where ok requires `d.prev_final_update_id == self.last_final` (first diff accepted when last_final==0), then updates last; `GapDetected{expected:i64,got:i64}` error carrying context for metric+resync log.

- [ ] Steps: failing tests (happy chain, first-diff, out-of-order drop, gap after simulated loss) → implement → PASS → commit `feat(ingestor): book gap sequencer`.

### Task 5: Kafka producer wrapper (envelope publish)

**Files:**
- Create: `crates/ingestor/src/publisher.rs`
- Test: `crates/ingestor/tests/publisher.rs` (asserts against embedded mock: trait abstraction)

**Interfaces:**
- Produces: `#[async_trait] pub trait Publish { async fn send(&self, topic:&str, key:&str, payload_json:String) -> Result<()> }`; `KafkaPublisher::new(brokers)` FutureProducer acks=all idempotence; topics `tf.trades.{sym}`, `tf.book.{sym}`; golden test serializing sample Envelope<Trade> and comparing to recorded JSON string.

- [ ] Steps: failing golden-json + topic-naming tests → implement wrapper (FutureRecord timeout 5s, delivery guarantee logged) → PASS → commit `feat(ingestor): idempotent kafka publisher`.

### Task 6: Live ingest loop wiring

**Files:**
- Modify: `crates/ingestor/src/main.rs` (orchestrates url→connect→read→parse→sequence→publish; counters increment)
- Test: integration `crates/ingestor/tests/live_smoke.rs` gated by env `TF_LIVE=1`

**Metrics:** `tf_messages_total{type}`, `tf_gaps_total`, `tf_reconnects_total`.

- [ ] Steps: wire loop with select! over ws read + shutdown signal; smoke test (ignored by default) connects BTCUSDT 15s asserting messages_total>100 and gaps_total==0 printed; run once locally with TF_LIVE=1 capture output into docs/benchmarks.md draft; commit `feat(ingestor): resilient live loop with metrics`.

### Task 7: Phase T1 exit check

- [ ] `scripts/soak.sh`: 60s live run → assert zero gaps in logs, print throughput msgs/s → paste summary into `docs/benchmarks.md §T1`. Compose now includes ingestor service (SYMBOLS=btcusdt,ethusdt) writing to kafka verified via `kafka-console-consumer --max-messages 10`. Commit `docs: T1 exit evidence`.

---

## Phase T2 — Market Engine: Books, Candles, Indicators (gRPC)

### Task 8: L2 order book apply + property invariants

**Files:**
- Create: `crates/engine/src/orderbook.rs`, `proto/market/v1/market.proto` (Depth/Candles/PlaceOrder/StreamMarket per spec §5 diagram)
- Test: `crates/engine/tests/orderbook_props.rs`

**Interfaces:**
- Produces:

```rust
pub struct OrderBook { bids: BTreeMap<Key,f64>, asks: BTreeMap<f64,f64> }
struct Key(u64); // price*1e8 wrapped Reverse for descending bids
impl OrderBook {
  pub fn apply(&mut self, side: Side, price: f64, qty: f64);
  pub fn best_bid(&self)->Option<(f64,f64)>; pub fn best_ask(&self)->Option<(f64,f64)>;
  pub fn depth(&self, n:usize)->(Vec<(f64,f64)>,Vec<(f64,f64)>);
}
```

Invariants tested with pseudo-random op sequences (fixed-seed xorshift in test): after every apply, if both sides non-empty then best_bid < best_ask; all qtys ≥ 0; delete of missing level is no-op; depth returns sorted descending bids / ascending asks.

- [ ] Steps: failing invariant harness → implement apply/upsert-delete with scaled keys → 10k-op fuzz PASS ×3 seeds → commit `feat(engine): scaled-key l2 orderbook with invariants`.

### Task 9: Book consumer feeding books from tf.book.* + resync

**Files:**
- Create: `crates/engine/src/book_feed.rs`
- Test: `crates/engine/tests/book_feed.rs` (embedded consumer against testcontainers kafka OR channel-injected diffs behind trait Source)

**Interfaces:**
- Produces: `BookFeed::on_diff(d:BookDiff)` applying via sequencer semantics; on GapDetected emits resync request (logs + clears book awaiting snapshot v1: full rebuild from next depth20 frame treated authoritative when `first_update_id==1` marker set by ingestor resync flag).

- [ ] Steps: failing tests (apply chain maintains best prices; gap clears book; recovery re-applies) → implement → PASS → commit `feat(engine): book feed with resync path`.

### Task 10: Candle aggregator state machine

**Files:**
- Create: `crates/engine/src/candles.rs`
- Test: `crates/engine/tests/candles.rs`

**Interfaces:**
- Produces: `pub struct Candle{open_ts:i64,o,h,l,c,v:f64,trades:u32}`; `CandleAggregator::new(interval_secs:u64)`; `fn on_trade(&mut self,t:&Trade)->Option<Candle>` — bucket = ts/interval; crossing boundary returns closed candle and opens new with current trade; skew window: ts older than current open−2s ignored (counter `skipped_old`).

- [ ] Steps: hand-computed boundary cases (exact multiple, mid-candle h/l updates, close==o single-trade, gap period leaves empty candle skipped not emitted, old-tick ignored) failing → implement → PASS → commit `feat(engine): incremental candles`.

### Task 11: Streaming indicators SMA + RSI(Wilder)

**Files:**
- Create: `crates/engine/src/indicators.rs`
- Test: `crates/engine/tests/indicators.rs`

**Interfaces:**
- Produces: `Sma::new(n)` pushing closes → Option<f64>; `RsiWilder::new(n=14)` with avg_gain/avg_loss smoothing α=1/n → Option<f64>.

- [ ] Steps: hand-computed sequences (e.g., closes 44.34.. series canonical RSI example asserting step values within 1e-4; SMA sliding window exactness incl. window fill boundary) failing → implement ring-buffer + running sums → PASS → commit `feat(engine): incremental sma and wilder rsi`.

### Task 12: gRPC surface + engine main loop

**Files:**
- Create: `crates/engine/build.rs`, `src/proto.rs`, `src/grpc.rs`, modify `main.rs`
- Test: `crates/engine/tests/grpc_api.rs` (in-process tonic server + client)

**Proto (verbatim):**

```protobuf
syntax = "proto3"; package market.v1;
service MarketService {
  rpc GetDepth(DepthRequest) returns (DepthResponse);
  rpc GetCandles(CandlesRequest) returns (CandlesResponse);
  rpc PlaceOrder(OrderRequest) returns (OrderAck);
  rpc StreamMarket(StreamRequest) returns (stream MarketEvent);
}
message DepthRequest{string symbol=1; int32 depth=2;}
message Level{double price=1; double qty=2;}
message DepthResponse{repeated Level bids=1; repeated Level asks=2;}
message CandlesRequest{string symbol=1; string interval=2; int32 limit=3;}
message CandleMsg{int64 open_ts=1; double o=2; double h=3; double l=4; double c=5; double v=6; int32 trades=7;}
message CandlesResponse{repeated CandleMsg candles=1;}
message OrderRequest{string order_id=1; string symbol=2; string side=3; string type=4; double quantity=5; double limit_price=6;}
message OrderAck{string order_id=1; string status=2;}
message StreamRequest{string symbol=1;}
message MarketEvent{string kind=1; string json=2;}
```

Engine consumes tf.trades.*/tf.book.* updating per-symbol books/candle maps/SMA-RSI states (interval set S1,M1,M5); keeps rolling VecDeque<Candle> (cap 720 per interval) serving GetCandles; PlaceOrder forwards to matching engine channel (Task 17) acking ACCEPTED; StreamMarket broadcasts JSON events via tokio::sync::broadcast (capacity 4096).

- [ ] Steps: build.rs prost compile; failing grpc tests: seed state via direct feed calls → GetDepth mirrors book.top(5); GetCandles returns closed+open candle; StreamMarket receives trade event pushed concurrently (timeout 1s) → implement loop (consumer group tf.engine) → PASS → clippy clean → commit `feat(engine): grpc queries and streaming`.

### Task 13: Criterion benches — book ops & candle throughput

**Files:**
- Create: `crates/engine/benches/book.rs`, extend `docs/benchmarks.md`

- [ ] Steps: bench apply() 20-level diff, depth(10), on_trade() hot loop; record measured ops/s + p99 into docs table with machine note; commit `perf(engine): criterion evidence`.

### Task 14: Phase T2 exit check

- [ ] compose adds engine consuming live ingestor output; scripted curl-grpc (grpcurl) GetDepth btcusdt shows plausible top-of-book while soak runs; screenshot grafana panel (add basic engine metrics exporter task here if trivial: prometheus crate counters already exposed on :9101). Commit `docs: T2 exit evidence`.

---

## Phase T3 — Event-Sourced Paper-Trading Matching Engine

### Task 15: Market orders immediate fill vs last trade

**Files:**
- Create: `crates/matching/src/{lib.rs,intents.rs}`
- Test: `crates/matching/tests/market_fills.rs`

**Interfaces:**
- Produces: `OrderIntent{order_id,symbol,side,type:MARKET|LIMIT,quantity:f64,limit_price:Option<f64>}`; `Fill{order_id,qty,price}`; `MatchingEngine::on_trade(t:&Trade)` updates last price + attempts resting limits; `on_intent(i:OrderIntent) -> Vec<Event>` where MARKET fills fully at last known price (error event NO_MARKET_PRICE if unknown).

- [ ] Steps: failing tests (full fill, zero-qty reject, unknown symbol accepted but parked until first trade — document choice) → implement → PASS → commit `feat(matching): market order fills`.

### Task 16: Limit orders, partial fills, cancel

**Files:**
- Test: `crates/matching/tests/limit_flow.rs`

**Rules:** LIMIT BUY rests while trade.price > limit; fills greedily each on_trade where price ≤ limit consuming min(remaining, trade.qty) events per trade; symmetric sells ≥ limit; `cancel(order_id)` closes remainder with Cancelled event.

- [ ] Steps: scenario tests (rest→multiple partial fills→complete; never crosses wrong direction; cancel mid-way leaves filled part intact) → implement → PASS → commit `feat(matching): limit lifecycle with partials`.

### Task 17: Positions & PnL (avg-cost) + event log/snapshots

**Files:**
- Create: `crates/matching/src/portfolio.rs`, `events.rs`
- Test: `crates/matching/tests/pnl_and_events.rs`

**Interfaces:**
- Produces: `Position{qty,avg_cost,realized_pnl}` updated on fills (increase: new avg cost; decrease: realized += (exit−avg)·qty); unrealized computed vs last price on demand; `EventLog` appends typed events (Accepted/Fill/Cancelled/Rejected{reason}) with monotonic seq; `state_hash()` FNV-1a over canonical JSON of positions+open orders; snapshot every 1000 events to stdout/PG hook (trait SnapshotSink).

- [ ] Steps: hand-computed PnL scenarios (buy 1@100, buy 1@110 avg=105; sell 1@120 realized=15; short side mirrored; fee-free v1 noted) + replay determinism test: fixed fixture intent/trade stream → two fresh engines → identical seq+state_hash → implement → PASS → commit `feat(matching): pnl accounting and deterministic event sourcing`.

### Task 18: Kafka event publishing + PlaceOrder bridge

**Files:**
- Modify: matching main binary `crates/matching/src/main.rs` (consume intents topic tf.orders.intents produced by API; publish events to tf.orders.events keyed order_id)
- Test: integration `crates/matching/tests/kafka_roundtrip.rs` (testcontainers kafka, send intent → read event)

- [ ] Steps: failing roundtrip test → rdkafka consumer/producer pair with at-least-once handling (event dedup by (order_id,seq)) → PASS → commit `feat(matching): kafka-backed event pipeline`.

### Task 19: Phase T3 exit check

- [ ] Scripted demo: grpcurl PlaceOrder market buy 0.001 BTC → kafka-console-consumer tf.orders.events shows Accepted+Fill; replay script rebuilds identical hash (printed twice equal). Screenshots + commit `docs: T3 exit evidence`.

---

## Phase T4 — TimescaleDB Store + FastAPI Service Layer

### Task 20: Timescale schema + continuous aggregates

**Files:**
- Create: `services/api/migrations/0001_timescale.sql`, applied by init container
- Test: `tests/integration/test_candles_agg.py` (pytest + testcontainers timescale image)

**SQL (verbatim core):**

```sql
CREATE TABLE trades(time timestamptz NOT NULL, symbol TEXT NOT NULL,
  price NUMERIC(20,8) NOT NULL, qty NUMERIC(20,8) NOT NULL, side TEXT NOT NULL);
SELECT create_hypertable('trades','time', chunk_time_interval => INTERVAL '1 day');
CREATE MATERIALIZED VIEW candles_1m WITH (timescaledb.continuous) AS
SELECT time_bucket('1 minute', time) AS bucket, symbol,
       first(price,time) AS o, max(price) AS h, min(price) AS l,
       last(price,time) AS c, sum(qty) AS v, count(*) AS trades
FROM trades GROUP BY bucket, symbol WITH DATA;
ALTER TABLE trades SET (timescaledb.compress,
  timescaledb.compress_orderby='time DESC', timescaledb.compress_segmentby='symbol');
SELECT add_compression_policy('trades', INTERVAL '7 days');
SELECT add_retention_policy('trades', INTERVAL '30 days');
```

- [ ] Steps: failing test inserting 90 minutes of synthetic trades then `refresh_continuous_aggregate` → query candles_1m returns 90 rows with o/h/l/c correct vs computed expectation → PASS → compose wires api DB to this instance → commit `feat(store): hypertable and 1m continuous aggregate`.

### Task 21: FastAPI scaffold — auth + paper accounts

**Files:**
- Create: `services/api/pyproject.toml`, `services/api/tickforge_api/{main.py,settings.py,db.py,auth.py,accounts.py}`, migrations users/paper_accounts
- Test: `services/api/tests/test_auth_accounts.py` (httpx ASGI + testcontainers PG reuse pattern from ledgercore plan Task 3)

**Endpoints:** `POST /auth/register|login` (JWT HS256); `POST /paper/accounts` → `{id,balance_usdt:"10000.00"}` starting virtual balance; `GET /paper/accounts`.

- [ ] Steps: failing tests → implement mirroring LedgerCore auth patterns (reuse code shape, independent copy — repos are separate products) → PASS → commit `feat(api): auth and paper accounts`.

### Task 22: Order placement endpoint → matching bridge

**Files:**
- Create: `services/api/tickforge_api/orders.py` (+grpc stub gen from shared proto via grpcio-tools), `orders` table migration
- Test: `services/api/tests/test_place_order.py` (engine gRPC faked in-process server implementing proto)

**Behavior:** `POST /orders {symbol,side,type,quantity,limit_price?}` validates symbol ∈ env `TF_SYMBOLS` whitelist, qty>0, generates server-side order_id uuid → tonic PlaceOrder timeout 200ms → persist row status ACCEPTED → return 201; engine unreachable → 503.

- [ ] Steps: failing tests (whitelist 400, happy 201 persisted, engine-down 503) → implement grpc.aio stub call → PASS → commit `feat(api): validated order placement`.

### Task 23: GET /candles + trades history from store

- [ ] Steps: failing test querying agg through endpoint `GET /candles?symbol=btcusdt&interval=1m&limit=60` shaped `{candles:[{t,o,h,l,c,v}]}` → implement SQL over candles_1m (fallback bucket query for sub-minute live candle appended from engine cache optional OFF v1) → PASS → commit `feat(api): candle history endpoint`.

### Task 24: WebSocket fan-out hub (drop-oldest)

**Files:**
- Create: `services/api/tickforge_api/ws.py` (aiokafka consumer of engine StreamMarket relay via internal redis-pubsub OR direct grpc streaming client task)
- Test: `services/api/tests/test_ws_hub.py`

**Contract:** `WS /ws/stream?symbol=btcusdt`; hub keeps asyncio.Queue(capacity 256) per connection; overflow drops OLDEST and increments `ws_dropped_total{client}`; messages are raw MarketEvent JSON passthrough.

- [ ] Steps: failing tests (two clients receive published event; flood 1000 msgs slow reader → dropped counter rises, connection alive) → implement broadcast registry + grpc stream consumer task → PASS → commit `feat(api): resilient ws fan-out`.

### Task 25: Phase T4 exit check

- [ ] e2e python script `scripts/demo_t4.py`: register → account → place market buy while soak running → poll order status → fetch candles non-empty → ws client receives ≥5 events in 10s. Output transcript saved. Commit `docs: T4 exit evidence`.

---

## Phase T5 — Trading Desk Dashboard

### Task 26: Vite scaffold + WS hook with reconnect

**Files:**
- Create: `web/` (vite react-ts + tailwind), `web/src/hooks/useMarketStream.ts`
- Test: `web/src/hooks/useMarketStream.test.ts` (vitest + mocked WebSocket)

**Hook contract:** auto-reconnect exp backoff 500ms→5s; parses MarketEvent JSON; exposes `{candles, lastTrade, connected}`; caps candle buffer 720 shifting oldest.

- [ ] Steps: failing tests (connect, event appends candle update, reconnect after close, buffer cap) → implement → PASS → commit `feat(web): market stream hook`.

### Task 27: Watchlist strip + price flash

**Files:** `web/src/components/{Watchlist.tsx,PriceCell.tsx}` + tests
- [ ] Behavior: symbols from env list; PriceCell flashes bg green/red comparing previous vs next price (class toggle 600ms timeout). Failing render tests → implement → commit `feat(web): watchlist`.

### Task 28: Live candlestick chart (lightweight-charts)

**Files:** `web/src/components/PriceChart.tsx` + test
- [ ] Steps: failing test mounting chart with fake series data asserting series count + update on new candle event (lightweight-charts mocked thin wrapper `chartApi.ts` for testability) → implement subscribe to hook → commit `feat(web): live candles chart`.

### Task 29: Depth ladder + ticket + positions panel

**Files:** `web/src/components/{DepthLadder,TicketForm,PositionsPanel}.tsx` + tests
- [ ] DepthLadder polls `GET` grpc-proxy `POST /api/depth` (api adds passthrough route calling GetDepth) every 1s rendering horizontal bars; TicketForm posts orders reusing idempotent-attempt uuid pattern (same UX rule as LedgerCore); PositionsPanel consumes Fill events → `GET /positions`.
- [ ] Steps: component tests each → implement → commit `feat(web): depth, ticket, positions`.

### Task 30: nginx container + Playwright live e2e + exit

**Files:** `web/Dockerfile` (multi-stage node build→nginx proxy `/api`,`/ws`), `web/e2e/desk.spec.ts`
- [ ] e2e: open desk with compose stack + soak running → expect chart data revision change within 3s → place market buy 0.001 → PositionsPanel row appears ≤5s → screenshot `docs/screenshots/t5-desk.png`. Commit `e2e: live desk journey` + `docs: T5 exit`.

---

## Phase T6 — Kubernetes, Latency Report, MCP, Release

### Task 31: Helm chart

**Files:**
- Create: `deploy/helm/tickforge/{Chart.yaml,values.yaml,templates/{ingestor,engine,api,web}.{yaml,_hook.yaml},templates/_helpers.tpl}`

values toggles: `kafka.external:false` (else in-cluster bitnami dep off by default), images from GHCR, resources requests/limits, liveness `/healthz` (axum endpoints added where missing), web ingress disabled default.
- [ ] Steps: `helm lint` + `helm template` snapshot test (golden files committed) → kind cluster `scripts/kind-up.sh` (config with extraPortMappings) → `helm install` → all pods ready → port-forward smoke GetDepth → screenshots → commit `deploy: helm chart verified on kind`.

### Task 32: Latency harness ingest→render

**Files:**
- Create: `web/src/lib/latency.ts` (stamps `performance.now()-t0_unix_ms` per rendered trade/candle event, batches POST `/internal/latency`), `services/api/tickforge_api/internal.py` (collect rows into table latency_samples), `scripts/latency-report.sh` (10min soak with headless browser session, then SQL percentiles → markdown table appended docs/latency.md)

- [ ] Steps: failing percentile computation unit test → implement collect+report → run once, record p50/p95/p99 real numbers (budget p99<1500ms network-bound; engine-internal µs documented separately) → commit `perf: end-to-end latency report`.

### Task 33: MCP server for TickForge

**Files:**
- Create: `services/api/tickforge_api/mcp_server.py`
**Tools:** `get_order_book(symbol,depth)->dict` (via engine grpc), `get_candles(symbol,interval,limit)->list`, `place_paper_order(order_id,symbol,side,type,quantity,limit_price=None)->dict`, `get_positions(account_id)->list`.
- [ ] Steps: function-level failing tests → thin wrappers over existing services → stdio server runnable → transcript `docs/mcp-demo.txt` → commit `feat(mcp): agent tools surface`.

### Task 34: ADRs, AGENTS.md, consolidated benchmarks, release

**Files:** `docs/adr/0001-rust-workspace-split.md` … `0008-mcp-surface.md` (workspace split; kafka topic topology+t0 envelopes; scaled-int book keys; event-sourced matching w/ snapshots; cont-agg vs query-time bucketing; ws drop-oldest policy; helm-first deployment; mcp surface), `AGENTS.md`, README final diagrams + benchmark/latency tables, CHANGELOG.
- [ ] Steps: ADRs written ≤60 lines each; `scripts/demo_all.sh` chains T1/T2/T4/T5 exit scripts from clean `docker compose down -v && up`; CI green matrix (fmt+clippy+cargo test, pytest, vitest, playwright); `git tag -a v1.0.0 -m "TickForge v1.0.0"`; push tags.

## Self-Review Checklist (executed during execution)

Spec §5 DoD mapping: FLAT-SIMD replaced by production L2 book (spec allows engine evolution; SIMD micro-opt deferred ADR-noted) ✓benchmarks(T13) ✓grpc(T12) ✓metadata filtering folded into symbol whitelisting v1 (ADR noted) ✓scoring compuesto deferred to recommendations epic (out of MVP scope, tracked) ✓React UI(T26-30) ✓ADR set(T34) ✓rust tests throughout. Latency doc ✓(T32). K8s ✓(T31). MCP ✓(T33).
