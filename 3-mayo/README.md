# 📊 Mayo — Data Engineering & SaaS Patterns

> _"Los datos sin estructura son ruido. Los datos bien procesados son decisiones."_

## 🎯 Objetivo del mes

Dominar la ingeniería de datos y patrones SaaS de producción. Construir pipelines ETL robustos, APIs multi-tenant con Row-Level Security, sistemas de billing con Stripe y APIs de reportes. En paralelo, desarrollar un framework de evaluación y observabilidad para agentes IA.

---

## 📅 Proyectos del mes

### 🏗️ Backend Track (fines de semana)

| Semana | Fechas    | Proyecto                                        | Estado       |
| ------ | --------- | ----------------------------------------------- | ------------ |
| 09     | 2-3 May   | [DataBridge](./semana-09-databridge.md)         | ⬜ Pendiente |
| 10     | 9-10 May  | [TenantAPI](./semana-10-pricewatch.md)          | ⬜ Pendiente |
| 11     | 16-17 May | [PayFlow](./semana-11-flowengine.md)            | ⬜ Pendiente |
| 12     | 23-24 May | [InsightAPI](./semana-12-insightapi.md)         | ⬜ Pendiente |
| 13     | 30-31 May | [LogStream](./semana-13-logstream.md)           | ⬜ Pendiente |

### 🤖 AI Track (entre semana ~40h)

| Proyecto                                | Tecnologías clave                 | Descripción                                                    |
| --------------------------------------- | --------------------------------- | -------------------------------------------------------------- |
| [EvalForge](./mes-03-evalforge.md)      | Langfuse, RAGAS, Grafana, FastAPI | Framework de evaluación y observabilidad con A/B testing       |

---

## 🧠 Habilidades que se desarrollan

**Backend:**
- Diseño de pipelines ETL (Extract, Transform, Load)
- Multi-tenancy con PostgreSQL Row-Level Security
- Billing y suscripciones con Stripe (webhooks, idempotencia)
- SQL avanzado y modelado de datos
- Visualización de datos con Plotly
- Procesamiento y análisis de logs con OpenTelemetry

**AI:**
- Evaluación de LLMs: LLM-as-judge, golden sets, datos sintéticos
- Observabilidad con Langfuse (trazas, coste, latencia)
- A/B testing estadístico (t-test, Cohen's d, Bayesian)
- Detección de regresiones en CI/CD

---

## 🔗 Cómo se conectan los proyectos

```
Backend Track:
  Semana 09: DataBridge
      │  ETL, validación, transformación → fundamentos de datos
      ▼
  Semana 10: TenantAPI
      │  Multi-tenant SaaS, PostgreSQL RLS → aislamiento de datos
      ▼
  Semana 11: PayFlow
      │  Stripe, webhooks, idempotencia → billing como servicio
      ▼
  Semana 12: InsightAPI
      │  Pandas + API → reportes como servicio
      ▼
  Semana 13: LogStream
         Kafka, OpenTelemetry → ingestión y observabilidad

AI Track:
  EvalForge
      Framework de evaluación con RAGAS, LLM-as-judge,
      A/B testing estadístico y dashboards Grafana
```

---

## 📊 Progreso

**Backend:**
- [ ] Semana 09 — DataBridge
- [ ] Semana 10 — TenantAPI
- [ ] Semana 11 — PayFlow
- [ ] Semana 12 — InsightAPI
- [ ] Semana 13 — LogStream

**AI:**
- [ ] EvalForge
