# 📈 Semana 12 — InsightAPI

> **API de reportes dinámicos con Layered Architecture, Pandas y exportación multi-formato**

| Campo              | Detalle                       |
| ------------------ | ----------------------------- |
| 📅 Fechas          | 23-24 de mayo 2026            |
| 🏷️ Categoría       | Data Engineering & Automation |
| ⏱️ Tiempo estimado | 10-12 horas                   |
| 📊 Dificultad      | ⭐⭐⭐ Intermedio             |

---

## 🎯 Descripción

InsightAPI es una API que genera reportes dinámicos a partir de datos almacenados en PostgreSQL. Los usuarios pueden solicitar reportes configurables (filtros, agrupaciones, métricas), obtener visualizaciones como gráficos interactivos (Plotly), y exportar los resultados en múltiples formatos (JSON, CSV, PDF).

El proyecto aplica **Layered Architecture** estricta (ReportController → ReportService → ReportRepository), utiliza **Alembic** para el esquema de datos de ventas, **API First** con OpenAPI, y **TDD** con flujo Rojo→Verde→Refactor. Documenta la arquitectura con un **diagrama de clases UML** del engine de reportes.

---

## 🏗️ Arquitectura (Layered Architecture)

```
┌───────────────────────────────────────┐
│         Controllers Layer              │
│  ReportController                      │
│  Request DTO (filtros, agrupación)     │
│  → Response DTO (datos + gráficos)    │
├───────────────────────────────────────┤
│          Services Layer                │
│  ReportService (lógica de agregación) │
│  ExportService (PDF, CSV, Excel)      │
│  ChartService (Plotly)                │
├───────────────────────────────────────┤
│         Repositories Layer             │
│  ReportRepository (queries complejas) │
│  → PostgreSQL (Alembic migrations)    │
└───────────────────────────────────────┘
```

---

## ✨ Features

### Reportes

- [ ] Reportes configurables por parámetros (filtros, rango de fechas, agrupación)
- [ ] Métricas calculadas (suma, promedio, conteo, percentiles)
- [ ] Agrupación por dimensiones (categoría, fecha, región)
- [ ] Comparación de períodos (mes actual vs anterior)
- [ ] Templates de reportes predefinidos

### Visualizaciones

- [ ] Gráficos de barras, líneas, pie (Plotly)
- [ ] Gráficos interactivos embebidos en HTML
- [ ] Exportación de gráficos como imagen (PNG)

### Exportación

- [ ] JSON (datos crudos)
- [ ] CSV (descarga directa)
- [ ] Excel (.xlsx)
- [ ] PDF con formato (ReportLab/WeasyPrint)

### Datos de ejemplo

- [ ] Dataset de ventas ficticio (seed via Alembic)
- [ ] Generador de datos de prueba

---

## 🛠️ Stack técnico

| Tecnología       | Propósito                         |
| ---------------- | --------------------------------- |
| **FastAPI**      | API REST (Layered Architecture)   |
| **Pandas**       | Procesamiento y agregación        |
| **Plotly**       | Visualizaciones                   |
| **PostgreSQL**   | Base de datos                     |
| **Alembic**      | Migraciones de esquema BD         |
| **SQLAlchemy**   | ORM + Repository pattern          |
| **Pydantic**     | DTOs (Request/Response schemas)   |
| **ReportLab**    | Generación de PDF                 |
| **openpyxl**     | Exportación Excel                 |
| **Docker**       | Containerización                  |
| **pytest**       | Testing (TDD)                     |
| **Testcontainers**| Tests integración con PostgreSQL |

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                    |
| -------------- | ------------------------------------------------------------ |
| 🌅 9:00-10:00  | Diseño UML (clases engine reportes) + contrato OpenAPI       |
| 🌅 10:00-10:30 | TDD: tests de integración de reportes                        |
| 🌅 10:30-12:00 | Alembic: migraciones + seed de ventas                        |
| 🌞 12:00-13:00 | Layered: ReportController + ReportService + ReportRepository |
| 🌞 14:00-16:00 | Engine con Pandas (filtros, agrupación, métricas)            |
| 🌆 16:00-18:00 | Visualizaciones con Plotly + API endpoints                   |

### Domingo

| Hora           | Actividad                                          |
| -------------- | -------------------------------------------------- |
| 🌅 9:00-10:30  | Exportación: CSV, JSON, Excel (ExportService)      |
| 🌅 10:30-12:00 | Exportación: PDF con formato                       |
| 🌞 13:00-14:30 | Templates de reportes predefinidos                 |
| 🌞 14:30-16:00 | Testcontainers + tests completos (TDD)             |
| 🌆 16:00-17:00 | README con diagramas UML y screenshots             |

---

## ✅ Definición de "hecho"

- [ ] Contrato OpenAPI definido antes del código (API First)
- [ ] TDD: tests escritos primero (Rojo→Verde→Refactor)
- [ ] Layered Architecture: ReportController → ReportService → ReportRepository
- [ ] DTOs estrictos: Request/Response con Pydantic
- [ ] Migraciones versionadas con Alembic
- [ ] Al menos 3 tipos de reportes configurables
- [ ] Tests de integración con Testcontainers
- [ ] Diagrama de clases UML del engine de reportes
- [ ] Docker Compose funcional
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad            | Evidencia                             |
| -------------------- | ------------------------------------- |
| Layered Architecture | Controller → Service → Repository     |
| TDD / API First      | Tests primero + contrato OpenAPI      |
| Migraciones BD       | Alembic con versionado de esquema     |
| UML                  | Diagrama de clases del engine         |
| Testcontainers       | Tests con PostgreSQL real             |
| Análisis de datos    | Pandas, agregaciones, métricas        |
| Exportación          | Multi-formato (CSV, PDF, Excel)       |
