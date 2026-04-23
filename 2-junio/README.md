# 📊 Junio — Data Engineering & SaaS Patterns

> _"Los datos sin estructura son ruido. Las APIs sin aislamiento son un riesgo."_

## 🎯 Objetivo del mes

Dominar los patrones de infraestructura SaaS: aislamiento de datos entre clientes con Row-Level Security, control de acceso basado en plan de suscripción (cuotas), ingesta masiva de datos via ETL y prevención de race conditions con distributed locks en Redis.

---

## 📅 Proyectos del mes

### 🏗️ Proyecto Principal: [SaaSForge](./proyecto-saasforge.md)
Plataforma SaaS multi-tenant con aislamiento RLS en PostgreSQL. Reserva concurrente de recursos con Distributed Lock Redis (prevención de overbooking), cuotas de uso por plan con sliding window, pipelines ETL validados con Pydantic/Pandas y observabilidad con OpenTelemetry.

- **Tecnologías**: FastAPI, PostgreSQL (RLS), Redis (SETNX distributed lock), Pandas, SQLAlchemy async, OpenTelemetry
- **Estado**: ⬜ Pendiente

---

## 🧠 Habilidades que se desarrollan

- Diseño Multi-tenant con PostgreSQL RLS (Row-Level Security) — aislamiento a nivel de base de datos.
- Distributed Locks con Redis SETNX: prevención de race conditions bajo alta concurrencia.
- Rate limiting con sliding window (sorted sets Redis) — cuotas por plan de suscripción.
- Creación de procesos ETL validados con Pydantic v2 y Pandas — rechazando datos inválidos.
- Observabilidad con OpenTelemetry: trazas por request, spans por query SQL lenta.
