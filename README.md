# Luis Cruz — Backend & AI Engineering Portfolio

![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)
![Projects](https://img.shields.io/badge/Projects-2_%2B_1_site-6C63FF?style=flat-square)
![Status](https://img.shields.io/badge/Status-in_construction-orange?style=flat-square)
![Language](https://img.shields.io/badge/Python_%7C_Rust_%7C_TypeScript-3776AB?style=flat-square)

> The hub of my engineering portfolio: two production-grade backend systems and a personal site.
> This repository holds the plan, the standards and the market research behind them — not the code.

**Es:** El hub de mi portafolio. Aquí viven el plan, los estándares y la investigación de mercado; cada proyecto tendrá su propio repositorio cuando su primera tarea esté commiteada.

---

## What this repo is (and what it is not)

This is **not** a project. It is the single source of truth for three projects that do not share code:

- the **plan** — what gets built, in what order, and why
- the **standards** — the rules that define "done" for every project
- the **evidence** — 100 real job postings analysed to decide what is worth building

Each project gets its own repository, created when its first task lands. **Nothing here claims to work yet.** There are no screenshots of systems that do not exist, and no links to repositories that have not been created.

## The three deliverables

| Project | What it is | Core language | Target market |
|---|---|---|---|
| **NexusBoard** | Multi-tenant collaborative knowledge base with AI. Hybrid search (full-text + semantic), RAG assistant with streaming, real-time editing, and a **Rust CLI** (`nexusctl`) so AI agents can query and feed the base through scoped API keys. | Python (FastAPI) | Spain |
| **Cortex** | AI agent orchestrator written in **Rust**. A **custom message bus** (not an existing broker) with backpressure, pipeline execution, distributed tracing, and **published benchmarks against an equivalent Python baseline**. | Rust (Axum/Tokio) | Switzerland / Germany |
| **Personal site** | Clean React site presenting both systems with their real numbers, a downloadable CV and a technical blog. Trilingual ES/EN/DE. | TypeScript (React) | Both |

**The two systems are independent.** Each one starts with a single `docker compose up` and boots with pre-seeded demo data — never an empty screen.

## The problem

I analysed **100 real backend job postings** (LinkedIn, Spain + Switzerland/Germany) before writing a line of code. Three findings shaped the plan:

| Finding | Evidence |
|---|---|
| The Spanish market is Python-first, and AI is no longer optional | Python appears in **26.5%** of postings, Rust in 15, AI/LLM signals in 11 (10 of them in Barcelona). Roughly **35%** of general backend roles already ask for AI experience. |
| The Swiss/German market is the opposite | **Rust in 70.6%** of postings, Python in 0. A Python-only portfolio does not open that market. |
| My GitHub could not prove the claims I was making | **0 of 11** repositories had CI, topics or descriptions, while my profile advertised "CI/CD with GitHub Actions". Kafka, gRPC, Kubernetes and backend Rust appeared nowhere. |

So the plan targets the real gaps: **multi-tenancy, measured performance, CI that actually runs, and Rust that does real work** — instead of another CRUD application.

## How the pieces fit

```
                        ┌──────────────────────────────┐
                        │  Portafolio (this repo)      │
                        │  plan · standards · evidence │
                        └───────────────┬──────────────┘
                                        │
        ┌───────────────────────────────┼───────────────────────────────┐
        │                               │                               │
        ▼                               ▼                               ▼
┌───────────────────┐         ┌───────────────────┐         ┌───────────────────┐
│    NexusBoard     │         │      Cortex       │         │   Personal site   │
│  Python · FastAPI │         │   Rust · Axum     │         │  React · TS       │
│  PostgreSQL RLS   │         │   Custom bus      │         │  ES / EN / DE     │
│  pgvector + FTS   │         │   Criterion bench │         │  Blog + CV        │
│  React + nexusctl │         │   React dashboard │         │  GSAP animations  │
└───────────────────┘         └───────────────────┘         └───────────────────┘
      docker compose up             docker compose up          static build
```

No shared runtime, no shared database, no coupling: any project can be reviewed on its own.

## Key decisions

| Decision | Alternative rejected | Why |
|---|---|---|
| Build two new systems from zero | Extend KnowledgeForge / SaaSForge | The original repositories cannot be reviewed without the paid services they depended on, and they mix two markets in one codebase. |
| PostgreSQL Row-Level Security for multi-tenancy | Application-level filtering (`WHERE tenant_id = ?`) | RLS enforces isolation in the database itself, so one forgotten filter cannot leak another tenant's data. It is also the pattern enterprise postings ask about. |
| Custom message bus in Cortex | Redis pub/sub or Kafka | The point of the project is async Rust and backpressure mechanics. Delegating that to an existing broker removes the only interesting engineering. |
| Publish Rust-vs-Python benchmarks | Ship benchmarks without a baseline | A number without a comparison is marketing. A baseline makes the result falsifiable. |
| Documentation is a deliverable, not extra work | Write the code first, document later | The implementation is agent-assisted, so the written explanation is what makes the system defensible in an interview. |
| Zero budget | Paid cloud / LLM APIs | Everything runs locally on open source, with free-tier models and local embeddings. A reviewer can reproduce the whole stack. |

## Status

Honest state as of **September 2026**. Every epic is tracked in its own `docs/PLAN.md`.

| Project | Epics | Tasks | Done | Next milestone |
|---|:-:|:-:|:-:|---|
| NexusBoard | 9 | 33 | 0 | Foundation + CI (Oct–Nov) |
| Cortex | 8 | 22 | 0 | Foundation + CI (Oct–Nov) |
| Personal site | 5 | 13 | 0 | First deploy after the systems exist |

**No measured results are published yet, because nothing is measured yet.** The rule for every project in this portfolio is: if it is not measured, it is not published. Numbers — p95 latency, recall@5, throughput, coverage — appear in each project README as its epics close.

<details>
<summary><b>Full roadmap — 68 tasks across 22 epics</b></summary>
<br/>

**NexusBoard — 9 epics · 33 tasks**

| # | Epic |
|:-:|---|
| 0 | Foundations — monorepo layout, dev compose, **CI from the first commit**, app factory + health |
| 1 | Multi-tenancy, auth and limits — models + migration, **PostgreSQL Row-Level Security**, JWT + RBAC, rate limiting |
| 2 | Documents, versioning and demo data — page CRUD, versioning, deterministic seed (3 tenants, ~50 documents) |
| 3 | Hybrid search — full-text, semantic with pgvector, RRF fusion with measured recall |
| 4 | RAG assistant — OpenRouter client, streaming pipeline, guardrails, published evaluation |
| 5 | Real time — WebSocket hub, presence |
| 6 | **Rust CLI `nexusctl` + AI agents** — CLI crate, scoped API keys, agent skill |
| 7 | React frontend |
| 8 | Deploy and close-out |

**Cortex — 8 epics · 22 tasks**

| # | Epic |
|:-:|---|
| 0 | Foundations — workspace, CI, dev compose |
| 1 | **Message bus in Rust** (the differentiator) — bus core, orchestrator integration |
| 2 | Persistence and demo data — schema, migrations, seed (5 agents, 10 pipelines, 50 traces) |
| 3 | Pipeline execution — agent model, OpenRouter client, executor, gRPC |
| 4 | Observability — traces, aggregated metrics, live WebSocket |
| 5 | React dashboard — shell + i18n, screens, GSAP, E2E |
| 6 | **Benchmarks** — Criterion, Python baseline, publication |
| 7 | Deploy and close-out |

**Personal site — 5 epics · 13 tasks**

| # | Epic |
|:-:|---|
| 1 | Foundation — scaffold, design system, real content |
| 2 | Sections — structure, projects, downloadable CV |
| 3 | GSAP animation — entrances, interactions, performance discipline |
| 4 | i18n ES/EN/DE and data integrity |
| 5 | Deploy and close-out |

</details>

## Engineering standard

These rules apply to all three projects. They are the definition of done, not suggestions.

1. **CI from the first commit** — lint, type check, tests and build on every push, with real services (PostgreSQL, Redis), not mocks.
2. **Fixed README structure** — pitch → problem → demo → architecture → trade-offs → measured results → stack → how to run it → suggested review path → honest limitations → license.
3. **ADRs in `docs/decisions/`** — context, decision, alternatives considered, consequences. Minimum four per project.
4. **Measured metrics only.** No invented figures, no skill bars, no percentage lists.
5. **One-command demo** with pre-seeded data, plus a 45–90 s video and a 10–15 s GIF.
6. **Trilingual UI (ES/EN/DE)** verified by a key-parity test that fails when a locale is incomplete.
7. **Hygiene** — no secrets in history, correct `.gitignore`, LICENSE, topics, single `main` branch.
8. **Learning documentation** — every epic ships a written explanation of what was built, why, how to verify it, and the five questions an interviewer would ask.

## Suggested review path

This repository is short enough to read in five minutes. In this order:

1. **The three deliverables** (top) — what each system is and which market it targets.
2. **The problem** — the 100-posting analysis that decided the scope. Every claim carries a number.
3. **Key decisions** — six trade-offs, each with the alternative that was rejected and the reason.
4. **Status and roadmap** — the real epic and task breakdown, including what is *not* built yet.
5. **Engineering standard** — the eight rules that define "done". This is what the projects get measured against.
6. **Limitations** — what this portfolio does not do. Read this one before judging the rest.
7. **[`_archive/`](./_archive/)** — the superseded LedgerCore/TickForge specifications, kept as reference. Note what was dropped and why.
8. **[LICENSE](./LICENSE)** — MIT.

When the project repositories exist, each README opens with a review path of its own: run the demo, read the critical file, run the tests, read the ADRs.

## Limitations

- **The systems are not built yet.** This repository currently contains the plan, not the outcome. The roadmap and task counts above are the honest state.
- **The market research is a snapshot** of 100 postings collected in August 2026. It is directional, not statistically rigorous.
- **Agent-assisted implementation.** The code in the three projects is written with AI agents under my direction. The learning documentation exists precisely so I can explain and defend every design decision — a rule I hold to: if I cannot explain it, it does not get committed.
- **Zero cost constrains the demo.** Free-tier models and local embeddings mean the deployed demos are slower than a paid production setup. This is a deliberate trade-off, documented in each project.

## Archive

Earlier portfolio planning (the LedgerCore and TickForge specifications and their implementation plans) has been **superseded** and moved to [`_archive/`](./_archive/). It is kept as reference material for architecture patterns only — those systems are not part of this portfolio and will not be built.

## License

MIT — see [LICENSE](./LICENSE).
