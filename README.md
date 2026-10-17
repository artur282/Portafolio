# Luis Cruz — Backend & AI Engineering Portfolio

![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)
![Projects](https://img.shields.io/badge/Projects-2_%2B_1_site-6C63FF?style=flat-square)
![Status](https://img.shields.io/badge/Status-in_construction-orange?style=flat-square)
![Language](https://img.shields.io/badge/Python_%7C_Rust_%7C_TypeScript-3776AB?style=flat-square)

> The hub of my engineering portfolio: two production-grade backend systems and a personal site.
> This repository holds the plan, the standards and the roadmap behind them — not the code.

**Es:** El hub de mi portafolio. Aquí viven el plan, los estándares y el roadmap; cada proyecto tendrá su propio repositorio cuando su primera tarea esté commiteada.

---

## What this repo is (and what it is not)

This is **not** a project. It is the single source of truth for three projects that do not share code:

- the **plan** — what gets built, in what order, and why
- the **standards** — the rules that define "done" for every project
- the **roadmap** — the honest state of every epic and task

Each project gets its own repository, created when its first task lands. **Nothing here claims to work yet.** There are no screenshots of systems that do not exist, and no links to repositories that have not been created.

## The three deliverables

| Project | What it is | Core language | Target market |
|---|---|---|---|
| **Caliber** | Quality lab for search and RAG: load a corpus and golden questions, run comparable experiments (text vs vector vs hybrid, chunking, embeddings) and publish reproducible metrics — recall@k, MRR, nDCG — through an evidence console that shows everything in 60 seconds. | Python (FastAPI) | Spain |
| **Continuum** | Durable execution engine written in **Rust**: declarative workflows that survive crashes (event-sourced journal), retries, timers, signals and **exactly-once effects** verified by crash-injection tests — with published benchmarks and a chaos-button demo. | Rust (Axum/Tokio) | Switzerland / Germany |
| **Personal site** | Terminal-style interactive site presenting both systems with their real numbers, a downloadable CV (ES/EN/DE) and a technical blog. | TypeScript (Next.js) | Both |

**The two systems are independent.** Each one starts with a single `docker compose up` and boots with pre-seeded demo data — never an empty screen.

## The problem

I analysed **100 real backend job postings** (LinkedIn, Spain + Switzerland/Germany) before writing a line of code. Three findings shaped the plan:

| Finding | Evidence |
|---|---|
| The Spanish market is Python-first, and AI is no longer optional | Python appears in **26.5%** of postings, Rust in 15, AI/LLM signals in 11 (10 of them in Barcelona). |
| The Swiss/German market is the opposite | **Rust in 70.6%** of postings, Python in 0. A Python-only portfolio does not open that market. |
| My GitHub could not prove the claims I was making | **0 of my repositories had CI**, topics or descriptions, while my profile advertised "CI/CD with GitHub Actions". Backend Rust appeared nowhere. |

So the plan targets the real gaps: **reproducible evaluation, durable systems with measured performance, CI that actually runs, and Rust that does real work** — instead of another CRUD application.

## How the pieces fit

```
                        ┌──────────────────────────────┐
                        │  Portafolio (this repo)      │
                        │  plan · standards · roadmap  │
                        └───────────────┬──────────────┘
                                        │
        ┌───────────────────────────────┼───────────────────────────────┐
        │                               │                               │
        ▼                               ▼                               ▼
┌───────────────────┐         ┌───────────────────┐         ┌───────────────────┐
│      Caliber      │         │     Continuum     │         │   Personal site   │
│  Python · FastAPI │         │   Rust · Tokio    │         │  Next.js · TS     │
│  pgvector + FTS   │         │  Event-sourced    │         │  Terminal UI      │
│  Eval console     │         │  Chaos-button demo│         │  Blog + CV        │
└───────────────────┘         └───────────────────┘         └───────────────────┘
      docker compose up             docker compose up          static build
```

No shared runtime, no shared database, no coupling: any project can be reviewed on its own.

## Key decisions

| Decision | Alternative rejected | Why |
|---|---|---|
| Build everything from scratch | Extend the previous repositories | Clean ownership: standards from the first commit and every line explainable in an interview. The previous repositories stay as history. |
| Evidence-first fronts with a 60-second guided tour | README-only presentation | Reviewers look at a project for about a minute: each system ships a front that proves its capabilities and a tour that triggers the key actions itself. |
| Caliber: reproducible experiments with a stable config hash | One-off demos | If a result cannot be reproduced, it is not published. Same config → same numbers. |
| Continuum: event-sourced journal as the source of truth | In-memory state plus snapshots | Crash recovery becomes a property of the design, not a patch. Exactly-once effects are verified by crash-injection tests in CI. |
| Measured numbers only | Absolute benchmarks without a baseline | p95, throughput, recall@k, recovery time — each with methodology and hardware. A number without a baseline is marketing. |
| English interfaces, Spanish learning docs | Trilingual product UIs (ES/EN/DE) | The market data does not ask for trilingual products; the learning documentation (in Spanish) is what makes the work defensible in an interview. |
| Zero budget | Paid cloud / LLM APIs | Local embeddings, free-tier models, free deployment. A reviewer can reproduce the whole stack. |

## Status

Honest state as of **October 2026**. Every epic is tracked in its own `docs/PLAN.md`.

| Project | Epics | Tasks | Done | Next milestone |
|---|:-:|:-:|:-:|---|
| Caliber | 8 | 29 | 0 | Foundations + CI (Oct–Nov) |
| Continuum | 8 | 26 | 0 | Foundations + CI (Oct–Nov) |
| Personal site | 8 | 34 | 0 | Foundations + CI (Oct–Nov) |

**No measured results are published yet, because nothing is measured yet.** The rule for every project in this portfolio is: if it is not measured, it is not published. Numbers — p95 latency, recall@k, throughput, recovery time, coverage — appear in each project README as its epics close.

<details>
<summary><b>Full roadmap — 89 tasks across 24 epics</b></summary>
<br/>

**Caliber — 8 epics · 29 tasks**

| # | Epic |
|:-:|---|
| 0 | Foundations — monorepo, dev compose, **CI from the first commit**, app factory + health |
| 1 | Corpus and golden questions — models + migration, deterministic seed (~120 docs, ~80 questions), chunking |
| 2 | Retrieval engine — full-text, pgvector, RRF hybrid, stable config hash |
| 3 | Experiments and metrics — recall@k / MRR / nDCG, async runner with SSE progress, optional LLM judge, leaderboard |
| 4 | API — API keys, corpus/experiments endpoints, live compare |
| 5 | **Evidence front** — live experiments, leaderboard charts, side-by-side comparator, 60-second guided tour |
| 6 | Observability and quality — logs, traces, Prometheus, coverage ≥80%, adversarial API tests |
| 7 | Deploy and close-out — multi-stage images, free VM deploy, README with real metrics, ADRs, learning docs |

**Continuum — 8 epics · 26 tasks**

| # | Epic |
|:-:|---|
| 0 | Foundations — workspace, CI (fmt/clippy/deny), dev compose |
| 1 | Model and journal — typed workflows, **event-sourced journal in PostgreSQL**, demo seed |
| 2 | Scheduler and execution — bounded queues with backpressure, retries, durable timers, signals, **exactly-once effects** |
| 3 | **Durability and recovery** — crash recovery, crash-injection tests in CI, deterministic replay |
| 4 | API and observability — Axum REST, live journal stream (WS/SSE), Prometheus |
| 5 | **Evidence front** — live mission control, **chaos button**, journal viewer, benchmarks panel, 60-second guided tour |
| 6 | Benchmarks — Criterion, reproducible methodology, results in the README |
| 7 | Deploy and close-out — lean images, guarded public demo, ADRs, learning docs |

**Personal site — 8 epics · 34 tasks**

| # | Epic |
|:-:|---|
| 0 | Foundations — Next.js scaffold, **CI from the first commit**, layout, performance baseline |
| 1 | **Terminal engine** — command parser, section registry, base commands, accessible navigation |
| 2 | Real content — single typed data layer, projects with `TODO(metrics)`, integrity test |
| 3 | Sections — welcome, about, projects, skills, experience, contact, blog |
| 4 | Visual identity — sidebar, design tokens, mobile-first, chrome |
| 5 | Motion — typewriter, section reveals, performance discipline |
| 6 | Quality — accessibility, SEO, Lighthouse ≥95, real-device test |
| 7 | Deploy and close-out — Vercel, downloadable CV, learning docs, GIF and video |

</details>

## Engineering standard

These rules apply to all three projects. They are the definition of done, not suggestions.

1. **CI from the first commit** — lint, type check, tests and build on every push, with real services (PostgreSQL, Redis), not mocks.
2. **Evidence-first README** — the first screen answers in 10 seconds: GIF, live demo link, and a table of real metrics. Then pitch → problem → architecture → trade-offs → measured results → stack → how to run it → suggested review path → honest limitations → license.
3. **ADRs in `docs/decisions/`** — context, decision, alternatives considered, consequences. Minimum four per project.
4. **Measured metrics only.** No invented figures, no skill bars, no percentage lists.
5. **One-command demo** with pre-seeded data, plus a 45–90 s video and a 10–15 s GIF.
6. **Evidence front with a 60-second guided tour** — every screen proves a capability; the tour triggers the key actions and narrates the technology behind each step.
7. **Hygiene** — no secrets in history, correct `.gitignore`, LICENSE, topics, single `main` branch.
8. **Learning documentation** — every epic ships a written explanation of what was built, why, how to verify it, and the five questions an interviewer would ask.

## Suggested review path

This repository is short enough to read in five minutes. In this order:

1. **The three deliverables** (top) — what each system is and which market it targets.
2. **The problem** — the 100-posting analysis that decided the scope. Every claim carries a number.
3. **Key decisions** — seven trade-offs, each with the alternative that was rejected and the reason.
4. **Status and roadmap** — the real epic and task breakdown, including what is *not* built yet.
5. **Engineering standard** — the eight rules that define "done". This is what the projects get measured against.
6. **Limitations** — what this portfolio does not do. Read this one before judging the rest.
7. **[`_archive/`](./_archive/)** — the superseded LedgerCore/TickForge specifications, kept as reference. Note what was dropped and why.
8. **[LICENSE](./LICENSE)** — MIT.

When the project repositories exist, each README opens with a review path of its own: run the demo, read the critical file, run the tests, read the ADRs.

## Limitations

- **The systems are not built yet.** This repository currently contains the plan, not the outcome. The roadmap and task counts above are the honest state.
- **The market research is a snapshot** of 100 postings collected in August 2026. It is directional, not statistically rigorous, and the raw dataset is kept private.
- **Agent-assisted implementation.** The code in the three projects is written with AI agents under my direction. The learning documentation exists precisely so I can explain and defend every design decision — a rule I hold to: if I cannot explain it, it does not get committed.
- **Zero cost constrains the demo.** Free-tier models and local embeddings mean the deployed demos are slower than a paid production setup. This is a deliberate trade-off, documented in each project.

## Archive

Earlier planning has been **superseded** twice, and both rounds are documented rather than deleted:

- The **LedgerCore / TickForge** specifications (August 2026) are archived in [`_archive/`](./_archive/) — kept as reference material for architecture patterns only.
- The **NexusBoard / Cortex** plans (September 2026) were superseded in October 2026 by Caliber and Continuum, after a market-fit review; they were retired before publication and the reasoning lives in the Key decisions section above.

## License

MIT — see [LICENSE](./LICENSE).
