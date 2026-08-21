# LedgerCore Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build LedgerCore — a payments platform with an immutable double-entry ledger, idempotent payment API, a Rust gRPC risk engine (<5ms p99), transactional-outbox event pipeline with LLM fraud analysis, and a React banking dashboard, fully runnable with `docker compose up`.

**Architecture:** FastAPI owns correctness/orchestration; PostgreSQL holds the ledger with deterministic row-locking; Kafka receives events via the transactional outbox pattern; a Rust tonic service evaluates risk on the synchronous hot path; React+Vite dashboard consumes REST + SSE.

**Tech Stack:** Python 3.11+, FastAPI 0.115+, SQLAlchemy 2.0 async + asyncpg + Alembic, pydantic-settings · PostgreSQL 16 · Redis 7 · Kafka 3.9 (KRaft) · Rust stable + tonic 0.12 + tokio + dashmap · React 18 + Vite 5 + TypeScript + TailwindCSS · pytest + Testcontainers + Hypothesis + k6 · OpenTelemetry · GitHub Actions.

## Global Constraints

- Money is **BIGINT minor units** everywhere. Floats for money are forbidden.
- `ledger_entries` rows are immutable — no UPDATE/DELETE ever issued against them.
- Every ledger mutation happens inside exactly **one DB transaction** together with its `payments` row and `outbox_events` row.
- Idempotency-Key header required on `POST /payments`; replays return the stored response verbatim.
- Risk engine policy default `RISK_FAIL_MODE=closed` (unreachable engine denies).
- One-command demo contract: fresh clone → `docker compose up` → dashboard on :5173, API on :8000, Grafana on :3001. No local toolchain besides Docker.
- Docs in English; conventional commits; TDD red-green-refactor per task.
- **UI i18n (hard requirement):** three locales — `en` (default), `es`, `de`. No hardcoded user-facing strings anywhere in `web/`; locale persisted in `localStorage` (`lc_locale`) with browser-language detection fallback; every amount/date rendered through `Intl.NumberFormat`/`Intl.DateTimeFormat` using the active locale.
- Target repo: new repo at `~/Documents/Desarrollo/ledgercore` (created by Task 1).

---

## Phase L1 — Foundation: API + Double-Entry Ledger + Idempotency

### Task 1: Repository scaffold + Compose + health endpoint

**Files:**
- Create: `pyproject.toml`, `docker-compose.yml`, `.github/workflows/ci.yml`, `Makefile`, `src/ledgercore/__init__.py`, `src/ledgercore/main.py`, `tests/conftest.py`, `tests/integration/test_health.py`

**Interfaces:**
- Produces: `app = create_app()` FastAPI factory in `src/ledgercore/main.py`; `GET /health` → `{"status":"ok"}`.

- [ ] **Step 1: Create repo and project skeleton**

```bash
mkdir -p ~/Documents/Desarrollo/ledgercore && cd ~/Documents/Desarrollo/ledgercore && git init
mkdir -p src/ledgercore tests/integration .github/workflows
```

`pyproject.toml`:

```toml
[project]
name = "ledgercore"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
  "fastapi>=0.115", "uvicorn[standard]>=0.30",
  "sqlalchemy[asyncio]>=2.0", "asyncpg>=0.29",
  "alembic>=1.13", "pydantic-settings>=2.3",
  "redis>=5.0", "aiokafka>=0.10",
  "pyjwt>=2.8", "bcrypt>=4.1",
]

[dependency-groups]
dev = ["pytest>=8", "pytest-asyncio>=0.23", "httpx>=0.27",
       "testcontainers[postgres]>=4.7", "hypothesis>=6.100"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.pytest.ini_options]
asyncio_mode = "auto"
testpaths = ["tests"]
```

`src/ledgercore/main.py`:

```python
from fastapi import FastAPI

def create_app() -> FastAPI:
    app = FastAPI(title="LedgerCore")
    @app.get("/health")
    async def health() -> dict[str, str]:
        return {"status": "ok"}
    return app

app = create_app()
```

`docker-compose.yml`:

```yaml
services:
  postgres:
    image: postgres:16-alpine
    environment: { POSTGRES_USER: ledger, POSTGRES_PASSWORD: ledger, POSTGRES_DB: ledger }
    ports: ["5432:5432"]
    healthcheck: { test: ["CMD-SHELL","pg_isready -U ledger"], interval: 5s, retries: 10 }
  redis:
    image: redis:7-alpine
    ports: ["6379:6379"]
  api:
    build: { context: ., dockerfile: Dockerfile.api }
    env_file: .env.example
    ports: ["8000:8000"]
    depends_on:
      postgres: { condition: service_healthy }
      redis: { condition: service_started }
    volumes: ["./src:/app/src"]
    command: uvicorn ledgercore.main:app --host 0.0.0.0 --port 8000 --reload
```

`.env.example`: `LC_DATABASE_URL=postgresql+asyncpg://ledger:ledger@postgres:5432/ledger`, `LC_REDIS_URL=redis://redis:6379/0`, `LC_JWT_SECRET=dev-secret-change-me`, `LC_RISK_FAIL_MODE=closed`, `LC_ENABLE_DEV_ENDPOINTS=true`.

`Makefile`: `dev` (compose up), `test` (`uv run pytest -q`), `lint` (`ruff check src tests`), `migrate` (`alembic upgrade head`). Add `Dockerfile.api` (python:3.12-slim, install uv, sync deps, copy src). CI workflow: jobs `lint` (ruff) and `test` (uv sync + pytest) on push/PR.

- [ ] **Step 2: Write failing test** `tests/integration/test_health.py`

```python
import httpx
from ledgercore.main import create_app

async def test_health_ok():
    async with httpx.AsyncClient(
        transport=httpx.ASGITransport(app=create_app()), base_url="http://t"
    ) as c:
        r = await c.get("/health")
    assert r.status_code == 200
    assert r.json() == {"status": "ok"}
```

- [ ] **Step 3: Run** `uv sync && uv run pytest -q` → Expected: FAIL initially only if imports broken after Step 1 code present; if written after main.py it PASSES. Record result honestly; fix import errors until PASS.
- [ ] **Step 4: Verify compose boots:** `docker compose up -d postgres redis && curl -f localhost:5432 || true; docker compose ps` → postgres healthy.
- [ ] **Step 5: Commit**

```bash
git add -A && git commit -m "feat(api): scaffold fastapi app, compose stack and health endpoint"
```

### Task 2: Settings module

**Files:**
- Create: `src/ledgercore/settings.py`
- Test: `tests/unit/test_settings.py`

**Interfaces:**
- Produces: `Settings` pydantic BaseSettings fields `database_url: PostgresDsn`, `redis_url`, `jwt_secret: str`, `risk_fail_mode: Literal["open","closed"] = "closed"`, `enable_dev_endpoints: bool = False`; `get_settings() -> Settings` lru_cached.

- [ ] **Step 1: Failing test**

```python
def test_settings_reads_lc_prefix(monkeypatch):
    monkeypatch.setenv("LC_DATABASE_URL", "postgresql+asyncpg://u:p@h:1/d")
    monkeypatch.setenv("LC_JWT_SECRET", "s3cret")
    from ledgercore.settings import Settings
    s = Settings()
    assert str(s.database_url).startswith("postgresql+asyncpg://")
    assert s.risk_fail_mode == "closed"
```

- [ ] **Step 2: Run** `uv run pytest tests/unit/test_settings.py -q` → Expected: FAIL (module missing).
- [ ] **Step 3: Implement**

```python
from functools import lru_cache
from typing import Literal
from pydantic import PostgresDsn
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    model_config = SettingsConfigDict(env_prefix="LC_", env_file=".env", extra="ignore")
    database_url: PostgresDsn
    redis_url: str = "redis://localhost:6379/0"
    jwt_secret: str
    jwt_ttl_seconds: int = 3600
    risk_fail_mode: Literal["open", "closed"] = "closed"
    enable_dev_endpoints: bool = False

@lru_cache
def get_settings() -> Settings:
    return Settings()
```

- [ ] **Step 4: Run** → PASS.
- [ ] **Step 5:** `git add -A && git commit -m "feat(config): pydantic-settings with LC_ prefix"`

### Task 3: Async DB session factory

**Files:**
- Create: `src/ledgercore/db.py`
- Test: `tests/integration/conftest.py`, `tests/integration/test_db_session.py`

**Interfaces:**
- Produces: `engine(url)` -> AsyncEngine; `session_factory(engine)` -> async_sessionmaker[AsyncSession]; FastAPI dependency `get_session` yielding session with rollback-on-exception.

- [ ] **Step 1: Integration conftest using Testcontainers**

```python
import pytest
from testcontainers.postgres import PostgresContainer
from sqlalchemy.ext.asyncio import create_async_engine
from sqlalchemy.orm import sessionmaker
from sqlalchemy.ext.asyncio import AsyncSession

@pytest.fixture(scope="session")
def pg_url():
    with PostgresContainer("postgres:16-alpine") as pg:
        yield pg.get_connection_url().replace("psycopg2", "asyncpg")

@pytest.fixture
async def db_engine(pg_url):
    engine = create_async_engine(pg_url)
    yield engine
    await engine.dispose()

@pytest.fixture
async def session(db_engine):
    maker = sessionmaker(db_engine, class_=AsyncSession, expire_on_commit=False)
    async with maker() as s:
        yield s
```

- [ ] **Step 2: Failing test** `test_db_session.py`: `SELECT 1` via session returns row.
- [ ] **Step 3: Implement `db.py`**

```python
from collections.abc import AsyncIterator
from fastapi import Depends
from sqlalchemy.ext.asyncio import AsyncSession, async_sessionmaker, create_async_engine
from .settings import get_settings

def make_engine():
    return create_async_engine(str(get_settings().database_url), pool_pre_ping=True)

engine = None  # set in lifespan
SessionFactory: async_sessionmaker[AsyncSession] | None = None

def init_engine(e):  # called from lifespan
    global engine, SessionFactory
    engine = e
    SessionFactory = async_sessionmaker(e, expire_on_commit=False)

async def get_session() -> AsyncIterator[AsyncSession]:
    assert SessionFactory is not None
    async with SessionFactory() as s:
        try:
            yield s
            await s.commit()
        except Exception:
            await s.rollback()
            raise
```

Wire `create_app` lifespan to `init_engine(make_engine())`.
- [ ] **Step 4: Run integration tests** `uv run pytest tests/integration -q` → PASS.
- [ ] **Step 5:** `git commit -am "feat(db): async engine, session factory and request dependency"`

### Task 4: Alembic + initial migration (five tables)

**Files:**
- Create: `alembic.ini`, `alembic/env.py`, `alembic/versions/0001_core_tables.py`
- Test: `tests/integration/test_migrations.py`

**Interfaces:**
- Produces tables `accounts`, `ledger_entries`, `payments`, `idempotency_keys`, `outbox_events` exactly as Global Constraints schema (see spec §4).

- [ ] **Step 1: Init** `uv run alembic init alembic`; edit `env.py` to read `LC_DATABASE_URL`, `target_metadata=None` (raw SQL migrations), `sqlalchemy.url` from settings.
- [ ] **Step 2: Failing test**

```python
async def test_core_tables_exist(session):
    rows = await session.execute(
        "select table_name from information_schema.tables where table_schema='public'")
    names = {r[0] for r in rows}
    assert {"accounts","ledger_entries","payments","idempotency_keys","outbox_events"} <= names
```

(test fixture runs `alembic upgrade head` against container before yielding engine — add subprocess call in `db_engine` fixture.)
- [ ] **Step 3: Migration** `0001_core_tables.py` with `op.execute("""<DDL from spec §4 verbatim>""")` creating all five tables incl. checks (`direction in ('debit','credit')`, `amount_minor > 0`, payments status check), unique `payments.idempotency_key`, index `ledger_entries(account_id)`.
- [ ] **Step 4: Run** `uv run pytest tests/integration/test_migrations.py -q` → PASS; also `alembic downgrade base && alembic upgrade head` round-trip clean.
- [ ] **Step 5:** `git commit -am "feat(db): core tables migration"`

### Task 5: Accounts domain

**Files:**
- Create: `src/ledgercore/accounts/{models.py,schemas.py,repository.py,router.py}`
- Modify: `main.py` include router
- Test: `tests/integration/test_accounts_api.py`

**Interfaces:**
- Produces: `AccountModel(id, owner_id, currency, status, created_at)`; `AccountRepo.create(session, owner_id, currency) -> AccountModel`, `AccountRepo.get(session, account_id) -> AccountModel | None`; endpoints `POST /accounts {owner_id,currency}` → 201 body `{id,...}`; `GET /accounts/{id}` → 200/404.

- [ ] **Step 1: Failing API test** — register router; create account via httpx ASGI client with dependency-overridden session bound to Testcontainers engine (run migrations first); assert 201 then GET returns same id and currency `"USD"`.
- [ ] **Step 2: Run** → FAIL (router missing).
- [ ] **Step 3: Implement** models (SQLAlchemy `Mapped[...]` declarative matching DDL), schemas (`AccountCreate{owner_id:UUID,currency:Literal["USD","EUR"]}`, `AccountOut`), repository methods, router with status codes 201/404.
- [ ] **Step 4: Run** → PASS.
- [ ] **Step 5:** `git commit -am "feat(accounts): create/get accounts"`

### Task 6: Ledger posting + derived balance

**Files:**
- Create: `src/ledgercore/ledger/{models.py,repository.py}`
- Test: `tests/integration/test_ledger_repository.py`

**Interfaces:**
- Produces: `LedgerRepo.post_entry(session, *, payment_id|None, account_id, direction, amount_minor, currency, entry_type) -> LedgerEntryModel`; `LedgerRepo.balance(session, account_id) -> int` implemented as `SELECT coalesce(sum(CASE WHEN direction='credit' THEN amount_minor ELSE -amount_minor END),0)`.

- [ ] **Step 1: Failing test** — seed account via AccountRepo; post credit 10_000 and debit 4_000 (payment_id None); balance == 6_000; second debit of 99_999 does NOT raise here (balance check lives in transfer service); entries have created_at set.
- [ ] **Step 2: Run** → FAIL.
- [ ] **Step 3: Implement** both methods; `entry_type` free string (`deposit`,`transfer_out`,`transfer_in`).
- [ ] **Step 4: Run** → PASS.
- [ ] **Step 5:** `git commit -am "feat(ledger): immutable entries and derived balance"`

### Task 7: Transfer service with deterministic locking (concurrency-proof)

**Files:**
- Create: `src/ledgercore/ledger/service.py`, `src/ledgercore/errors.py`
- Test: `tests/integration/test_transfer_concurrency.py`

**Files (added):**
- Create: `src/ledgercore/payments/models.py`, `src/ledgercore/outbox/models.py` — minimal ORM rows plus `PaymentsRepo.create_pending / mark_completed` and `OutboxRepo.enqueue` used below (full payments API arrives in Task 9).

**Interfaces:**
- Produces: `class InsufficientFundsError(Exception)`; `execute_transfer(session, *, source_id, dest_id, amount_minor, currency) -> PaymentModel` performing locks + two entries + completed payments row + outbox row in ONE transaction.

- [ ] **Step 1: Failing concurrency test**

```python
async def test_parallel_transfers_never_corrupt(session_factory, funded_source, targets20):
    async def drain(i):
        async with session_factory() as s:
            await execute_transfer(s, source_id=funded_source.id,
                dest_id=targets20[i].id, amount_minor=500, currency="USD")
    await asyncio.gather(*(drain(i) for i in range(20)))
    total = await system_total(session_factory)  # sum over accounts of derived balance
    assert total == INITIAL_TOTAL  # money conserved; no negatives possible by construction
```

- [ ] **Step 2: Run** → FAIL (function missing).
- [ ] **Step 3: Implement**

```python
from sqlalchemy import text
from uuid import UUID

async def execute_transfer(session, *, source_id: UUID, dest_id: UUID,
                           amount_minor: int, currency: str):
    if amount_minor <= 0:
        raise ValueError("amount must be positive")
    ordered = sorted([source_id, dest_id])
    for aid in ordered:  # deterministic order prevents deadlocks
        await session.execute(
            text("SELECT id FROM accounts WHERE id=:i FOR UPDATE"), {"i": aid})
    bal = await LedgerRepo.balance(session, source_id)
    if bal < amount_minor:
        raise InsufficientFundsError(source_id)
    payment = await PaymentsRepo.create_pending(session, source_id, dest_id,
                                                amount_minor, currency)
    await LedgerRepo.post_entry(session, payment_id=payment.id, account_id=source_id,
                                direction="debit", amount_minor=amount_minor,
                                currency=currency, entry_type="transfer_out")
    await LedgerRepo.post_entry(session, payment_id=payment.id, account_id=dest_id,
                                direction="credit", amount_minor=amount_minor,
                                currency=currency, entry_type="transfer_in")
    await OutboxRepo.enqueue(session, topic="payments.completed",
                             key=str(payment.id), payload={"payment_id": str(payment.id),
                             "source": str(source_id), "dest": str(dest_id),
                             "amount_minor": amount_minor, "currency": currency})
    await PaymentsRepo.mark_completed(session, payment.id)
    return payment
```

- [ ] **Step 4: Run** → PASS (repeat 3× to catch flakes).
- [ ] **Step 5:** `git commit -am "feat(ledger): deadlock-safe transfers with money conservation proof"`

### Task 8: Property-based ledger invariants

**Files:**
- Test: `tests/property/test_ledger_invariants.py`

- [ ] **Step 1: Hypothesis test** — strategy generates 1–15 random valid ops (deposit/transfer within funds) against in-memory PG container state; invariant asserted every step: `sum(debits)==sum(credits)+external_deposits` and every account balance ≥ 0; shrink-friendly failure output prints op list.
- [ ] **Step 2: Run** `uv run pytest tests/property -q --hypothesis-seed=random` twice → PASS both times.
- [ ] **Step 3: Commit** `git commit -am "test(ledger): property-based conservation invariants"`

### Task 9: Payments API + idempotency semantics

**Files:**
- Create: `src/ledgercore/payments/{schemas.py,service.py,router.py}`, `src/ledgercore/idempotency/models.py`
- Modify: `main.py` (router)
- Test: `tests/integration/test_payments_api.py`

**Interfaces:**
- Produces: `POST /payments` headers `Idempotency-Key`, body `{source_account_id,destination_account_id,amount_minor,currency}`; responses: 200 replay (`X-Replayed: true`), 201 created, 409 key-reuse-with-different-body, 403 risk-denied (L2), 422 insufficient funds; `PaymentsService.create(...)` orchestrates.

- [ ] **Step 1: Failing tests** — three cases above; replay asserts identical JSON body AND no extra ledger rows counted before/after.
- [ ] **Step 2: Run** → FAIL.
- [ ] **Step 3: Implement flow**: begin tx → lookup key (hit+same hash → return stored; hit+different hash → 409) → validate accounts exist/currency match → execute_transfer → store `response_status=201,response_body` with SAME tx as transfer (atomicity) → return.
- [ ] **Step 4: Run** all L1 suite → PASS.
- [ ] **Step 4b: Redis fast-path (optional optimization, spec §4 DoD)** — best-effort `SETEX idem:{key} 86400 response_json` written after commit; on lookup try Redis first (hit → return immediately), DB remains source of truth on miss/conflict; unit test with fakeredis asserts hit path skips DB and stale entries fall back cleanly.
- [ ] **Step 5:** `git commit -am "feat(payments): idempotent payment endpoint"`

### Task 10: Auth (register/login JWT)

**Files:**
- Create: `src/ledgercore/auth/{security.py,service.py,router.py}`, table migration `0002_users.py`
- Test: `tests/integration/test_auth.py`

**Interfaces:**
- Produces: `hash_password/verify_password (bcrypt)`, `issue_token(user_id)->str`, `get_current_user` dependency reading `Authorization: Bearer`; `POST /auth/register`, `POST /auth/login` → `{access_token}`; accounts.owner_id now references users.id.

- [ ] **Steps:** failing tests (register→login→call protected `POST /accounts` without/with token expecting 401 then 201) → implement bcrypt cost 4 in dev config, HS256 TTL from settings → run → `git commit -am "feat(auth): jwt register/login and protected routes"`.

### Task 11: Makefile polish + CI green

**Files:** Modify: `Makefile`, `.github/workflows/ci.yml`
- [ ] Steps: `make lint test` local green; CI matrix python 3.11/3.12 running ruff+pytest (unit only on CI without docker: mark integration `@pytest.mark.integration`, CI runs `-m "not integration"`); push branch, PR green. Commit: `ci: github actions lint+unit`.

### Task 12: Phase L1 exit check

- [ ] `docker compose up -d && uv run alembic upgrade head` (or migrate job) then scripted happy path: register, login, create 2 accounts, deposit seed endpoint, transfer 500 minor units, GET balances reflect change; screenshot saved `docs/screenshots/l1-api-flow.png`. Tag nothing yet. Update README quickstart section. `git commit -am "docs: L1 exit runbook"`.

---

## Phase L2 — Rust Risk Engine (gRPC, <5ms p99)

### Task 13: Protobuf contract + Python stubs

**Files:**
- Create: `proto/risk/v1/risk.proto`, generation script `scripts/gen_stubs.sh`, generated `src/ledgercore/risk_pb2*` (grpcio-tools)
- Test: `tests/unit/test_risk_stubs.py` importing stubs and asserting message fields.

```protobuf
syntax = "proto3";
package risk.v1;
service RiskService { rpc Evaluate(EvaluateRequest) returns (EvaluateDecision); }
message EvaluateRequest { string payment_id=1; string source_account_id=2;
  string destination_account_id=3; int64 amount_minor=4; string currency=5; int64 now_unix_ms=6; }
message EvaluateDecision { bool allowed=1; repeated string reasons=2; int32 score=3; }
```

- [ ] Steps: write proto → generate (`python -m grpc_tools.protoc -I proto --python_out=src/ledgercore --grpc_python_out=src/ledgercore proto/risk/v1/risk.proto`) → failing import test → pass → `git commit -am "feat(risk): proto contract v1"`.

### Task 14: Rust crate scaffold + tonic echo server

**Files:**
- Create: `services/risk-engine/{Cargo.toml,build.rs,src/{main.rs,proto/risk.v1.rs}}`, root `Cargo.toml` workspace stub
- Test: `services/risk-engine/tests/grpc_echo.rs`

**Interfaces:**
- Produces: tonic server listening `0.0.0.0:50051`, `RiskService::Evaluate` echoing allowed=true score=0 (replaced in Task 16).

- [ ] Steps: cargo init with deps `tonic 0.12, prost 0.13, tokio(full), dashmap 6, thiserror, tracing`; build.rs compile_proto; failing tokio test spawning server on random port + tonic client Evaluate asserting ok; minimal echo handler; pass; `cargo clippy -- -D warnings` clean; commit `feat(risk): tonic scaffold echoes allow`.

### Task 15: Velocity sliding window (DashMap + VecDeque)

**Files:**
- Create: `services/risk-engine/src/velocity.rs`
- Test: `services/risk-engine/src/velocity.rs` `#[cfg(test)]` mod

**Interfaces:**
- Produces: `pub struct VelocityTracker{windows: DashMap<String, VecDeque<i64>>, window_ms: i64}` with `record(&self, key:&str, now_ms:i64) -> usize` returning count-in-window AFTER insert; internal prune of timestamps `< now-window_ms`.

- [ ] Step 1: failing tests — count grows 1..10 across pushes; old entries pruned when advancing `now_ms += window_ms+1`; distinct keys isolated; concurrent `record` from 8 std threads × 1000 increments totals exactly 8000 (dashmap shard safety).
- [ ] Step 2: run `cargo test -p risk-engine velocity` → FAIL → implement struct (per Global Constraint: pure in-memory) → PASS.
- [ ] Step 3: commit `feat(risk): sliding-window velocity tracker`.

### Task 16: Rules engine (pure functions) + decision wiring

**Files:**
- Create: `services/risk-engine/src/rules.rs`, modify `service.rs`
- Test: `rules.rs` table-driven unit tests; `tests/evaluate_flow.rs`

**Interfaces:**
- Produces: `pub struct LimitsCfg{max_txs_per_min:u32, max_amount_minor:i64, daily_cap_minor:i64}` + `evaluate(limits:&LimitsCfg, amount_minor:i64, txs_last_min:usize, txs_today:i64, now_ms:i64, day_start_ms:i64) -> Decision{allowed:bool, reasons:Vec<String>, score:i32}` (score = 40×tx_rate_ratio + 40×amount_ratio capped 100, deterministic); service handler composes tracker.record + evaluate; denied path returns allowed=false with reasons.

- [ ] Steps: failing rule tests (each limit tripped individually, none tripped, boundary equality trips) → implement pure fn (no I/O, trivially testable) → wire into tonic handler replacing echo → integration test: 11 rapid Evaluates same account → 11th denied reason `rate_limit` → clippy+pass → commit `feat(risk): configurable rules and scoring`.

### Task 17: Python client + fail-mode policies

**Files:**
- Create: `src/ledgercore/risk_client.py`; Modify: `payments/service.py`, `settings.py` (+`risk_endpoint:str="localhost:50051"`)
- Test: `tests/integration/test_risk_integration.py`, `tests/unit/test_fail_modes.py`

**Interfaces:**
- Produces: `async evaluate(payment_ctx) -> RiskOutcome{allowed, reasons, score}` calling grpc.aio channel with 50ms timeout; payments flow calls BEFORE execute_transfer; denial → payment failed + HTTP 403 `{"reasons":[...]}`; `RISK_FAIL_MODE=closed`: unreachable → deny(reason="engine_unreachable"); `open` → allow.

- [ ] Steps: failing tests with a fake gRPC server (spawn in-process grpc.aio server implementing proto in test) for allowed/denied/unreachable×two modes (6 cases) → implement client + branch in service (fail mode read from settings; closed default) → run full suite PASS → compose gains `risk-engine` build from `services/risk-engine/Dockerfile` (multi-stage rust:1.82-slim builder → distroless cc) → `docker compose up risk-engine` healthy → commit `feat(risk): hot-path evaluation with fail modes`.

### Task 18: Benchmarks — Criterion + k6 documented

**Files:**
- Create: `services/risk-engine/benches/eval.rs`, `scripts/load/risk.js`, `docs/benchmarks.md`
- Modify: Cargo.toml benches section

- [ ] Steps: criterion bench seeding 10k windows × 50 entries then measuring `evaluate` p99 (report actual number to docs; target <5ms p99 on dev laptop, else note hardware); k6 script POSTing to axum `/eval` wrapper (axum added behind feature `bench-http`) 500 VUs 30s capturing p95; write `docs/benchmarks.md` tables (criterion + k6 + machine specs) + ADR `docs/adr/0004-fail-closed-risk-policy.md`; commit `perf(risk): benchmark evidence and adr`.

### Task 19: Phase L2 exit check

- [ ] Scripted demo `scripts/demo_l2.sh`: compose up → payment denied when 11/min exceeded (curl shows 403 rate_limit), allowed under limits, engine down + closed mode → 403 engine_unreachable; outputs captured to `docs/screenshots/`. `make test` green. `git commit -am "docs: L2 exit runbook"`.

---

## Phase L3 — Transactional Outbox → Kafka + Consumers

### Task 20: Outbox relay worker (SKIP LOCKED batches)

**Files:**
- Create: `src/ledgercore/outbox/{models.py,relay.py}` (models may exist from 0001; add relay)
- Test: `tests/integration/test_relay.py` with Testcontainers Kafka

**Interfaces:**
- Produces: `async def run_relay(session_factory, producer, batch_size=100, poll_secs=0.5)` — loop: `SELECT * FROM outbox_events WHERE published_at IS NULL ORDER BY created_at LIMIT :n FOR UPDATE SKIP LOCKED` → aiokafka `send_and_wait(topic,key,payload_json)` → per-row `UPDATE ... SET published_at=now()` → commit batch; graceful shutdown on SIGTERM.

- [ ] Step 1: failing test — enqueue 3 events; fake producer records sends; run one iteration; all 3 marked published; second iteration sends none.
- [ ] Step 2–4: implement → PASS → wire relay as compose sidecar command `python -m ledgercore.outbox.relay` with real aiokafka producer (acks=all).
- [ ] Step 5: commit `feat(outbox): skip-locked relay publishing to kafka`.

### Task 21: Crash recovery / at-least-once proof

**Files:**
- Test: `tests/integration/test_relay_crash.py`

- [ ] Step 1: failing test — FlakyProducer raising after K sends; run_relay catches, commits only sent subset (rows already sent stay unpublished → will resend next loop); consumer-side dedup helper `seen(event_id) -> bool` backed by Redis SETNX TTL 24h; simulate consumer processing duplicated event asserting single effect.
- [ ] Steps: implement dedup util in `consumers/base.py`; PASS; commit `feat(outbox): crash-safe delivery with consumer dedup`.

### Task 22: Notifications SSE bus

**Files:**
- Create: `src/ledgercore/consumers/notifications.py`, `src/ledgercore/events/sse.py`
- Test: `tests/integration/test_sse_stream.py`

**Interfaces:**
- Produces: `GET /events/stream` (auth) → `text/event-stream`; events shaped `event: payment.completed\ndata:{json}` filtered by owner accounts; bus = dict[user_id, asyncio.Queue] with maxsize 100 drop-oldest.

- [ ] Steps: failing test subscribing two clients, publish payment.completed payload touching user's account → both receive within 1s; unrelated user receives nothing → implement consumer bridging Kafka → bus + SSE endpoint (StreamingResponse) → PASS → commit `feat(notifications): sse fan-out of payment events`.

### Task 23: Fraud scanner consumer (LLM)

**Files:**
- Create: `src/ledgercore/consumers/fraud.py`, migration `0003_fraud_verdicts.py` (`fraud_verdicts(payment_id uuid pk, score int, reasons jsonb, llm_summary text, created_at)`)
- Test: `tests/integration/test_fraud_consumer.py` (OpenAI mocked via `respx`)

**Interfaces:**
- Produces: pre-filter: process only events where `payload.amount_minor > 50_000_00` or risk score ≥70 (score attached to enriched outbox payload from L2); else skip; flagged → OpenAI `gpt-4o-mini` chat with strict-JSON system prompt → parse → upsert verdict; error → retry once then log dead-letter row.

- [ ] Steps: failing tests (skip-path no LLM call; flag-path mocked response parsed+stored; malformed LLM json → retry once → dead-letter) → implement with tenacity retry=1 → PASS → commit `feat(fraud): llm verdicts with prefilter and dead-letter`.

### Task 24: Wire consumers into compose + lag metric

- [ ] Steps: compose services `relay`,`notifications-consumer`,`fraud-consumer`; prometheus_client counters `events_consumed_total{topic}`, gauge `outbox_backlog` (periodic count query) exposed on api `/metrics`; unit test counter increment; commit `feat(obs): consumer metrics`.

### Task 25: Phase L3 exit check

- [ ] Demo script: make payment → SSE event received in curl stream; large payment triggers fraud verdict row (mock OPENAI_API_KEY via recorded response env `LC_OPENAI_MOCK=1` serving canned JSON); kill -9 relay mid-batch → restart → backlog drains to zero (query shows published_at null count 0). Screenshots + `git commit -am "docs: L3 exit runbook"`.

---

## Phase L4 — React Dashboard (demoable product)

### Task 26: Vite scaffold + typed API client + auth pages + i18n base

**Files:**
- Create: `web/` (vite react-ts template, tailwind init), `web/src/api/client.ts`, `web/src/pages/{Login.tsx,Register.tsx}`
- Create: `web/src/i18n/index.ts`, `web/src/i18n/locales/{en.json,es.json,de.json}`
- Test: `web/src/api/client.test.ts` (vitest, msw), `web/src/i18n/i18n.test.ts`

**Interfaces:**
- Produces: `apiClient` with `login/register/createAccount/getAccounts/pay(payload,idemKey)/streamEvents(onEvent)`; JWT stored in memory + localStorage refresh-on-mount; 401 → redirect login.
- Produces: i18n singleton via `react-i18next` — `initI18n()` loading namespaces from `locales/*.json`; helper `t(key, opts?)`; supported locales const `SUPPORTED = ['en','es','de'] as const`; key parity enforced by test.

- [ ] Steps: scaffold (`npm create vite@latest web -- --template react-ts`), tailwind init, **install `i18next react-i18next`**, create locale files covering ALL existing strings (auth + common nav) with complete translations en/es/de — German uses formal register (Sie-form), Spanish neutral LatAm — failing tests: (a) msw handlers for auth endpoints token attached / 401 handling; (b) i18n test asserting `t('auth.login.title')` differs across the three locales and unknown key throws in dev → implement fetch wrapper + i18n init with `localStorage('lc_locale')` → navigator.language fallback clamped to SUPPORTED → vitest PASS → Login/Register forms wired **using `t()` only, zero literal strings** → commit `feat(web): api client, auth pages and trilingual i18n base`.

### Task 27: Dashboard shell — accounts + balances + language switcher

**Files:**
- Create: `web/src/pages/Dashboard.tsx`, `web/src/components/{AccountCard.tsx,LanguageSwitcher.tsx}`, `web/src/lib/format.ts`
- Test: `web/src/components/AccountCard.test.tsx`, `web/src/components/LanguageSwitcher.test.tsx`

**Interfaces:**
- Produces: `formatMinor(amountMinor:number, currency:string, locale:'en'|'es'|'de')` — BIGINT minor units → localized string via `Intl.NumberFormat(locale,{style:'currency',currency})`; `<LanguageSwitcher/>` renders EN|ES|DE toggle writing `localStorage('lc_locale')`, syncing `document.documentElement.lang`, and triggering i18next re-render.

- [ ] Steps: failing render tests (card shows formatted amount per active locale — e.g. minor 1050 USD renders `$10.50` in en and `10,50 US$` in de; switcher click changes visible nav label and persists) → implement card + switcher + grid fetching `getAccounts` every 5s (polling hook `usePolling(fn,ms)`) → vitest PASS → commit `feat(web): dashboard with live balances and language switcher`.

### Task 28: Transfer form modal (idempotent UX)

**Files:**
- Create: `web/src/components/TransferModal.tsx`
- Test: `web/src/components/TransferModal.test.tsx`

**Behavior:** uuid generated once per logical attempt and REUSED on network-error retry (state kept until success/navigate); validation client-side (amount>0, dest≠source).

- [ ] Steps: failing tests (retry keeps same key — assert via msw request history; success clears) → implement → PASS → commit `feat(web): idempotent transfer form`.

### Task 29: History table + alerts panel (SSE hook)

**Files:**
- Create: `web/src/components/{HistoryTable.tsx,AlertsPanel.tsx}`, `web/src/hooks/useEventStream.ts`
- Test: `web/src/hooks/useEventStream.test.ts` (EventSource mocked)

- [ ] Hook contract: auto-reconnect exponential backoff 1s→8s, resubscribe same URL, exposes last event + connection state badge.
- [ ] Steps: failing hook tests (reconnect after error, filters by account) → implement → panels consume → PASS → commit `feat(web): history and live alerts`.

### Task 30: nginx container + Playwright e2e + exit

**Files:**
- Create: `web/Dockerfile` (node:20 build → nginx:alpine serve, proxy `/api`,`/events`→api), `web/e2e/happy.spec.ts`, playwright.config.ts
- Modify: `docker-compose.yml` (+`web` service profile `demo` port 5173)

- [ ] e2e scenario: register A+B (via UI) → dev-seed fund A (`LC_ENABLE_DEV_ENDPOINTS=true` route `POST /dev/seed {account_id, amount_minor}` added with guard test) → A transfers 500 to B → B page (second context) balance updates WITHOUT reload ≤5s (assert DOM text change) → **i18n assertion: switch language EN→DE, assert transfer modal label changes (e.g. `Amount`→`Betrag`) AND persists after page reload** → screenshots to `docs/screenshots/l4-dashboard.png`.
- [ ] Run: `npm run e2e` green with compose stack up. Commit `e2e: full transfer journey through UI` + `docs: L4 exit`.

---

## Phase L5 — Observability, MCP Server, Docs Freeze

### Task 31: OpenTelemetry traces end-to-end

**Files:**
- Modify: `main.py` (OTel SDK auto-instrument FastAPI+SQLAlchemy, OTLP exporter), compose (+jaeger `jaegertracing/all-in-one` :16686), `services/risk-engine/src/main.rs` (tracing + otel-grpc span around Evaluate)
- Test: `tests/integration/test_tracing.py` asserting span attributes present via in-memory exporter.

- [ ] Steps: failing span-presence test → wire SDK (`opentelemetry-sdk`, `instrumentation-fastapi`, `instrumentation-sqlalchemy`) → rust side adds `trace` attr spans → manual verify waterfall shows api→grpc chain in Jaeger UI screenshot → commit `feat(obs): distributed tracing across python-rust`.

### Task 32: Grafana dashboards-as-code

**Files:**
- Create: `grafana/provisioning/{datasources/pg.yml,dashboards.yml}`, `grafana/dashboards/ledgercore.json` (panels: payments rate, transfer latency p95, outbox backlog gauge, kafka consumer lag, risk latency histogram)
- Modify: compose (+grafana :3001 provisioned volume)

- [ ] Steps: dashboards load via provisioning (curl grafana API asserts 1 dashboard titled LedgerCore) → generate traffic script → screenshot overview → commit `feat(obs): provisioned grafana`.

### Task 33: MCP server exposing platform tools

**Files:**
- Create: `src/ledgercore/mcp/server.py`, migration `0004_review_flags.py` (`review_flags(payment_id uuid pk, note text, created_at)`)
- Test: `tests/integration/test_mcp_tools.py` invoking tool functions directly (protocol layer thin)

**Tools (official `mcp` SDK, stdio):**
`get_balance(account_id)`, `list_recent_payments(limit=20)`, `get_fraud_verdict(payment_id)`, `flag_payment_for_review(payment_id, note)`.

- [ ] Steps: failing function-level tests → implement thin wrappers over existing repos/services → register with SDK server → smoke-run `uv run python -m ledgercore.mcp.server` + sample client call transcript into `docs/mcp-demo.txt` → commit `feat(mcp): agent-operable tools surface`.

### Task 34: ADR set + AGENTS.md + consolidated benchmarks

**Files:**
- Create: `docs/adr/0001-double-entry-vs-balance-column.md` … `0007-mcp-surface.md` (titles: double-entry design; BIGINT minor units; deterministic lock ordering; transactional outbox; fail-closed risk policy; SSE over WebSocket v1; MCP tools surface), `AGENTS.md` (agentic workflow used, commands, conventions)
- Modify: `README.md` final architecture diagram + benchmark table linking docs/benchmarks.md

- [ ] Steps: each ADR ≤60 lines with Context/Decision/Consequences/Alternatives; README updated; links checked (`lychee` or grep script) → commit `docs: adr set and engineering workflow`.

### Task 35: Final hardening + release v1.0.0

- [ ] Full suite: `make lint test` + integration + property + web vitest + Playwright green in CI; `docker compose down -v && docker compose up` from clean state passes Task 12/19/25/30 scripts sequentially (script `scripts/demo_all.sh`); CHANGELOG.md generated from conventional commits (`git log --oneline` curated); `git tag -a v1.0.0 -m "LedgerCore v1.0.0"`; push tags.

## Self-Review Checklist (executed during execution)

- Spec §4 DoD items map: API-first ✓(T9,T13) · async ingest n/a · hybrid search n/a · sources n/a · MCP ✓(T33) · Langfuse→OTel equivalent ✓(T31) · RAGAS n/a · Testcontainers ✓ · session memory n/a · compose ✓(T1..). Trading-spec items intentionally absent (this is LedgerCore plan).
