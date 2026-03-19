# 🔍 Semana 08 — SearchMaster

> **Motor de búsqueda full-text con Elasticsearch, FastAPI y relevance tuning**

| Campo              | Detalle             |
| ------------------ | ------------------- |
| 📅 Fechas          | 25-26 de abril 2026 |
| 🏷️ Categoría       | IA/ML & GenAI       |
| ⏱️ Tiempo estimado | 10-12 horas         |
| 📊 Dificultad      | ⭐⭐⭐⭐ Intermedio-alto |

---

## 🎯 Descripción

SearchMaster es un servicio de búsqueda avanzada construido sobre **Elasticsearch**. Indexa catálogos de productos/artículos, ofrece búsqueda full-text con typo-tolerance, autocompletado, faceted search (filtros dinámicos), y relevance tuning configurable. Expone todo mediante una API REST con FastAPI.

Elasticsearch es una de las tecnologías **más solicitadas** en roles Backend Senior. El proyecto aplica **Layered Architecture**, **TDD**, **API First**, **eventos de dominio** (`DocumentIndexed`, `SearchPerformed`), y usa **Testcontainers** para tests de integración con Elasticsearch real.

---

## 🏗️ Arquitectura (Layered Architecture + Elasticsearch)

```
┌──────────────────────────────────────────┐
│           Controllers Layer               │
│  SearchController  IndexController        │
│  Request DTO → Validación → Response DTO  │
├──────────────────────────────────────────┤
│            Services Layer                 │
│  SearchService (query building, boosting) │
│  IndexService (bulk index, mappings)      │
│  SuggestionService (autocomplete)         │
├──────────────────────────────────────────┤
│            Events Layer                   │
│  DocumentIndexed → listener actualiza     │
│                    stats de búsqueda      │
│  SearchPerformed → listener registra      │
│                    analytics              │
├──────────────────────────────────────────┤
│          Repositories Layer               │
│  ElasticsearchRepository (queries DSL)    │
│  ProductRepository → PostgreSQL (Alembic) │
└──────────────────────────────────────────┘
```

---

## ✨ Features

### Búsqueda avanzada

- [ ] Full-text search con typo-tolerance (fuzzy matching)
- [ ] Búsqueda multi-campo (título, descripción, categoría, tags)
- [ ] Boosting configurable (dar más peso a título vs descripción)
- [ ] Highlighting de términos en resultados
- [ ] Soporte multi-idioma (español + inglés)

### Autocompletado y sugerencias

- [ ] Autocomplete en tiempo real (edge n-grams)
- [ ] "Did you mean?" para corrección de errores
- [ ] Sugerencias de búsquedas populares

### Faceted Search (Filtros dinámicos)

- [ ] Filtros por categoría, rango de precio, marca
- [ ] Conteo de resultados por faceta (aggregations)
- [ ] Filtros combinables (AND/OR)
- [ ] Ordenamiento por relevancia, precio, fecha

### Indexación

- [ ] Bulk indexing desde PostgreSQL
- [ ] Mappings y analyzers personalizados
- [ ] Reindexación con zero downtime (aliases)
- [ ] Sincronización incremental (solo cambios)

### Eventos de Dominio

- [ ] `DocumentIndexed` — listener actualiza estadísticas de índice
- [ ] `SearchPerformed` — listener registra analytics de búsqueda
- [ ] `IndexRebuilt` — listener notifica completación

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                         |
| ------------------ | --------------------------------- |
| **FastAPI**        | API REST (Layered Architecture)   |
| **Elasticsearch**  | Motor de búsqueda                 |
| **PostgreSQL**     | Datos maestros (source of truth)  |
| **Alembic**        | Migraciones de esquema BD         |
| **Pydantic**       | DTOs (Request/Response schemas)   |
| **Docker Compose** | ES + PG + API                     |
| **pytest**         | Testing (TDD)                     |
| **Testcontainers** | Tests con Elasticsearch real      |

---

## 📡 Endpoints de la API

```
# Búsqueda
GET    /api/v1/search?q=term&category=x&sort=relevance  # Búsqueda con filtros
GET    /api/v1/search/suggest?q=pho                       # Autocompletado
GET    /api/v1/search/facets?q=term                       # Facetas disponibles

# Indexación
POST   /api/v1/index/products                             # Indexar productos
POST   /api/v1/index/rebuild                              # Reindexar todo
GET    /api/v1/index/stats                                # Estadísticas del índice

# Productos (CRUD en PostgreSQL)
POST   /api/v1/products                                   # Crear producto
GET    /api/v1/products/{id}                               # Obtener producto
PUT    /api/v1/products/{id}                               # Actualizar producto
DELETE /api/v1/products/{id}                               # Eliminar producto
```

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                       |
| -------------- | --------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (clases + secuencia query) + contrato OpenAPI        |
| 🌅 10:00-10:30 | TDD: tests de integración búsqueda + indexación                 |
| 🌅 10:30-12:00 | Alembic: migraciones + Docker Compose (ES + PG)                 |
| 🌞 12:00-13:00 | Mappings y analyzers en Elasticsearch                           |
| 🌞 14:00-16:00 | Layered: SearchService + IndexService + Repositories            |
| 🌆 16:00-18:00 | Full-text search: fuzzy, boosting, highlighting                 |

### Domingo

| Hora           | Actividad                                              |
| -------------- | ------------------------------------------------------ |
| 🌅 9:00-10:30  | Autocompletado (edge n-grams) + "Did you mean?"        |
| 🌅 10:30-12:00 | Faceted search (aggregations) + filtros combinables     |
| 🌞 13:00-14:30 | Eventos: DocumentIndexed, SearchPerformed + analytics   |
| 🌞 14:30-16:00 | Testcontainers: tests con Elasticsearch real            |
| 🌆 16:00-17:00 | README con diagramas UML y curl examples                |

---

## ✅ Definición de "hecho"

- [ ] Contrato OpenAPI definido antes del código (API First)
- [ ] TDD: tests escritos primero (Rojo→Verde→Refactor)
- [ ] Layered Architecture: Controller → Service → Repository → DTO
- [ ] Eventos: DocumentIndexed, SearchPerformed
- [ ] Migraciones versionadas con Alembic (PostgreSQL)
- [ ] Full-text search funcional con typo-tolerance
- [ ] Autocompletado funcional
- [ ] Faceted search con al menos 3 facetas
- [ ] Tests de integración con Testcontainers (Elasticsearch real)
- [ ] Diagramas UML (clases + secuencia de una query)
- [ ] Docker Compose (ES + PG + API)
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                   |
| ---------------------- | ------------------------------------------- |
| **Elasticsearch**      | Full-text, fuzzy, facets, aggregations      |
| Layered Architecture   | Controller → Service → Repository → DTO     |
| TDD / API First        | Tests primero + contrato OpenAPI            |
| Event-Driven           | DocumentIndexed, SearchPerformed            |
| Migraciones BD         | Alembic + sincronización PG → ES            |
| Testcontainers         | Tests con Elasticsearch real                |
| Relevancia industrial  | ES es requisito frecuente en Backend Senior |
