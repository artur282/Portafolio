# 🌉 Semana 09 — DataBridge

> **Pipeline ETL completo con validación, transformación y carga entre sistemas**

| Campo              | Detalle                       |
| ------------------ | ----------------------------- |
| 📅 Fechas          | 2-3 de mayo 2026              |
| 🏷️ Categoría       | Data Engineering & Automation |
| ⏱️ Tiempo estimado | 10-12 horas                   |
| 📊 Dificultad      | ⭐⭐⭐ Intermedio             |

---

## 🎯 Descripción

DataBridge es un pipeline ETL (Extract, Transform, Load) profesional que migra datos entre diferentes fuentes: archivos CSV/JSON, bases SQLite y PostgreSQL. Incluye validación de datos, transformaciones configurables, manejo de errores con rollback, logging detallado y reportes de ejecución.

Simula un escenario real de migración de datos empresariales. El proyecto aplica el **Strategy Pattern** (GoF) para extractores y transformadores intercambiables, emite **eventos de dominio** (`ExtractCompleted`, `LoadCompleted`) para desacoplar las etapas del pipeline, y usa **Alembic** para versionar el esquema destino en PostgreSQL.

---

## 🏗️ Arquitectura (Pipeline Pattern + Strategy + Event-Driven)

```
┌──────────────────────────────────────────────────────┐
│                    Pipeline ETL                       │
│                                                      │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐       │
│  │ EXTRACT  │───▶│TRANSFORM │───▶│   LOAD   │       │
│  │ Strategy │    │ Strategy │    │ Strategy │       │
│  │          │    │          │    │          │       │
│  │ • CSV    │    │ • Clean  │    │ • PostgreSQL│     │
│  │ • JSON   │    │ • Validate│   │ • Batch   │     │
│  │ • SQLite │    │ • Convert │   │ • Upsert  │     │
│  │ • API    │    │ • Enrich  │   │ • Rollback│     │
│  └──────────┘    └──────────┘    └──────────┘       │
│       │               │               │              │
│  Event:          Event:          Event:               │
│  ExtractCompleted TransformDone  LoadCompleted        │
│                                                      │
│              ┌────────────────────┐                   │
│              │   Orchestrator      │  ← Service Layer │
│              │   (PipelineService) │                   │
│              └────────┬───────────┘                   │
│                       │                              │
│       ┌───────────────┼───────────────┐              │
│       │               │               │              │
│  ┌────▼────┐    ┌────▼────┐    ┌────▼────┐         │
│  │ Logger  │    │ Reporter│    │ Monitor │         │
│  └─────────┘    └─────────┘    └─────────┘         │
└──────────────────────────────────────────────────────┘
```

---

## ✨ Features

### Extract (Extracción) — Strategy Pattern

- [ ] Lectura de archivos CSV con diferentes encodings
- [ ] Lectura de archivos JSON y JSON Lines
- [ ] Extracción desde SQLite
- [ ] Extracción desde APIs REST
- [ ] Lectores como estrategias intercambiables (Strategy Pattern — GoF)

### Transform (Transformación)

- [ ] Limpieza de datos (nulls, duplicados, whitespace)
- [ ] Validación con schemas (Pydantic/Pandera)
- [ ] Conversión de tipos de datos
- [ ] Normalización de campos (fechas, monedas, emails)
- [ ] Enriquecimiento (campos calculados)
- [ ] Transformaciones configurables por YAML

### Load (Carga)

- [ ] Carga a PostgreSQL con esquema versionado (Alembic)
- [ ] Inserción batch (bulk insert)
- [ ] Upsert (insert or update)
- [ ] Transacciones con rollback en caso de error
- [ ] Exportación a CSV/JSON (alternativa)

### Eventos de Dominio (Event-Driven Architecture)

- [ ] `ExtractCompleted` — disparado tras extracción, listener inicia transformación
- [ ] `TransformCompleted` — disparado tras transformación, listener inicia carga
- [ ] `LoadCompleted` — disparado tras carga exitosa, listener genera reporte
- [ ] `PipelineError` — disparado en error, listener ejecuta rollback

### Orquestación

- [ ] Pipeline configurable por YAML
- [ ] Ejecución por CLI
- [ ] Logging detallado (por paso y por fila)
- [ ] Reporte de ejecución (filas procesadas, errores, duración)
- [ ] Modo dry-run (validar sin cargar)

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                 |
| ------------------ | ------------------------- |
| **Python 3.11+**   | Lenguaje base             |
| **Pandas**         | Procesamiento de datos    |
| **Pandera**        | Validación de schemas     |
| **SQLAlchemy**     | Conexión a bases de datos |
| **PostgreSQL**     | Destino principal         |
| **Alembic**        | Migraciones de esquema BD |
| **SQLite**         | Fuente de ejemplo         |
| **Typer**          | CLI                       |
| **Rich**           | Visualización de progreso |
| **Docker Compose** | Infraestructura           |
| **pytest**         | Testing (TDD)             |
| **Testcontainers** | Tests integración con PG  |

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                      |
| -------------- | -------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (secuencia pipeline + clases Strategy) + OpenAPI    |
| 🌅 10:00-10:30 | TDD: tests de integración del flujo ETL completo              |
| 🌅 10:30-12:00 | Alembic: setup + migraciones para esquema destino              |
| 🌞 12:00-13:00 | Extractores: CSV, JSON, SQLite (Strategy Pattern)              |
| 🌞 14:00-16:00 | Transformadores: limpieza, validación, normalización           |
| 🌆 16:00-18:00 | Loaders: PostgreSQL (batch + upsert) + eventos de dominio      |

### Domingo

| Hora           | Actividad                                                 |
| -------------- | --------------------------------------------------------- |
| 🌅 9:00-10:30  | Pipeline orchestrator (PipelineService) + configuración   |
| 🌅 10:30-12:00 | CLI + modo dry-run                                        |
| 🌞 13:00-14:30 | Testcontainers: tests con PostgreSQL real + Logging       |
| 🌞 14:30-16:00 | Eventos: ExtractCompleted, LoadCompleted + listeners      |
| 🌆 16:00-17:00 | README con diagramas UML y ejemplos                       |

---

## ✅ Definición de "hecho"

- [ ] TDD: tests escritos primero (Rojo→Verde→Refactor)
- [ ] Strategy Pattern para extractores/transformadores/loaders
- [ ] Eventos de dominio: ExtractCompleted, LoadCompleted
- [ ] Migraciones versionadas con Alembic
- [ ] Pipeline funcional CSV → Transform → PostgreSQL
- [ ] Tests de integración con Testcontainers
- [ ] Diagramas UML (secuencia + clases Strategy)
- [ ] CLI con modo normal y dry-run
- [ ] Docker Compose funcional
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad            | Evidencia                                    |
| -------------------- | -------------------------------------------- |
| Patrones GoF         | Strategy Pattern para extractores/loaders    |
| Event-Driven         | Eventos de dominio entre etapas del pipeline |
| Migraciones BD       | Alembic con versionado de esquema            |
| Testcontainers       | Tests de integración con PostgreSQL real     |
| TDD                  | Tests escritos antes del código              |
| ETL                  | Pipeline completo extract → transform → load |
| SQL                  | PostgreSQL, batch insert, upsert             |
