# 🏆 Mes 06 — TailorAI Platform

> **Capstone: Plataforma AI empresarial end-to-end con agente RAG + guardrails + observabilidad + CI/CD + despliegue en Azure**

| Campo              | Detalle                                                   |
| ------------------ | --------------------------------------------------------- |
| 📅 Mes             | Agosto 2026                                               |
| 🏷️ Categoría       | Capstone — Full Stack AI Engineering                      |
| ⏱️ Tiempo estimado | ~50 horas (1 mes a tiempo parcial, intensivo)             |
| 📦 Repositorio     | [artur282/TailorAI](https://github.com/artur282/TailorAI) |
| 📊 Dificultad      | ⭐⭐⭐⭐⭐ Avanzado                                       |
| 🎯 Competencia     | Arquitectura completa AI + MLOps + Cloud + Documentación  |

---

## 🎯 Caso de uso empresarial

**Contexto:** Este proyecto es el capstone integrador. Toma inspiración directa en la plataforma [Tailor de Emais](https://www.emais.es/tailor/) — una solución de IA para procesos empresariales del sector bancario e inmobiliario.

**Problema de negocio:**
Una empresa de servicios financieros/inmobiliarios necesita una plataforma IA que:

1. **Asesore** a clientes sobre productos disponibles (hipotecas, inversiones, inmuebles)
2. **Extraiga** datos de documentos (contratos, informes, escrituras)
3. **Genere** propuestas comerciales personalizadas
4. **Mida** continuamente su calidad y coste
5. **Cumpla** con GDPR y seguridad bancaria

**Todo integrado y desplegado en Azure, con CI/CD completo.**

**KPIs del sistema en producción:**

- Task Completion Rate ≥ 85%
- Faithfulness (respuestas basadas en documentos) ≥ 0.87
- Coste por interacción ≤ $0.008
- Latencia P95 ≤ 4s
- PII Recall ≥ 0.95 (ningún dato sensible se filtra)
- Uptime ≥ 99.5%

---

## 🏗️ Arquitectura completa

```
┌──────────────────────────────────────────────────────────────────────┐
│                        TailorAI Platform                              │
│                                                                      │
│  ┌───────────────┐    ┌────────────────────────────────────────────┐ │
│  │   Frontend    │    │              API Gateway                   │ │
│  │   (opcional:  │───►│  FastAPI + Rate Limiting + Auth (API Key)  │ │
│  │   Streamlit)  │    └───────────────────────────┬────────────────┘ │
│  └───────────────┘                                │                  │
│                                                   ▼                  │
│  ┌──────────────────────────────────────────────────────────────────┐│
│  │                    GuardAI Middleware (Mes 5)                    ││
│  │         Input Guard → PII Detect → Injection Check              ││
│  └──────────────────────────────────┬───────────────────────────────┘│
│                                     │                                │
│                                     ▼                                │
│  ┌──────────────────────────────────────────────────────────────────┐│
│  │                AgentFlow Core (Mes 4)                            ││
│  │  LangGraph Agent → RAG (DocuRAG Mes 2) → Tools → Response       ││
│  │  Memoria: PostgreSQL (episódica) + pgvector (semántica)          ││
│  └──────────────────────────────────┬───────────────────────────────┘│
│                                     │                                │
│                                     ▼                                │
│  ┌──────────────────────────────────────────────────────────────────┐│
│  │              Output Guard + Observabilidad (Mes 5 + 3)           ││
│  │  Output PII → Langfuse Tracing → Métricas → Alertas             ││
│  └──────────────────────────────────┬───────────────────────────────┘│
│                                     │                                │
└─────────────────────────────────────┼────────────────────────────────┘
                                      │
              ┌───────────────────────▼──────────────────────┐
              │              Azure Cloud                      │
              │                                              │
              │  Azure Container Apps (API)                  │
              │  Azure PostgreSQL Flexible Server             │
              │  Azure Blob Storage (documentos)              │
              │  Azure Monitor + Application Insights         │
              │  Azure Key Vault (secrets)                    │
              └──────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                     CI/CD Pipeline (GitHub Actions)                  │
│                                                                     │
│  Push PR → Run Tests → Run Evaluation (EvalForge) → Deploy Staging  │
│  Si métricas OK → Deploy Prod → Notify + Update Langfuse Dashboard   │
└─────────────────────────────────────────────────────────────────────┘
```

---

## ✨ Features — Integrando todos los meses anteriores

### Core AI Platform

- [ ] **PromptLab integrado (Mes 1)**: catálogo de prompts versionados con Langfuse Prompt Hub
- [ ] **DocuRAG integrado (Mes 2)**: ingesta y Q&A sobre documentos empresariales
- [ ] **AgentFlow integrado (Mes 4)**: agente multi-paso con tools, memoria y LangGraph
- [ ] **GuardAI integrado (Mes 5)**: PII detection, guardrails, audit trail GDPR
- [ ] **EvalForge integrado (Mes 3)**: evaluación continua, alertas y dashboards

### Nuevas capacidades del Capstone

- [ ] **Model selection dinámico**: GPT-4o para preguntas complejas, GPT-4o-mini para simples (reducción de coste)
- [ ] **Prompt caching**: caché de system prompts para reducir tokens y costes
- [ ] **Batching**: procesamiento batch de documentos (ingesta masiva)
- [ ] **Streaming end-to-end** desde el agente hasta el cliente
- [ ] **Multi-tenancy básica**: soporte para múltiples clientes aislados (por tenant_id)
- [ ] **Demo UI**: interfaz mínima con Streamlit para demostración

### Cloud & MLOps

- [ ] **Docker Compose** para desarrollo local (todos los servicios)
- [ ] **Docker Compose para Azure**: deploy con `docker compose up`
- [ ] **Infrastructure as Code**: `docker-compose.azure.yml` + Azure CLI scripts
- [ ] **CI/CD completo** (GitHub Actions):
  - `lint` → `test` → `evaluate` → `build` → `deploy staging`
  - Promotion manual a producción con checklist de métricas
  - Rollback automático si métricas degradan en producción
- [ ] **Secrets management**: Azure Key Vault para API keys
- [ ] **Health checks** y readiness probes

### Documentación y Enablement

- [ ] **README completo** con arquitectura, setup y uso
- [ ] **ADRs** (4 decisiones de arquitectura) documentadas
- [ ] **Playbook operacional**: guía para que negocio use la plataforma
- [ ] **Guía de onboarding** para desarrolladores nuevos
- [ ] **Retrospectiva del portafolio**: qué aprendí, qué haría diferente, próximos pasos

---

## 🛠️ Stack técnico completo

| Capa               | Tecnología                                     |
| ------------------ | ---------------------------------------------- |
| **LLM Providers**  | OpenAI GPT-4o / GPT-4o-mini (routing dinámico) |
| **Agente**         | LangGraph + LangChain                          |
| **RAG**            | pgvector + OpenAI Embeddings + Cohere Rerank   |
| **Guardrails**     | Microsoft Presidio + NeMo Guardrails           |
| **Prompts**        | Langfuse Prompt Hub (versionado central)       |
| **Evaluación**     | RAGAS + LLM-judge + Langfuse Evaluations       |
| **Observabilidad** | Langfuse (full tracing + métricas + alertas)   |
| **API**            | FastAPI + async + rate limiting                |
| **Base de datos**  | Azure PostgreSQL + pgvector                    |
| **Archivos**       | Azure Blob Storage                             |
| **CI/CD**          | GitHub Actions                                 |
| **Cloud hosting**  | Azure Container Apps                           |
| **Secrets**        | Azure Key Vault                                |
| **Monitoring**     | Azure Monitor + Application Insights           |
| **Demo UI**        | Streamlit                                      |

---

## 📁 Estructura del proyecto

```bash
tailorai/
├── app/
│   ├── main.py                          # FastAPI entry point
│   ├── config.py                        # Settings + Azure setup
│   ├── routers/
│   │   ├── agent.py                     # Endpoint principal del agente
│   │   ├── documents.py                 # Ingesta de documentos
│   │   ├── health.py                    # /health + /ready
│   │   └── admin.py                     # Panel admin (métricas, prompts)
│   ├── agent/                           # AgentFlow core (Mes 4)
│   │   ├── graph.py
│   │   ├── nodes/
│   │   └── tools/
│   ├── rag/                             # DocuRAG core (Mes 2)
│   │   ├── ingestor.py
│   │   ├── retriever.py
│   │   └── generator.py
│   ├── security/                        # GuardAI core (Mes 5)
│   │   ├── pii_guard.py
│   │   ├── injection_guard.py
│   │   └── output_guard.py
│   ├── prompts/                         # PromptLab catálogo (Mes 1)
│   │   ├── registry.py                  # Langfuse Prompt Hub client
│   │   └── catalog/
│   ├── evaluation/                      # EvalForge core (Mes 3)
│   │   ├── runner.py
│   │   └── regression.py
│   └── infrastructure/
│       ├── model_router.py              # GPT-4o vs mini routing
│       ├── cache.py                     # Prompt caching
│       └── batch_processor.py          # Bulk document ingestion
├── evals/
│   ├── golden_sets/
│   └── baselines/
├── docs/
│   ├── architecture.md                  # Diagrama completo
│   ├── decisions/                       # ADR-001 a ADR-004
│   ├── playbook.md                      # Manual para negocio
│   ├── developer_guide.md               # Onboarding devs
│   └── retrospective.md                 # Reflexión del portafolio
├── ui/
│   └── streamlit_demo.py               # Demo UI
├── infra/
│   ├── docker-compose.yml               # Desarrollo local
│   ├── docker-compose.azure.yml         # Azure deployment
│   └── azure/
│       ├── provision.sh                 # Azure CLI setup
│       └── keyvault.sh                  # Secrets setup
├── .github/
│   └── workflows/
│       ├── ci.yml                       # Lint + Test + Eval
│       ├── deploy-staging.yml           # Deploy a staging
│       └── deploy-prod.yml              # Deploy a producción (manual)
├── tests/
├── Makefile
└── .env.example
```

---

## 📡 API principal

```http
# Core endpoints
POST   /api/v1/agent/chat            # Chat con el agente (streaming)
POST   /api/v1/documents/ingest      # Subir documento al knowledge base
GET    /api/v1/health                # Health check

# Body de /agent/chat:
# {
#   "tenant_id": "empresa-abc",
#   "session_id": "user-123-session-456",
#   "message": "¿Cuáles son las condiciones de la hipoteca variable para 2026?",
#   "stream": true
# }

# Response (streaming SSE):
# data: {"type": "step", "node": "planner", "content": "Planificando búsqueda..."}
# data: {"type": "step", "node": "retriever", "content": "Recuperando 3 documentos..."}
# data: {"type": "token", "content": "Las condiciones de"}
# data: {"type": "token", "content": " la hipoteca variable..."}
# data: {"type": "done", "sources": ["hipotecas_2026.pdf:p3"], "cost_usd": 0.0062}
```

---

## 🗓️ Plan mensual

### Semana 1 — Integración y arquitectura

- Integrar los 5 proyectos anteriores como módulos
- Resolver conflictos de dependencias y schemas
- Diseñar la interfaz entre capas (contratos Pydantic)
- Setup Azure: resource group, Container Apps, PostgreSQL, Key Vault

### Semana 2 — Nuevas capacidades y Cloud

- Model routing dinámico (GPT-4o vs mini según complejidad query)
- Prompt caching: reducir costes en system prompts repetitivos
- Multi-tenancy básica por tenant_id
- Despliegue en Azure Container Apps con docker-compose.azure.yml

### Semana 3 — CI/CD completo y evaluación en producción

- Pipeline CI/CD completo: test → eval → build → deploy
- EvalForge evaluando el sistema integrado (no los módulos por separado)
- Alertas de producción: si task_success_rate < 0.80 → PagerDuty/Slack
- Regression tests en cada deploy

### Semana 4 — Polish, demo y retrospectiva

- Demo UI con Streamlit: flujo completo demostrable en 5 minutos
- Documentación final: todos los ADRs, playbook, guía de onboarding
- Retrospectiva del portafolio: 6 meses, 6 proyectos, lecciones aprendidas
- Video demo de 3-5 minutos del sistema funcionando end-to-end

---

## 📊 ADRs incluidos (Architecture Decision Records)

| ADR     | Decisión                                         | Alternativas consideradas                                |
| ------- | ------------------------------------------------ | -------------------------------------------------------- |
| ADR-001 | LangGraph vs Autogen vs CrewAI para orquestación | Comparativa de controlabilidad, observabilidad y madurez |
| ADR-002 | pgvector vs Pinecone para producción             | Coste, latencia, integración con PostgreSQL existente    |
| ADR-003 | Azure Container Apps vs AKS vs App Service       | Complejidad operacional vs escalabilidad vs coste        |
| ADR-004 | Presidio vs Gliner vs modelo custom para PII     | Precisión en entidades españolas, facilidad de extensión |

---

## ✅ Definición de "hecho"

- [ ] Sistema integrado funcionando end-to-end localmente con `make up`
- [ ] Desplegado en Azure y accesible por URL pública
- [ ] CI/CD completo: push a main → deploy automático a staging
- [ ] EvalForge: evaluación automática en cada deploy con umbral de calidad
- [ ] Task Completion Rate ≥ 80% en escenarios de prueba empresariales
- [ ] Faithfulness ≥ 0.85 sobre golden set de documentos reales (anonimizados)
- [ ] Demo Streamlit funcional: demostrable en 5 minutos
- [ ] 4 ADRs documentados con decisiones razonadas
- [ ] Playbook de uso para perfiles de negocio
- [ ] Retrospectiva del portafolio completa

---

## 📝 Retrospectiva del portafolio (preview)

Este documento final del portafolio cubrirá:

- **Qué construí**: resumen de los 6 proyectos con enlaces
- **Qué aprendí técnicamente**: las decisiones más difíciles y sus lecciones
- **Qué haría diferente**: errores cometidos y cómo los evitaría
- **Progresión de habilidades**: de "sé qué es RAG" a "construí un sistema RAG evaluado en producción"
- **Próximos pasos**: qué falta para llegar al nivel Senior AI Engineer
- **Métricas del portafolio**: tiempo invertido, proyectos completados, skills adquiridas

---

## 💼 Lo que demuestra al reclutador

| Habilidad                   | Evidencia                                                         |
| --------------------------- | ----------------------------------------------------------------- |
| Arquitectura de sistemas AI | Plataforma end-to-end con 5 capas integradas y documentadas       |
| MLOps y CI/CD               | Pipeline completo: test → eval → deploy → monitoreo en Azure      |
| Cloud (Azure)               | Container Apps + PostgreSQL + Blob + Key Vault + Monitor          |
| Ownership técnico           | Integración de 6 meses de trabajo en un sistema coherente         |
| Comunicación con negocio    | Playbook operacional, demo UI, contexto de caso de uso real       |
| Mejora continua             | Evaluación automática, alertas, A/B testing, regression detection |
| Documentación profesional   | ADRs, playbook, guía de onboarding, retrospectiva                 |
| Orientación a datos         | KPIs medibles para cada módulo del sistema                        |
