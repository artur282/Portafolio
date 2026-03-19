# 📚 Semana 07 — DocuMind

> **Pipeline RAG para Q&A sobre documentos con embeddings y búsqueda semántica**

| Campo              | Detalle                  |
| ------------------ | ------------------------ |
| 📅 Fechas          | 18-19 de abril 2026      |
| 🏷️ Categoría       | IA/ML & GenAI            |
| ⏱️ Tiempo estimado | 10-12 horas              |
| 📊 Dificultad      | ⭐⭐⭐⭐ Intermedio-alto |

---

## 🎯 Descripción

DocuMind es una aplicación de Retrieval Augmented Generation (RAG) que permite hacer preguntas en lenguaje natural sobre documentos (PDFs, Markdown, texto). Ingesta documentos, los divide en chunks, genera embeddings, los almacena en una vector store, y usa búsqueda semántica para encontrar el contexto relevante antes de generar respuestas con un LLM.

El proyecto implementa **Layered Architecture** con servicios especializados (IngestionService, RetrievalService, GenerationService), utiliza **Alembic** para versionado de metadatos en PostgreSQL, define el contrato **API First** antes de codificar, y emite **eventos de dominio** para desacoplar las etapas del pipeline.

---

## 🏗️ Arquitectura (Layered Architecture + Event-Driven Pipeline)

```
                    ┌─────────────┐
                    │  Documentos  │
                    │ (PDF/MD/TXT) │
                    └──────┬──────┘
                           │
              ┌────────────▼────────────┐
              │   IngestionController    │  ← Controller Layer
              │   (Upload endpoints)     │
              └────────────┬────────────┘
                           │
              ┌────────────▼────────────┐
              │   IngestionService       │  ← Service Layer
              │   Load → Split → Embed  │
              │   Publica: DocumentIngested
              └────────────┬────────────┘
                           │
              ┌────────────▼────────────┐
              │   RetrievalService       │  ← Service Layer
              │   Búsqueda semántica     │
              └────────────┬────────────┘
                           │
              ┌────────────▼────────────┐
              │   GenerationService      │  ← Service Layer
              │   RAG Chain (LangChain)  │
              │   Publica: QueryProcessed│
              └────────────┬────────────┘
                           │
         ┌─────────────────┼─────────────────┐
         │                 │                 │
    ┌────▼─────┐    ┌──────▼──────┐   ┌──────▼──────┐
    │ Document │    │   Vector    │   │  PostgreSQL  │
    │Repository│    │   Store     │   │  (Alembic)   │
    └──────────┘    │ (pgvector)  │   └──────────────┘
                    └─────────────┘     Repository Layer
```

---

## ✨ Features

### Ingestión de documentos

- [ ] Carga de PDFs, Markdown y archivos de texto
- [ ] Chunking inteligente (recursive, por tamaño, semántico)
- [ ] Metadatos por chunk (página, fuente, posición)
- [ ] Procesamiento batch de múltiples documentos

### Búsqueda y retrieval

- [ ] Generación de embeddings
- [ ] Almacenamiento en vector store (pgvector)
- [ ] Búsqueda semántica (Top-K)
- [ ] Filtrado por metadatos
- [ ] Score de relevancia

### Generación de respuestas

- [ ] RAG chain con LangChain
- [ ] Citación de fuentes en la respuesta
- [ ] Prompt template optimizado para Q&A
- [ ] Manejo de preguntas sin respuesta

### Eventos de Dominio (Event-Driven Architecture)

- [ ] `DocumentIngested` — disparado tras ingestión exitosa, listener indexa en vector store
- [ ] `QueryProcessed` — disparado tras generar respuesta, listener registra métricas
- [ ] `ChunkEmbedded` — disparado por chunk procesado, listener actualiza progreso

### API (API First — contrato OpenAPI previo)

- [ ] Upload de documentos
- [ ] Q&A sobre documentos cargados
- [ ] Listar documentos y sus chunks
- [ ] Búsqueda semántica directa

---

## 🛠️ Stack técnico

| Tecnología          | Propósito                             |
| ------------------- | ------------------------------------- |
| **LangChain**       | Orquestación RAG                      |
| **pgvector**        | Vector store en PostgreSQL            |
| **OpenAI**          | Embeddings + LLM                      |
| **FastAPI**         | API REST (Layered Architecture)       |
| **PostgreSQL**      | Metadatos + vector store              |
| **Alembic**         | Migraciones de esquema de BD          |
| **PyPDF2**          | Lectura de PDFs                       |
| **Docker**          | Containerización                      |
| **pytest**          | Testing (TDD)                         |
| **Testcontainers**  | Tests de integración con PostgreSQL   |

---

## 📡 Endpoints de la API

```
POST   /api/v1/documents/upload       # Subir documento
GET    /api/v1/documents               # Listar documentos
GET    /api/v1/documents/{id}/chunks   # Ver chunks de un documento
DELETE /api/v1/documents/{id}          # Eliminar documento

POST   /api/v1/query                   # Hacer pregunta sobre documentos
POST   /api/v1/search                  # Búsqueda semántica directa
```

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                    |
| -------------- | ------------------------------------------------------------ |
| 🌅 9:00-10:00  | Diseño UML (secuencia del pipeline RAG) + contrato OpenAPI   |
| 🌅 10:00-11:00 | TDD: tests de integración del flujo ingestión → query        |
| 🌅 11:00-12:00 | Alembic: migraciones para documentos, chunks y metadatos     |
| 🌞 12:00-13:00 | Layered: IngestionService + DocumentRepository               |
| 🌞 14:00-16:00 | RetrievalService + GenerationService + RAG chain             |
| 🌆 16:00-18:00 | Eventos: DocumentIngested, QueryProcessed + listeners        |

### Domingo

| Hora           | Actividad                                                     |
| -------------- | ------------------------------------------------------------- |
| 🌅 9:00-10:30  | Citación de fuentes + score de relevancia                     |
| 🌅 10:30-12:00 | Testcontainers: tests de integración con PostgreSQL + pgvector|
| 🌞 13:00-14:30 | Metadatos y filtrado                                          |
| 🌞 14:30-16:00 | Docker + ajuste de prompts                                    |
| 🌆 16:00-17:00 | README con diagramas UML, ejemplos y demo                     |

---

## ✅ Definición de "hecho"

- [ ] Contrato OpenAPI definido antes del código (API First)
- [ ] TDD: tests escritos primero (Rojo→Verde→Refactor)
- [ ] Layered Architecture con servicios separados (Ingestion, Retrieval, Generation)
- [ ] Al menos 2 eventos de dominio publicados y consumidos
- [ ] Migraciones versionadas con Alembic
- [ ] Pipeline RAG funcional de extremo a extremo
- [ ] Soporte para PDF y Markdown
- [ ] Tests de integración con Testcontainers (PostgreSQL + pgvector)
- [ ] Docker Compose funcional
- [ ] Diagramas UML (secuencia del pipeline RAG) en docs/uml/
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                             |
| ---------------------- | ----------------------------------------------------- |
| Layered Architecture   | Ingestion → Retrieval → Generation Services           |
| Event-Driven           | DocumentIngested, QueryProcessed eventos de dominio   |
| TDD / API First        | Tests primero + contrato OpenAPI previo               |
| Migraciones BD         | Alembic con versionado de esquema                     |
| RAG                    | Pipeline completo: ingestión → retrieval → generación |
| Testcontainers         | Tests de integración con PostgreSQL + pgvector        |
| UML                    | Diagrama de secuencia del pipeline                    |
| Demanda industrial     | RAG es una de las skills más buscadas                 |
