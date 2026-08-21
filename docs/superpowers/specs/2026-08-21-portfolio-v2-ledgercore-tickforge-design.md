# Portfolio v2 Design — LedgerCore + TickForge

> **Status:** Approved · **Date:** 2026-08-21 · **Supersedes:** 6-project monthly roadmap (May–Oct 2026)

---

## 1. Context & Goals

The original portfolio planned 6 monthly megaprojects. This redesign replaces it with **2 production-grade systems** built to depth, targeting **Backend Senior / Architect roles with Python or Rust in Switzerland and Europe**.

**Goals**

- Demonstrate sustained engineering: architecture design, ADR-driven decisions, performance work, real infrastructure, and maintenance of complex systems.
- Cover the exact skill matrix extracted from the 7 target job offers.
- Make every project visually demonstrable (frontend) and runnable by anyone with one command.

## 2. Market Analysis (7 Target Offers — Final Set)

Offers analyzed: Fusion Consulting (Rust, CH) · Keyrock HFT (Rust, EU) · Lansweeper (Go/Rust, ES) · Logicalis (Python, ES) · FinTech FastAPI (Python, US hours) · Hedge Fund Lugano (Rust, CH) · ERNI Schweiz (Rust, CH).

**Discarded as targets:** Linro (Go-only stack) · Revolut Graduate (graduate-program eligibility mismatch) · Nomic Foundation (EVM-specialization outside portfolio scope).

| Requirement | Offers demanding it |
|---|---|
| Rust | Fusion, Keyrock, Lugano Fund, Lansweeper, ERNI (5/7) |
| Python / FastAPI | Keyrock (tooling), Logicalis, FinTech FastAPI, Lugano Fund (research tooling) (4/7) |
| Rust + Python combo | Keyrock, Lugano Fund, Fusion ("polyglot") (3/7) |
| Distributed systems | Keyrock, Lugano Fund, Lansweeper, Logicalis, ERNI (5/7) |
| Kafka / event-driven | ERNI explicit; event-driven patterns implied by trading/data-platform roles (1/7+) |
| Deep SQL / PostgreSQL | Fusion ("relational data stores"), FinTech FastAPI (2/7) |
| Data pipelines / ETL | Fusion (Glue), Lansweeper (core), Logicalis (PySpark), Keyrock (market data) (4/7) |
| AWS / K8s / Docker infra | Keyrock, Lansweeper, Logicalis, ERNI, Fusion (5/7) |
| Low latency / performance | Keyrock, Lugano Fund, Lansweeper (3/7) |
| AI integration + agentic engineering (LLMs, MCP) | Keyrock ("LLMs, MCP APIs"), Lansweeper (AI-native, MCP workflows) — 2/7 explicit; Logicalis AI/ML soft, Lugano ML nice-to-have |
| Trading / fintech domain | Keyrock, Lugano Fund, FinTech FastAPI (3/7) |
| TDD / DDD / testing culture | ERNI (DDD/TDD/BDD), Fusion, Keyrock, Lansweeper (4/7) |
| GitHub visibility reviewed | Lansweeper ("active engineering practice — we will look") (1/7) |

**Key insights**

1. The **Rust + Python polyglot combo** is the market sweet spot for this profile.
2. **Trading/market-data infrastructure maps directly to Swiss hubs** (Zurich/Zug/Lugano; Keyrock + prediction-market fund).
3. **Agentic engineering (LLM/MCP-assisted development)** appears explicitly in the two most senior offers (Keyrock, Lansweeper).
4. **Data pipelines** appear in 4/7 offers.
5. Non-portfolio blockers tracked separately: ERNI requires professional German; FinTech FastAPI requires US business-hours overlap.

## 3. Decision Log

| # | Decision | Choice | Rationale |
|---|---|---|---|
| 1 | Number of projects | 2 megaprojects | Depth over breadth; matches senior narrative |
| 2 | Origin | Designed from scratch | New domains distinct from archived roadmap |
| 3 | Domains | Fintech payments + Market data/trading | Highest demand across analyzed offers; Swiss alignment |
| 4 | Target roles | Backend Senior / Architect (Python or Rust) | Per user goal |
| 5 | Timeline | Unbounded; scope by phases | Quality-first |
| 6 | Code location | Separate public repos (`ledgercore`, `tickforge`); this repo = specs hub | Better GitHub showcase |
| 7 | Frontend | React + Vite (SPA) both projects | Demo vehicle only; backend-first roles |
| 8 | Language strategy | Polyglot both, dominant language differs | LedgerCore ≈70% Python / 30% Rust; TickForge ≈65% Rust / 35% Python. Every repo alone shows the full combo |
| 9 | Runtime contract | **One-command demo**: `docker compose up` boots everything incl. frontend | Hard requirement from user |
| 10 | Build order | Incremental phases, each ending demoable | Risk reduction |
| 11 | Target offer set finalized at 7 | Discarded Linro (Go-only), Revolut Graduate (eligibility), Nomic (EVM specialization) | Focus documentation and evidence on achievable targets |
| 12 | UI i18n EN/ES/DE in both dashboards | react-i18next, `en` default, German formal register, key-parity tested, Intl formatting | ERNI requires German; Spanish serves LATAM network; English is EU default — trilingual UI is interview evidence for CH market |

## 4. Project 1 — LedgerCore 💳 Payments Platform

**Pitch.** Multi-currency wallet platform: instant internal transfers with strict ACID double-entry ledger, idempotent payment API, sub-millisecond risk engine in Rust, async LLM fraud analysis, immutable audit trail, and a full banking dashboard.

### Architecture

```
React+Vite ──REST──▶ FastAPI API (auth JWT, accounts, payments)
                      │ Idempotency-Key middleware
                      ├──gRPC sync──▶ Risk Engine (Rust/Axum+tonic)
                      │                velocity windows, limits,
                      │                blacklists → <5ms p99
                      ▼
                PostgreSQL (double-entry ledger, BIGINT minor units)
                      │ Transactional Outbox
                      ▼
                   Kafka ──▶ Notifications (SSE to dashboard)
                         ──▶ Fraud Scanner (deep LLM analysis)
                         ──▶ Audit Projector
```

- **Language split:** Python-led (~70%): API, orchestration, consumers, LLM fraud analysis. Rust (~30%): risk engine on the synchronous hot path.
- **Data model (core tables):** `accounts`, `ledger_entries` (immutable, paired debit/credit), `payments` (unique `idempotency_key`), `outbox_events`, `fraud_verdicts`. Money as BIGINT minor units — never floats.

### Hard problems showcased

1. Concurrent transfers without lost updates: row-level locking; k6 with 1000 concurrent transfers → zero negative balances, zero money created/destroyed.
2. Exactly-once at API level: idempotency keys (Redis fast-path + DB unique constraint safety net).
3. Transactional outbox → Kafka: eliminates dual-write problem.
4. Property-based invariant testing: `sum(debits) == sum(credits)` under concurrency (Hypothesis).
5. Rust risk engine benchmarked: <5ms p99 documented with Criterion + k6.

### Stack

FastAPI · SQLAlchemy async · Alembic · PostgreSQL · Redis · Kafka · Rust/Axum + tonic (gRPC) · React + Vite · OpenTelemetry · Prometheus/Grafana · pytest + Testcontainers · k6 · Hypothesis · Docker multi-stage (distroless) · GitHub Actions.

## 5. Project 2 — TickForge 📈 Market Data & Paper-Trading Engine

**Pitch.** Real-time crypto market platform: ingests public exchange WebSocket streams (Binance), maintains in-memory L2 order books in Rust, computes candles and indicators incrementally, persists ticks in TimescaleDB, and lets users paper-trade against live prices through an event-sourced matching engine — visualized live in a trading-desk dashboard.

### Architecture

```
Binance WS ──▶ Ingestor (Rust/Tokio) ──▶ Kafka (trades, book updates)
               reconnect, backpressure,      │
               sequence-gap detection        ▼
                                     Market Engine (Rust)
                                     in-memory L2 books,
                                     incremental candles 1s/1m/5m,
                                     streaming indicators
                                            │ gRPC
             FastAPI ◀───────────────────────┘
               │ auth, paper accounts, orders,
               │ history (TimescaleDB), WebSocket fan-out
               ▼
         React+Vite: live watchlist, lightweight-charts,
         depth ladder, buy/sell ticket, positions & PnL
```

- **Language split:** Rust-led (~65%): ingestor, market engine, matching engine. Python (~35%): REST/WebSocket API layer, persistence queries.
- Matching engine is event-sourced to Kafka with deterministic replay; snapshots persisted.

### Hard problems showcased

1. Backpressure and reconnect handling without message loss (gap detection).
2. High-performance L2 order book: Criterion benchmarks at µs per insert/update.
3. Event-sourced matching engine with deterministic replay from Kafka.
4. End-to-end latency documentation: ingest→UI p50/p99 (headline interview chart).
5. TimescaleDB hypertables + continuous aggregates for candles + compression policies.
6. Kubernetes deployment story (Helm chart/raw manifests).

### Stack

Rust/Tokio · rdkafka · tonic · TimescaleDB · FastAPI · React + Vite + lightweight-charts · Prometheus/Grafana · k6 · Docker multi-stage (distroless) · GitHub Actions · Helm/K8s manifests.

## 6. Shared Methodology (Both Projects)

- **One-command demo contract:** `docker compose up` starts every service — Python API, Rust services, PostgreSQL/TimescaleDB, Redis, Kafka, Grafana/Prometheus, and the containerized frontend (Vite build served by nginx). No local toolchain needed beyond Docker. README states it first; CI verifies the stack comes up healthy.
- **Docs in English:** README, 10+ ADRs per project, C4/Mermaid diagrams, OpenAPI spec, benchmark reports, runbook.
- **Agentic engineering as a deliverable:** each project ships an **MCP server** exposing domain tools (`get_balance`, `place_paper_order`, …) plus repo-level `AGENTS.md` documenting the AI-assisted workflow used to build it — direct evidence for Keyrock/Lansweeper-style requirements (LLMs, MCP APIs, AI-native workflows).
- **TDD:** tests first on the hard problems; conventional commits; trunk-based flow with PRs.
- **CI/CD:** GitHub Actions — clippy/ruff lint → unit tests (cargo test / pytest) → integration tests (Testcontainers) → docker buildx multi-arch distroless → push GHCR → smoke test `docker compose up`.
- **Observability baseline:** OpenTelemetry traces crossing the Python↔gRPC↔Rust boundary, Prometheus metrics, provisioned Grafana dashboards-as-code.

## 7. Development Support Skills Installed

Installed globally via `npx skills add` to assist implementation:

vercel-react-best-practices (651K) · fastapi-templates (23K) · postgresql-table-design (24K) · rust-async-patterns (17K) · rust-best-practices/Apollo (16K) · helm-chart-scaffolding (10K) · rust-testing (8.4K) · rust-patterns (8K) · docker-compose-orchestration (2.6K) · react-vite-best-practices (2.2K) · kafka-development (972) · axum-web-framework (523) · grpc-service-development (504).

## 8. Build Phases

Each phase ends in something demoable.

### LedgerCore

| Phase | Scope | Exit criteria |
|---|---|---|
| **L1** | FastAPI skeleton, accounts + double-entry ledger, idempotency middleware, Alembic migrations, compose stack (PG+Redis+app) | Transfer API works; concurrent-transfer k6 test green; invariant property tests pass |
| **L2** | Rust risk engine (gRPC), velocity rules, benchmarks | Authorization path <5ms p99 documented; CI builds distroless image |
| **L3** | Kafka + transactional outbox; notification consumer; LLM fraud scanner consumer | Payment events reliable end-to-end; fraud verdicts stored |
| **L4** | React+Vite dashboard: login, balances, transfers, history, alerts panel; SSE live notifications; frontend containerized | Full flow demoable via `docker compose up` |
| **L5** | OpenTelemetry traces, Grafana dashboards, benchmark reports, MCP server, final ADRs/runbook | Docs complete; observability screenshots; v1.0 tag |

### TickForge

| Phase | Scope | Exit criteria |
|---|---|---|
| **T1** | Rust WS ingestor against Binance streams; normalization; Kafka publishing; gap detection | Sustained ingestion without loss under reconnects; metrics visible |
| **T2** | Market engine: L2 books, incremental candles, streaming indicators; gRPC queries; Criterion benches | Book ops µs-level benchmarked; candle correctness vs fixture stream |
| **T3** | Matching engine: paper orders, fills, positions/PnL; event-sourced replay | Deterministic replay test green |
| **T4** | FastAPI layer: auth, paper accounts, order placement, TimescaleDB tick/candle store | API e2e against live stream green |
| **T5** | Dashboard: watchlist, charts, depth ladder, ticket, PnL; WebSocket fan-out; containerized | Live trading desk demo via `docker compose up` |
| **T6** | Helm chart/k8s manifests, latency report ingest→UI, MCP server, final ADRs/runbook | K8s deploy verified; latency doc published; v1.0 tag |

## 9. Repository Restructure (this repo)

- Root `README.md` rewritten as the portfolio hub (English, recruiter-facing).
- Monthly folders (`1-mayo` … `6-octubre`) moved to `_archive/` — preserved as reference material (e.g., TimescaleDB SQL reused by TickForge).
- Specs for each project will seed their future dedicated repos.

## 10. Out of Scope

- Actual code lives in the future `ledgercore` and `tickforge` repos, not here.
- Mobile apps (Flutter) dropped: web dashboards cover the demo need.
