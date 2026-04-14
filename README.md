# 🚀 Portafolio de Ingeniería de Software + AI — Abril a Septiembre 2026

> **6 megaproyectos. 6 meses. Backend Senior + AI Engineer en un solo roadmap consolidado.**

<p align="center">
  <img src="https://img.shields.io/badge/Duración-6_meses-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Proyectos-6_Sistemas_de_Producci%C3%B3n-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Stack-Python_|_Rust_|_LLMs_|_Kafka_|_gRPC-purple?style=for-the-badge"/>
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
🤖 RAG/MCP  📊 SaaS+     ☁️ API GW    🚗 IoT       🏆 Automation  🎯 Vector
Knowledge   Booking      MicroGate   Stream       AutoPlatform   Search
Forge       Forge        Polyglot    (Rust+Kafka) (Capstone)     VectoRust
  │            │            │            │            │            │
  ├─ Python    ├─ RLS+Redis ├─ Rust+Py   ├─ Rust      ├─ LCEL chain  ├─ Rust SIMD
  ├─ pgvector  ├─ Redlock   ├─ gRPC      ├─ Kafka     ├─ Next.js ISR  ├─ gRPC tonic
  └─ MCP SDK   └─ Redis     └─ Distroless └─ Timescale └─ Portfolio   └─ Benchmarks
```

---

## 📅 Plan mensual

### [1. Abril — KnowledgeForge (IA/ML & GenAI)](./1-abril/)
Plataforma empresarial de gestión de conocimiento. Ingesta documentos, los indexa con Elasticsearch+pgvector, ofrece RAG avanzado, chatbot, y expone su base vía servidor MCP nativo.

**Stack:** FastAPI · pgvector · LangChain · Elasticsearch · MCP SDK

---

### [2. Mayo — SaaSForge (SaaS Patterns & Data)](./2-mayo/)
Plataforma SaaS multi-tenant. Aislamiento de datos por Row-Level Security (RLS) en PostgreSQL, reserva concurrente de recursos con Distributed Lock (Redis SETNX) que previene overbooking bajo alta concurrencia, cuotas de uso por plan con sliding window, y pipeline ETL validado con Pandas.

**Stack:** PostgreSQL RLS · Redis SETNX · Pandas · FastAPI · OpenTelemetry

---

### [3. Junio — MicroGate (Cloud & Microservices)](./3-junio/)
Patrones avanzados arquitectónicos "Polyglot". API Gateway en Python protege, autentica y divide cargas a un microservicio escrito absolutamente en Rust (Axum / Distroless). Comunicación inter-servicio super-veloz (gRPC/ProtoBuf). Automatización 100% CI/CD.

**Stack:** Rust/Axum · FastAPI · gRPC/Protobuf · Redis · Distroless Docker · GitHub Actions

---

### [4. Julio — IoTStream (IoT & Real-Time Data Engineering)](./4-julio/)
Plataforma de telemetría vehicular. Rust/Axum ingesta miles de eventos/seg desde sensores, Kafka garantiza exactly-once delivery con 6 particiones, TimescaleDB persiste con continuous aggregates automáticos y compresión nativa, FastAPI expone analytics predictivos de mantenimiento preventivo vehicular.

**Stack:** Rust/Axum · rdkafka · Apache Kafka · TimescaleDB · FastAPI · Grafana

---

### [5. Agosto — AutoPlatform (Capstone V1)](./5-agosto/)
El primer pico. Hub de flujo de automatizaciones dirigidos con IA (Strategy pattern engine). Levantando adicionalmente el despliegue del código abierto, Marketplace APIs auto-generado y publicando en línea el Portfolio consolidado estático visualizando todos los retos.

**Stack:** Next.js 15 · FastAPI · LangChain LCEL · GoF Strategy · Vercel

---

### [6. Septiembre — VectoRust (Semantic Search & High-Performance Rust)](./6-septiembre/)
Motor de búsqueda vectorial semántica construido desde cero en Rust con SIMD. Expuesto vía gRPC (tonic). Python/FastAPI orquesta la extracción de embeddings (OpenAI) y el enriquecimiento de resultados. Benchmark documentado FLAT-SIMD vs pgvector HNSW con ADR de tradeoffs. Dominio: recomendaciones de e-commerce (500K productos).

**Stack:** Rust SIMD · tonic gRPC · FastAPI · pgvector · OpenAI Embeddings

---

## 🛠️ Stack tecnológico

```text
Backend           IA/ML Track        DevOps & Cloud      Distributed Systems
─────────         ───────────        ───────────      ───────────────────
Python 3.11+      LangChain/Graph    Docker              Redis SETNX Locks
FastAPI           MCP SDK            GitHub Actions      Kafka (exactly-once)
PostgreSQL (RLS)  RAGAS Metrics      GitFlow Branch      TimescaleDB
Rust 🦀 (Axum)    Langfuse Monitor   Linux/Bash          gRPC / tonic
gRPC / Protobuf   Presidio PII       Prometheus Monit    Dist. Locking
Elasticsearch     OpenAI / VectorDB  Distroless Build    OpenTelemetry
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

| Mes | Proyecto Macro | Stack clave | Estado |
| --- | --- | --- | --- |
| 🤖 Abril | KnowledgeForge | FastAPI · pgvector · LangChain · MCP | ⬜ Pendiente |
| 📊 Mayo | SaaSForge | PostgreSQL RLS · Redis SETNX · Pandas | ⬜ Pendiente |
| ☁️ Junio | MicroGate | Rust Axum · FastAPI · gRPC · Distroless | ⬜ Pendiente |
| 🚗 Julio | IoTStream | Rust · Kafka · TimescaleDB · FastAPI | ⬜ Pendiente |
| 🏆 Agosto | AutoPlatform | Next.js ISR · LangChain · GoF Strategy | ⬜ Pendiente |
| 🎯 Septiembre | VectoRust | Rust SIMD · gRPC tonic · pgvector · OpenAI | ⬜ Pendiente |
| **Total** | **6 Proyectos** | **Python · Rust · LLMs · Kafka · gRPC** | **0%** |

---

## 📜 Licencia

Cada plataforma retiene MIT u otra bajo directorio. Documentación roadmap MIT by Author.