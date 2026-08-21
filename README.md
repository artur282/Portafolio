# 🎯 Software Engineering Portfolio — Backend Senior (Python · Rust)

> **Two production-grade systems. One engineering standard.**
> Not tutorials — complete products with architecture decisions, performance benchmarks, real infrastructure, and live frontends.

<p align="center">
  <img src="https://img.shields.io/badge/Projects-2_Production_Systems-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Stack-Python_|_Rust_|_Kafka_|_gRPC_|_React-purple?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Demo-docker_compose_up-green?style=for-the-badge"/>
</p>

---

## 📋 Table of Contents

- [The Portfolio](#-the-portfolio)
- [💳 LedgerCore — Payments Platform](#-ledgercore--payments-platform)
- [📈 TickForge — Market Data & Paper-Trading Engine](#-tickforge--market-data--paper-trading-engine)
- [Engineering Standard](#-engineering-standard)
- [Stack vs Market Matrix](#-stack-vs-market-matrix)
- [Roadmap](#-roadmap)
- [Archive](#-archive)

---

## 🌟 The Portfolio

Every project in this portfolio follows one contract:

> **`docker compose up`** — and the entire system is running: backend services (Python + Rust), databases, message broker, observability stack, **and the frontend**. No local toolchain beyond Docker.

| Capability | LedgerCore | TickForge |
|---|:-:|:-:|
| Distributed consistency / strict ACID | ✅ core | ➖ |
| High concurrency / throughput | ✅ | ✅ core |
| Event-driven (Kafka, outbox, sagas) | ✅ | ✅ |
| Polyglot Python ↔ Rust (gRPC/Protobuf) | ✅ | ✅ |
| Geospatial-free real-time (WebSocket streaming) | SSE | ✅ core |
| AI integration (fraud LLM / MCP servers) | ✅ | ✅ |
| Infra: Docker distroless, CI/CD, K8s, observability | ✅ | ✅ + Helm |
| Visual demo (React + Vite) | ✅ banking dashboard | ✅ live trading desk |

---

## 💳 LedgerCore — Payments Platform

> Multi-currency wallets · double-entry ledger · idempotent payment API · sub-ms risk engine in Rust · LLM fraud analysis · banking dashboard.

**Repo:** [`github.com/<you>/ledgercore`](https://github.com/) *(in progress)*

### What makes it hard

| Problem | Solution | Proof |
|---|---|---|
| Concurrent transfers corrupting balances | Row-level locking + ACID ledger | k6: 1000 concurrent transfers → 0 anomalies |
| Duplicate payments under retries | Idempotency keys (Redis fast-path + DB constraint) | Retry-storm test suite |
| Dual-write between DB and Kafka | Transactional outbox pattern | Crash-recovery test |
| Fraud scoring on hot path | Rust/Axum gRPC engine, in-memory velocity windows | Benchmark: <5ms p99 |
| Money integrity | Property-based tests: `sum(debits) == sum(credits)` always | Hypothesis suites |

**Architecture:** FastAPI (API/orchestration) ⇄ gRPC ⇄ Risk Engine (Rust) → PostgreSQL (immutable double-entry entries, BIGINT minor units) → transactional outbox → Kafka → notification/fraud/audit consumers.

---

## 📈 TickForge — Market Data & Paper-Trading Engine

> Live crypto market ingestion (Binance WS) · in-memory L2 order books in Rust · event-sourced matching engine · TimescaleDB tick store · real-time trading desk UI.

**Repo:** [`github.com/<you>/tickforge`](https://github.com/) *(in progress)*

### What makes it hard

| Problem | Solution | Proof |
|---|---|---|
| Ingesting exchange feeds without loss | Rust/Tokio ingestor with reconnects, backpressure, gap detection | Sustained-stream soak test |
| Order book at trading speed | Lock-minimized L2 book in memory | Criterion: µs per update |
| Simulated trading against real prices | Event-sourced matching engine | Deterministic replay test |
| Millions of ticks queried fast | TimescaleDB hypertables + continuous aggregates + compression | Query latency report |
| Perceived performance | End-to-end latency pipeline | ingest→UI p50/p99 chart |

**Architecture:** Binance WS → Rust ingestor → Kafka → Market Engine (books/candles/indicators) ⇄ gRPC ⇄ FastAPI (orders/history/WS fan-out) → React dashboard (lightweight-charts, depth ladder, PnL).

---

## 🧭 Engineering Standard

Both projects ship:

- **📖 English docs:** README, 10+ ADRs, C4/Mermaid diagrams, OpenAPI, benchmark reports, runbook.
- **🤖 Agentic engineering:** each platform exposes an **MCP server** (AI agents can operate it: `get_balance`, `place_paper_order`, …) plus an `AGENTS.md` documenting the AI-assisted workflow that built it.
- **🧪 TDD:** tests first on the hard problems; property-based testing for invariants; Testcontainers integration suites.
- **🔁 CI/CD:** GitHub Actions — lint (clippy/ruff) → unit → integration → multi-arch distroless build → GHCR → compose smoke test.
- **📊 Observability:** OpenTelemetry traces across the Python↔gRPC↔Rust boundary, Prometheus metrics, Grafana dashboards-as-code.
- **🌐 Trilingual UI:** both dashboards ship EN/ES/DE (German formal register) via i18next with key-parity tests — direct alignment with Swiss market requirements.
- **☸️ Deployment:** Docker Compose dev/demo; TickForge additionally ships Helm/K8s manifests.

## 📊 Stack vs Market Matrix

Skill matrix validated against the 7 target job offers (Switzerland & Europe, Python/Rust backend roles):

| Requirement | Covered by |
|---|---|
| Production Rust (async/Tokio, Axum, tonic) | TickForge core + LedgerCore risk engine |
| Production Python/FastAPI at senior level | Both API layers + consumers |
| Kafka / event-driven architecture | Outbox+consumers (LedgerCore), stream processing (TickForge) |
| Deep PostgreSQL / time-series SQL | Double-entry ledger design / TimescaleDB aggregates |
| Low-latency systems + benchmarks | Risk engine <5ms p99 / order book µs ops |
| Kubernetes + Helm + distroless images | CI builds + TickForge deploy story |
| AI-native workflows (LLM, MCP) | Fraud scanner + per-project MCP servers |
| Frontend demonstrable | Two full dashboards |

## 🗺️ Roadmap

Status legend: ⬜ pending · 🟨 in progress · ✅ done

### LedgerCore
- [ ] **L1** — API + double-entry ledger + idempotency + compose stack
- [ ] **L2** — Rust risk engine (gRPC) + benchmarks
- [ ] **L3** — Kafka outbox + notifications + LLM fraud scanner
- [ ] **L4** — React dashboard + SSE + containerized frontend
- [ ] **L5** — Observability + reports + MCP server + final ADRs

### TickForge
- [ ] **T1** — Rust WS ingestor → Kafka (reconnect/gap handling)
- [ ] **T2** — Market engine: L2 books + candles + indicators (+ benches)
- [ ] **T3** — Event-sourced matching engine + paper trading
- [ ] **T4** — FastAPI layer + TimescaleDB store
- [ ] **T5** — Trading desk dashboard + WebSocket fan-out
- [ ] **T6** — Helm/K8s + latency report + MCP server + final ADRs

Full design rationale: [`docs/superpowers/specs/2026-08-21-portfolio-v2-ledgercore-tickforge-design.md`](./docs/superpowers/specs/2026-08-21-portfolio-v2-ledgercore-tickforge-design.md)

## 🗄️ Archive

The previous 6-project monthly roadmap (May–Oct 2026) lives in [`_archive/`](./_archive/) — kept as reference material.

## 📜 License

MIT.
