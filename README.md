# 🚀 Portafolio de Ingeniería de Software + AI — Marzo a Agosto 2026

> **32 proyectos. 6 meses. Backend Senior + AI Engineer en un solo roadmap.**

<p align="center">
  <img src="https://img.shields.io/badge/Duración-6_meses-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Proyectos-32-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Backend-26_weekend_projects-orange?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/AI-6_monthly_projects-red?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Stack-Python_|_Rust_|_LLMs_|_RAG_|_DevOps-purple?style=for-the-badge"/>
</p>

---

## 📋 Tabla de contenidos

- [🎯 Objetivo](#-objetivo)
- [🗺️ Roadmap visual](#️-roadmap-visual)
- [📅 Plan mensual](#-plan-mensual)
- [🛠️ Stack tecnológico](#️-stack-tecnológico)
- [📐 Metodología](#-metodología)
- [📊 Progreso](#-progreso)

---

## 🎯 Objetivo

Construir un portafolio progresivo que demuestre dominio completo en **ingeniería backend, IA/GenAI, ingeniería de datos, DevOps e integración full-stack**. Cada proyecto es independiente, funcional y desplegable — diseñado para resolver problemas reales, no solo ejercicios académicos.

### Principios del portafolio

| Principio                          | Descripción                                                                                        |
| ---------------------------------- | -------------------------------------------------------------------------------------------------- |
| 🏗️ **Producción primero**          | Cada proyecto está diseñado como si fuera a producción: tests, docs, Docker, CI/CD                 |
| 📈 **Progresión**                  | La complejidad aumenta mes a mes, construyendo sobre habilidades previas                           |
| 🔗 **Integración**                 | Los proyectos finales combinan múltiples habilidades en soluciones completas                       |
| 📖 **Documentación**               | Cada proyecto incluye README, diagramas UML (clases, secuencia) y decisiones de arquitectura (ADR) |
| 🧪 **TDD & Testing Trophy**        | TDD estricto (Rojo→Verde→Refactor), priorizando tests de integración sobre unitarios               |
| 🏛️ **Layered Architecture**        | Separación estricta: Controller → Service → Repository → Request/Response DTO                      |
| ⚡ **Event-Driven**                | Arquitectura orientada a eventos con Publishers/Listeners en cada proyecto                         |
| 📐 **API First**                   | Contrato OpenAPI definido antes de escribir código                                                 |

---

## 🗺️ Roadmap visual

```
MAR ──────── ABR ──────── MAY ──────── JUN ──────── JUL ──────── AGO
  │            │            │            │            │            │
  ▼            ▼            ▼            ▼            ▼            ▼
🏗️ Backend   🤖 IA/ML    📊 Datos    ☁️ DevOps   🔗 Full     🏆 Capstone
Foundations  & GenAI     & Automat.  & Cloud     Stack       Projects
  │            │            │            │            │            │
  ├─ TaskFlow  ├─ ConversAI ├─ DataBridg ├─ ShipIt    ├─ ProjectH  ├─ AutomateAI
  ├─ AuthGuard ├─ MCPForge  ├─ TenantAPI ├─ AxumMicro ├─ LiveDash  ├─ APIMarket
  ├─ DataHarv  ├─ DocuMind  ├─ PayFlow   ├─ GateKeep  ├─ SnapTask📱├─ PortfolioOS
  └─ RustCLI🦀 └─ SearchMas ├─ InsightAP ├─ ProtoLink └─ NotifyH   ├─ OpenContrib
                             └─ LogStream                           └─ Retrospect.

🤖 AI TRACK (mensual, entre semana ~40h/mes)
  │            │            │            │            │            │
  └─PromptLab └─ DocuRAG   └─ EvalForge └─ AgentFlow └─ GuardAI   └─ TailorAI
     Prompt       RAG          Eval &       Agentes      Seguridad     Capstone
     Versioning   Pipeline     Observab.    LangGraph    & Guardrails  AI Platform
```

---

## 📅 Plan mensual

### [🏗️ Marzo — Backend Foundations](./1-marzo/)

> _Sentar las bases con APIs robustas, autenticación, scraping y herramientas CLI_

| Semana | Proyecto                                                                                               | Tecnologías clave            | Descripción                                                   |
| ------ | ------------------------------------------------------------------------------------------------------ | ---------------------------- | ------------------------------------------------------------- |
| 01     | **[TaskFlow API](./1-marzo/semana-01-taskflow-api.md)**                                                | FastAPI, PostgreSQL, Docker  | API REST completa con CRUD, filtros, paginación y tests ✅    |
| 02     | **[AuthGuard](./1-marzo/semana-02-authguard.md)**                                                      | FastAPI, PostgreSQL, JWT     | Sistema de autenticación con roles, permisos y refresh tokens ✅ |
| 03     | **[DataHarvest](./1-marzo/semana-03-dataharvest.md)**                                                  | Selenium, Pandas, PostgreSQL | Web scraper inteligente con extracción y análisis de datos ✅ |
| 04     | **[RustCLI (DashTUI)](./1-marzo/semana-04-rustcli.md)**                                                | Rust, Clap, Ratatui          | TUI interactivo y CLI todo-en-uno de alto rendimiento 🦀✅    |
| 🤖 AI  | **[PromptLab](./1-marzo/mes-01-promptlab.md)**                                                         | OpenAI, Anthropic, Langfuse  | Sistema de versionado de prompts con evaluación comparativa   |

---

### [🤖 Abril — IA/ML & GenAI](./2-abril/)

> _Integrar inteligencia artificial en aplicaciones prácticas y productivas_

| Semana | Proyecto                                            | Tecnologías clave          | Descripción                                                         |
| ------ | --------------------------------------------------- | -------------------------- | ------------------------------------------------------------------- |
| 05     | **[ConversAI](./2-abril/semana-05-conversai.md)**   | LangChain, FastAPI, OpenAI | Chatbot conversacional con memoria y contexto                       |
| 06     | **[MCPForge](./2-abril/semana-06-mcpforge.md)**     | MCP SDK, Python, Docker    | Servidor MCP personalizado que expone herramientas como servicio    |
| 07     | **[DocuMind](./2-abril/semana-07-documind.md)**     | RAG, Vector DBs, LangChain | Pipeline de Q&A sobre documentos con retrieval augmented generation |
| 08     | **[SearchMaster](./2-abril/semana-08-sentiscope.md)** | Elasticsearch, FastAPI    | Motor de búsqueda full-text con facets, autocompletado y relevance tuning    |
| 🤖 AI  | **[DocuRAG](./2-abril/mes-02-docurag.md)**          | RAGAS, pgvector, Cohere    | Pipeline RAG de producción con chunking avanzado y evaluación RAGAS |

---

### [📊 Mayo — Data Engineering & Automation](./3-mayo/)

> _Dominar pipelines de datos, ETL, monitoreo y automatización empresarial_

| Semana | Proyecto                                           | Tecnologías clave           | Descripción                                                  |
| ------ | -------------------------------------------------- | --------------------------- | ------------------------------------------------------------ |
| 09     | **[DataBridge](./3-mayo/semana-09-databridge.md)** | Python, PostgreSQL, SQLite  | Pipeline ETL completo con validación, transformación y carga |
| 10     | **[TenantAPI](./3-mayo/semana-10-pricewatch.md)**  | FastAPI, PostgreSQL RLS    | API Multi-tenant SaaS con Row-Level Security y aislamiento        |
| 11     | **[PayFlow](./3-mayo/semana-11-flowengine.md)**    | FastAPI, Stripe, Webhooks  | Sistema de billing y suscripciones con Stripe webhooks             |
| 12     | **[InsightAPI](./3-mayo/semana-12-insightapi.md)** | FastAPI, Pandas, Plotly     | API de reportes dinámicos con visualizaciones                |
| 13     | **[LogStream](./3-mayo/semana-13-logstream.md)**   | Python, PostgreSQL, FastAPI | Sistema de ingestión y análisis de logs                      |
| 🤖 AI  | **[EvalForge](./3-mayo/mes-03-evalforge.md)**      | Langfuse, RAGAS, Grafana    | Framework de evaluación y observabilidad con A/B testing     |

---

### [☁️ Junio — DevOps & Cloud](./4-junio/)

> _Infraestructura, CI/CD, containerización y despliegue en la nube_

| Semana | Proyecto                                              | Tecnologías clave            | Descripción                                           |
| ------ | ----------------------------------------------------- | ---------------------------- | ----------------------------------------------------- |
| 14     | **[ShipIt](./4-junio/semana-14-shipit.md)**           | GitHub Actions, Docker       | Pipeline CI/CD completo con GitFlow y deploy automático   |
| 15     | **[AxumMicro](./4-junio/semana-15-axummicro.md)**     | Axum, SQLx, Serde, Docker    | Microservicio ultraligero con Distroless (20-30MB)    |
| 16     | **[GateKeeper](./4-junio/semana-16-gatekeeper.md)**   | Python, Redis, JWT           | API Gateway con rate limiting, caché y auth           |
| 17     | **[ProtoLink](./4-junio/semana-17-clouddeploy.md)**   | gRPC, Protobuf, Rust, Python | Servicio gRPC polyglot: servidor Python + cliente Rust |
| 🤖 AI  | **[AgentFlow](./4-junio/mes-04-agentflow.md)**        | LangGraph, OpenAI, Function Calling | Agente empresarial multi-paso con tools y memoria |

---

### [🔗 Julio — Full-Stack Integration](./5-julio/)

> _Proyectos full-stack que combinan frontend y backend en experiencias completas_

| Semana | Proyecto                                            | Tecnologías clave           | Descripción                                           |
| ------ | --------------------------------------------------- | --------------------------- | ----------------------------------------------------- |
| 18     | **[ProjectHub](./5-julio/semana-18-projecthub.md)** | React, FastAPI, PostgreSQL  | App de gestión de proyectos full-stack                |
| 19     | **[LiveDash](./5-julio/semana-19-livedash.md)**     | WebSockets, React, Charts   | Dashboard en tiempo real con gráficos interactivos    |
| 20     | **[SnapTask](./5-julio/semana-20-snaptask.md)**     | Flutter, Dart, FastAPI | App móvil de gestión de tareas con sync en la nube 📱 |
| 21     | **[NotifyHub](./5-julio/semana-21-notifyhub.md)**   | FastAPI, Celery, React      | Sistema de notificaciones multi-canal                 |
| 🤖 AI  | **[GuardAI](./5-julio/mes-05-guardai.md)**          | Presidio, NeMo, OWASP      | Seguridad LLM: PII, guardrails, red-teaming y GDPR   |

---

### [🏆 Agosto — Capstone Projects](./6-agosto/)

> _Proyectos integradores que demuestran dominio completo del stack_

| Semana | Proyecto                                                   | Tecnologías clave              | Descripción                                          |
| ------ | ---------------------------------------------------------- | ------------------------------ | ---------------------------------------------------- |
| 22     | **[AutomateAI](./6-agosto/semana-22-automateai.md)**       | FastAPI, LangChain, Vector DBs | Plataforma de automatización potenciada por IA       |
| 23     | **[APIMarket](./6-agosto/semana-23-apimarket.md)**         | FastAPI, React, Docker         | Agregador de APIs con documentación automática       |
| 24     | **[PortfolioOS](./6-agosto/semana-24-portfolioos.md)**     | React, TailwindCSS, Vercel     | Portfolio personal interactivo y moderno             |
| 25     | **[OpenContrib](./6-agosto/semana-25-opencontrib.md)**     | Git, GitHub, Open Source       | Contribución documentada a proyecto open source      |
| 26     | **[Retrospectiva](./6-agosto/semana-26-retrospectiva.md)** | Markdown, Mermaid, Docs        | Documentación final, métricas y lecciones aprendidas |
| 🤖 AI  | **[TailorAI](./6-agosto/mes-06-tailorai.md)**              | LangGraph, Langfuse, Azure     | Capstone AI: agente + RAG + guardrails end-to-end    |

---

## 🛠️ Stack tecnológico

```
Backend          IA/ML & GenAI       AI Engineering        DevOps & Cloud      Frontend & Mobile
─────────        ──────────────      ──────────────        ──────────────      ─────────────────
Python 3.11+     LangChain           LangGraph             Docker              React
FastAPI          MCP SDK             RAGAS                 GitHub Actions      Flutter 📱
Pydantic         OpenAI API          Langfuse              Linux/Bash          Dart
Rust 🦀          Anthropic API       MS Presidio (PII)     Git                 TailwindCSS
gRPC / Protobuf  Embeddings          NeMo Guardrails       Prometheus          Flutter/Riverpod
Elasticsearch    Vector DBs          DeepEval              Grafana
Stripe API       (Chroma, pgvector)  LLM-as-Judge
```

---

## 📐 Metodología

Cada proyecto sigue la misma estructura para mantener consistencia y calidad profesional:

### Estructura de cada proyecto

```
proyecto/
├── README.md                  # Documentación completa
├── docs/
│   ├── openapi.yaml           # Contrato API First (OpenAPI 3.0)
│   ├── uml/                   # Diagramas UML (PlantUML / Mermaid)
│   │   ├── class-diagram.puml
│   │   └── sequence-diagram.puml
│   └── adr/                   # Architecture Decision Records
├── src/
│   ├── controllers/           # Capa Controller (Request/Response handling)
│   ├── services/              # Capa Service (Lógica de negocio)
│   ├── repositories/          # Capa Repository (Acceso a datos)
│   ├── schemas/               # DTOs (Request DTO / Response DTO)
│   ├── events/                # Publishers / Listeners (Event-Driven)
│   └── models/                # Entidades de dominio
├── migrations/                # Alembic — versionado de esquema de BD
├── tests/
│   ├── unit/                  # Tests unitarios
│   ├── integration/           # Tests de integración (Testcontainers)
│   └── conftest.py            # Fixtures compartidos
├── docker-compose.yml         # Infraestructura local
├── Makefile                   # Comandos comunes
├── .github/
│   └── workflows/             # CI/CD (GitFlow: feature/* → develop → main)
└── .env.example               # Variables de entorno
```

### Definición de "hecho" (DoD)

- [ ] **API First** — Contrato OpenAPI definido antes de codificar
- [ ] **TDD estricto** — Tests escritos antes del código (Rojo→Verde→Refactor)
- [ ] **Layered Architecture** — Separación Controller / Service / Repository / DTO
- [ ] **Migraciones de BD** — Esquema versionado con Alembic (o equivalente)
- [ ] **Eventos** — Al menos un evento de dominio publicado/consumido
- [ ] **Diagramas UML** — Diagrama de clases y/o secuencia por proyecto
- [ ] Tests con cobertura mínima de 80% (integración con Testcontainers)
- [ ] Docker para ejecución local (Docker Compose)
- [ ] README con descripción, setup y uso
- [ ] Código limpio (linting + formatting)
- [ ] Deploy o instrucciones de despliegue
- [ ] **GitFlow** — Ramas feature/*, develop, main; commits asociados a ticket

### Flujo de trabajo semanal

```
Sábado mañana     → Diseño UML, contrato OpenAPI y ADRs (1-2h)
Sábado mañana     → TDD: escribir tests de integración primero (1-2h)
Sábado tarde      → Implementación Layered Architecture (3-4h)
Domingo mañana    → Eventos, migraciones Alembic y Testcontainers (2-3h)
Domingo tarde     → Polish, deploy, README y diagramas finales (2-3h)
```

---

## 📊 Progreso

| Mes       | Proyectos | Completados | Estado         |
| --------- | --------- | ----------- | -------------- |
| 🏗️ Marzo  | 4         | 4/4         | ✅ Finalizado  |
| 🤖 Abril  | 4         | 0/4         | ⬜ Pendiente   |
| 📊 Mayo   | 5         | 0/5         | ⬜ Pendiente   |
| ☁️ Junio  | 4         | 0/4         | ⬜ Pendiente   |
| 🔗 Julio  | 4         | 0/4         | ⬜ Pendiente   |
| 🏆 Agosto | 5         | 0/5         | ⬜ Pendiente   |
| **Total** | **26**    | **4/26**    | **15%**        |

---

## 📜 Licencia

Cada proyecto individual tiene su propia licencia. El plan general y la documentación están bajo [MIT License](./LICENSE).

---