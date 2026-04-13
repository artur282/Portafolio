# 📊 Mayo — Data Engineering & SaaS Patterns

> _"Los datos sin estructura son ruido. Las APIs sin aislamiento son un riesgo."_

## 🎯 Objetivo del mes

Dominar la ingeniería de datos, el aislamiento y los patrones comerciales de un Software-as-a-Service (SaaS) completo. Enseña cómo orquestar transferencias de datos seguras, manejar múltiples clientes (multi-tenant) sobre la misma infra y gestionar suscripciones con pasarelas de pago.

---

## 📅 Proyectos del mes

### 🏗️ Proyecto Principal: [SaaSForge](./proyecto-saasforge.md)
Plataforma SaaS multi-tenant lista para producción con aislamiento RLS nativo en PostgreSQL. Ofrece pipelines ETL robustos, integración de billing de Stripe y generación dinámica de visualizaciones y analíticas usando Plotly.

- **Tecnologías**: FastAPI, PostgreSQL (RLS), Stripe API, Webhooks, Pandas, Plotly, SQLite
- **Estado**: ⬜ Pendiente

### 🤖 AI Track: [DocuRAG](./mes-02-docurag.md)
Pipeline RAG (Retrieval-Augmented Generation) avanzado sobre documentos utilizando estrategias complejas de chunking, evaluación estricta con RAGAS e implementando Semantic Re-ranking.

- **Tecnologías**: RAGAS, pgvector, Cohere
- **Estado**: ⬜ Pendiente

---

## 🧠 Habilidades que se desarrollan

- Diseño Multi-tenant con PostgreSQL RLS (Row-Level Security).
- Creación de procesos ETL (Extract, Transform, Load) seguros mediante Python Pandas.
- Integración robusta de pagos a través de Webhooks de Stripe e idempotencia.
- Visualización de grandes conjuntos de datos de forma dinámica consumible vía API.
- Evaluación programática de sistemas GenAI con métricas científicas.
