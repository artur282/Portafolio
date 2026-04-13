# 🚀 Portafolio de Ingeniería de Software + AI — Abril a Septiembre 2026

> **6 megaproyectos. 6 meses. Backend Senior + AI Engineer en un solo roadmap consolidado.**

<p align="center">
  <img src="https://img.shields.io/badge/Duración-6_meses-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Proyectos-6_Grandes_Proyectos-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/AI_Track-6_Proyectos_Mensuales-red?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Stack-Python_|_Rust_|_LLMs_|_React_|_DevOps-purple?style=for-the-badge"/>
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

Construir un portafolio de **máxima escalabilidad e impacto B2B**. En lugar de múltiples proyectos pequeños desconectados, construimos **un macro-sistema consolidado mensual**. Cada mes representa una plataforma B2B/B2C lista para producción combinada para demostrar expertise senior en ingeniería conectada, IA integrada y arquitecturas Cloud, DevOps & Full Stack. 

### Principios del portafolio

| Principio                          | Descripción                                                                                        |
| ---------------------------------- | -------------------------------------------------------------------------------------------------- |
| 🏗️ **Producción primero**          | Enfoque industrial. Todo tiene Docker multi-stage, CI/CD, métricas robustas.                       |
| 📈 **Progresión Consolidada**      | Sistemas de complejidad real: Un producto funcional robusto > Múltiples librerías pequeñas aisladas|
| 🔗 **Integración Completa**        | Backends, microservicios iterativos comunicados (Rest + gRPC + Rabbit) conviviendo paralelamente.  |
| 📖 **Documentación Estricta**      | README, UML (Arquitectura, Componentes, Secuencia), Decisiones y OpenAPI nativa exhaustiva.        |
| 🧪 **TDD & Event Driven**          | Driven by Test (Rojo→Verde→Refactor) y Driven by Event (Sagas, RabbitMQ brokers, Pub/Subs).      |
| 🛡️ **Tipado y Compiladores**       | Fuerte adopción del compiler-checker vía Rust o Strong Typing Pydantic con Strict mode.            |

---

## 🗺️ Roadmap visual

```
ABR ──────── MAY ──────── JUN ──────── JUL ──────── AGO ──────── SEP
  │            │            │            │            │            │
  ▼            ▼            ▼            ▼            ▼            ▼
🤖 IA/ML    📊 Datos &  ☁️ DevOps     🔗 Full     🏆 Integración   🦀 Alta
Knowledge   SaaS        & Microserv. Stack       AutoPlatform    Performance
Forge       Forge       Gate         Sync        (Capstone)      RustForge
  │            │            │            │            │            │
  ├─ Python    ├─ RLS       ├─ Rust+Py   ├─ React    ├─ Zapier-like  ├─ Axum
  ├─ VectorDB  ├─ ETL       ├─ API GW    ├─ WebSock  ├─ DevOps+UI    ├─ Async
  └─ MCP SDK   └─ Stripe    └─ gRPC      └─ Móvil    └─ Portfolio    └─ SQLx

🤖 AI TRACK PARALELO
  │            │            │            │            │            │
  └─PromptLab └─ DocuRAG   └─ EvalForge └─ AgentFlow └─ GuardAI   └─ TailorAI
```

---

## 📅 Plan mensual

### [1. Abril — KnowledgeForge (IA/ML & GenAI)](./1-abril/)
Plataforma empresarial de gestión de conocimiento. Ingesta documentos, los indexa con Elasticsearch+pgvector, ofrece RAG avanzado, chatbot, y expone su base vía servidor MCP nativo.

| Track | Proyecto Core/AI | Stack |
| --- | --- | --- |
| 🏗️ MAIN | [KnowledgeForge](./1-abril/proyecto-knowledgeforge.md) | FastAPI, pgvector, LangChain, Elasticsearch, MCP |
| 🤖 AI | [PromptLab](./1-abril/mes-01-promptlab.md) | OpenAI, Anthropic, Evaluación Comparativa |

---

### [2. Mayo — SaaSForge (SaaS Patterns & Data)](./2-mayo/)
API Multitenant pesada. Aísla transacciones implementando Row-Level Security PostgreSQL. Conecta un ETL estructurado y levanta portales de cobro integrados a Stripe asíncronamente con visualización de datos dinámica.

| Track | Proyecto Core/AI | Stack |
| --- | --- | --- |
| 🏗️ MAIN | [SaaSForge](./2-mayo/proyecto-saasforge.md) | PostgreSQL RLS, Stripe, Pandas, ETL, FastAPI |
| 🤖 AI | [DocuRAG](./2-mayo/mes-02-docurag.md) | Cohere Rerank, Advanced Chunking, RAGAS Metrics |

---

### [3. Junio — MicroGate (Cloud & Microservices)](./3-junio/)
Patrones avanzados arquitectónicos "Polyglot". API Gateway en Python protege, autentica y divide cargas a un microservicio escrito absolutamente en Rust (Axum / Distroless). Comunicación inter-servicio super-veloz (gRPC/ProtoBuf). Automatización 100% CI/CD.

| Track | Proyecto Core/AI | Stack |
| --- | --- | --- |
| 🏗️ MAIN | [MicroGate](./3-junio/proyecto-microgate.md) | Rust/Axum, CI/CD, gRPC, Protobuf, Multi-stage Docker |
| 🤖 AI | [EvalForge](./3-junio/mes-03-evalforge.md) | Langfuse Observability, Grafana, A/B Estadístico |

---

### [4. Julio — TeamSync (Full-Stack Integración)](./4-julio/)
Sistema colaborativo integral. Sincroniza tableros de gestión, emitiendo notificaciones en tiempo real al mobile app y la vista dashboard. Conecta brokers de colas (RabbitMQ+Celery) para procesar todo el fondo de forma distribuida de alto nivel. 

| Track | Proyecto Core/AI | Stack |
| --- | --- | --- |
| 🏗️ MAIN | [TeamSync](./4-julio/proyecto-teamsync.md) | React, FastAPI, Flutter, RabbitMQ, WebSockets |
| 🤖 AI | [AgentFlow](./4-julio/mes-04-agentflow.md) | Agente Reactivo Múltiple, Function Calling, Grafos LangGraph |

---

### [5. Agosto — AutoPlatform (Capstone V1)](./5-agosto/)
El primer pico. Hub de flujo de automatizaciones dirigidos con IA (Strategy pattern engine). Levantando adicionalmente el despliegue del código abierto, Marketplace APIs auto-generado y publicando en línea el Portfolio consolidado estático visualizando todos los retos.

| Track | Proyecto Core/AI | Stack |
| --- | --- | --- |
| 🏗️ MAIN | [AutoPlatform](./5-agosto/proyecto-autoplatform.md) | Vercel, Next/React, IA Workflows, OSS |
| 🤖 AI | [GuardAI](./5-agosto/mes-05-guardai.md) | Seguridad LLM Presidio, NeMo, OWASP Prompt Injection |

---

### [6. Septiembre — RustForge (Rust & Performance)](./6-septiembre/)
El cierre total en low-level safe backend. Re-creación nativa y super concurrente de conceptos API usando 100% Rust. Manejo de peticiones asíncronas seguras anti data-races. Interfaces TUIs directas por CLI. Carga en memorias diminutas hiper-optimizadas.

| Track | Proyecto Core/AI | Stack |
| --- | --- | --- |
| 🏗️ MAIN | [RustForge](./6-septiembre/proyecto-rustforge.md) | Rust, Axum, Tokio, SQLx Compile check, Ratatui |
| 🤖 AI | [TailorAI](./6-septiembre/mes-06-tailorai.md) | Capstone Agent, E2E Pipelines, Azure Deploy, Multi-Models |

---

## 🛠️ Stack tecnológico

```text
Backend           IA/ML Track        DevOps & Cloud      Frontend & Integrations
─────────         ───────────        ─────────┴────      ───────────────────────
Python 3.11+      LangChain/Graph    Docker              React, TailwindCSS
FastAPI           MCP SDK            GitHub Actions      Next.js, Vercel
PostgreSQL (RLS)  RAGAS Metrics      GitFlow Branch      WebSockets Async
Rust 🦀 (Axum)    Langfuse Monitor   Linux/Bash          RabbitMQ / Celery
gRPC / Protobuf   Presidio PII       Prometheus Monit    Flutter 📱 / Dart
Elasticsearch     OpenAI / VectorDB  Distroless Build    Stripe Billing SaaS
```

---

## 📐 Metodología

Cada macro-proyecto sigue estructura modular corporativa estandarizada:

```text
/mes-proyecto
├── README.md                  # Descripción particular
├── docs/                      # UML, ADR (Decision Logs) y OpenAPIs
├── src/                       # App
│   ├── web/                   # Capa Gateways (REST, gRPC, Sockets)
│   ├── domain/                # Capa Negocio Agnostic
│   ├── infrastructure/        # Capa Data Brokers, SQL Repos
│   └── scripts/               # Utilidades DB y CLIs
├── tests/                     # Integración Containers y e2e
├── docker-compose.yml         # Dev Environment Start
└── Makefile                   # Entrypoints standards
```

## 📊 Progreso Mensual Consolidado

| Mes | Proyecto Macro | Estado |
| --- | --- | --- |
| 🧠 Abril | KnowledgeForge | ⬜ Pendiente |
| 📊 Mayo | SaaSForge | ⬜ Pendiente |
| ☁️ Junio | MicroGate | ⬜ Pendiente |
| 🔗 Julio | TeamSync | ⬜ Pendiente |
| 🏆 Agosto | AutoPlatform | ⬜ Pendiente |
| 🦀 Septiembre | RustForge | ⬜ Pendiente |
| **Total** | **6 + 6 Proyectos Base**| **0%** |

---

## 📜 Licencia

Cada plataforma retiene MIT u otra bajo directorio. Documentación roadmap MIT by Author.