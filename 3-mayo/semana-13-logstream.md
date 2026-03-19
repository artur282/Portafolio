# 📝 Semana 13 — LogStream

> **Sistema de ingestión de logs con Kafka, observabilidad (OpenTelemetry) y Circuit Breaker**

| Campo              | Detalle                       |
| ------------------ | ----------------------------- |
| 📅 Fechas          | 30-31 de mayo 2026            |
| 🏷️ Categoría       | Data Engineering & Automation |
| ⏱️ Tiempo estimado | 10-12 horas                   |
| 📊 Dificultad      | ⭐⭐⭐⭐ Intermedio-alto      |

---

## 🎯 Descripción

LogStream es un sistema que ingesta logs de aplicaciones (formato JSON structured logging), los almacena en PostgreSQL, y provee una API para consultarlos con filtros avanzados. Incluye un generador de logs simulados, dashboards de métricas básicas y alertas configurables por patrones de error.

**Este es un proyecto clave para la propuesta laboral.** Utiliza **Apache Kafka** como broker de ingestión de alto rendimiento, instrumenta logs con **OpenTelemetry** y **TraceIDs** para correlación distribuida, implementa **Circuit Breaker** para resiliencia cuando el broker está caído, y garantiza **idempotencia** en el procesamiento de eventos. Aplica **Layered Architecture**, **Alembic** para migraciones, y **Testcontainers** para Kafka + PostgreSQL.

---

## 🏗️ Arquitectura (Layered + Event-Driven + Observabilidad)

```
┌─────────────────────────────────────────────────────┐
│                Controllers Layer                     │
│  LogIngestionController  LogQueryController           │
│  Request DTO → Validación → Response DTO             │
├─────────────────────────────────────────────────────┤
│                 Services Layer                       │
│  IngestionService  QueryService  AlertService        │
│  + OpenTelemetry TraceID propagation                │
├─────────────────────────────────────────────────────┤
│              Event Broker Layer                      │
│  ┌──────────┐    ┌───────────────┐                  │
│  │  Kafka    │───▶│ Consumer      │                  │
│  │ Producer  │    │ (idempotente) │                  │
│  └──────────┘    └───────┬───────┘                  │
│       │  Circuit Breaker │                           │
│       │  (si Kafka caído)│                           │
├───────┼──────────────────┼──────────────────────────┤
│       ▼                  ▼                           │
│          Repositories Layer                          │
│  LogRepository → PostgreSQL (Alembic)                │
│  Índices optimizados (timestamp, level, service)     │
├─────────────────────────────────────────────────────┤
│            Observabilidad                            │
│  OpenTelemetry → Traces + Metrics                    │
│  Prometheus → Grafana (métricas)                     │
│  TraceID en cada log para correlación                │
└─────────────────────────────────────────────────────┘
```

---

## ✨ Features

### Ingestión (Kafka + Event-Driven)

- [ ] API para recibir logs (POST batch)
- [ ] Kafka Producer: publicar logs al topic `logs.ingest`
- [ ] Kafka Consumer: consumir y persistir con idempotencia (deduplicación por ID)
- [ ] Circuit Breaker: fallback a escritura directa si Kafka está caído
- [ ] Soporte de logs estructurados (JSON)
- [ ] Niveles: DEBUG, INFO, WARNING, ERROR, CRITICAL
- [ ] Metadatos: service, timestamp, trace_id, tags
- [ ] Generador de logs simulados para demo

### Almacenamiento (PostgreSQL + Alembic)

- [ ] Esquema versionado con Alembic
- [ ] Índices para búsqueda rápida (timestamp, level, service)
- [ ] Retención configurable (auto-limpieza de logs antiguos)
- [ ] Particionamiento por fecha (si alcanza el tiempo)

### Consulta y análisis

- [ ] Búsqueda por rango de tiempo, nivel, servicio
- [ ] Búsqueda por texto en mensaje
- [ ] Filtrado por tags y metadatos
- [ ] **Correlación por TraceID** (ver todos los logs de una transacción)
- [ ] Agrupación por servicio/nivel (conteos)
- [ ] Detección de picos de errores

### Observabilidad (OpenTelemetry + Prometheus)

- [ ] Instrumentación con OpenTelemetry SDK
- [ ] TraceID propagado en headers y logs
- [ ] Métricas expuestas a Prometheus (logs/segundo, errores, latencia)
- [ ] Dashboard Grafana basico para visualizar métricas
- [ ] Health check con estado de Kafka y PostgreSQL

### Alertas

- [ ] Reglas configurables (ej: >10 errores en 1 minuto)
- [ ] Notificación por log/webhook
- [ ] Historial de alertas disparadas

### Resiliencia

- [ ] **Circuit Breaker** — Patrón para degradación elegante si Kafka falla
- [ ] **Retry** con backoff exponencial para publicación a Kafka
- [ ] **Idempotencia** — Cada log tiene ID único, deduplicación en consumer

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                             |
| ------------------ | ------------------------------------- |
| **FastAPI**        | API REST (Layered Architecture)       |
| **Apache Kafka**   | Broker de ingestión de logs           |
| **PostgreSQL**     | Almacenamiento de logs                |
| **Alembic**        | Migraciones de esquema BD             |
| **OpenTelemetry**  | Tracing distribuido + métricas        |
| **Prometheus**     | Recolección de métricas               |
| **Grafana**        | Dashboard de observabilidad           |
| **SQLAlchemy**     | ORM + Repository pattern              |
| **APScheduler**    | Tareas periódicas (limpieza, alertas) |
| **Docker Compose** | Kafka + PG + Prometheus + Grafana     |
| **pytest**         | Testing (TDD)                         |
| **Testcontainers** | Tests con Kafka + PostgreSQL reales   |

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                     |
| -------------- | ------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (secuencia ingestión con Kafka) + contrato OpenAPI |
| 🌅 10:00-10:30 | TDD: tests de integración del flujo Kafka → PG               |
| 🌅 10:30-12:00 | Alembic: migraciones + Docker Compose (Kafka + PG + Prom)     |
| 🌞 12:00-13:00 | Kafka Producer + Consumer con idempotencia                    |
| 🌞 14:00-16:00 | Layered: Controllers + Services + Repositories                |
| 🌆 16:00-18:00 | Circuit Breaker + Retry + generador de logs                   |

### Domingo

| Hora           | Actividad                                                 |
| -------------- | --------------------------------------------------------- |
| 🌅 9:00-10:30  | OpenTelemetry: instrumentación + TraceID propagation      |
| 🌅 10:30-12:00 | Prometheus: métricas + Grafana dashboard básico           |
| 🌞 13:00-14:30 | API de consulta: filtros, correlación por TraceID         |
| 🌞 14:30-16:00 | Testcontainers: tests con Kafka + PostgreSQL reales       |
| 🌆 16:00-17:00 | README con diagramas UML, métricas y documentación        |

---

## ✅ Definición de "hecho"

- [ ] Contrato OpenAPI definido antes del código (API First)
- [ ] TDD: tests escritos primero (Rojo→Verde→Refactor)
- [ ] Layered Architecture: Controller → Service → Repository → DTO
- [ ] Apache Kafka como broker de ingestión
- [ ] Idempotencia en consumer (deduplicación por ID)
- [ ] Circuit Breaker + Retry para resiliencia
- [ ] OpenTelemetry: TraceID en logs y headers
- [ ] Prometheus + Grafana para métricas
- [ ] Migraciones versionadas con Alembic
- [ ] Tests de integración con Testcontainers (Kafka + PostgreSQL)
- [ ] Diagramas UML (secuencia ingestión con Kafka)
- [ ] Docker Compose con todos los servicios
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                    |
| ---------------------- | -------------------------------------------- |
| **Apache Kafka**       | Broker de ingestión, producer/consumer        |
| **Idempotencia**       | Deduplicación de eventos en consumer          |
| **Circuit Breaker**    | Resiliencia ante fallo de Kafka               |
| **Observabilidad**     | OpenTelemetry, TraceID, Prometheus, Grafana   |
| Layered Architecture   | Controller → Service → Repository             |
| Event-Driven           | Kafka como bus de eventos                     |
| Migraciones BD         | Alembic con versionado de esquema             |
| Testcontainers         | Tests con Kafka + PostgreSQL reales           |
| SQL avanzado           | Índices, particionamiento, queries complejas  |
