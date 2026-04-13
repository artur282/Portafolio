# 📚 Mes 02 — DocuRAG

> **Pipeline RAG de producción sobre documentos empresariales con chunking avanzado, reranking y evaluación con RAGAS**

| Campo              | Detalle                                                 |
| ------------------ | ------------------------------------------------------- |
| 📅 Mes             | Abril 2026                                              |
| 🏷️ Categoría       | RAG & Knowledge Retrieval                               |
| ⏱️ Tiempo estimado | ~40 horas (1 mes a tiempo parcial)                      |
| 📦 Repositorio     | [artur282/DocuRAG](https://github.com/artur282/DocuRAG) |
| 📊 Dificultad      | ⭐⭐⭐ Intermedio-alto                                  |
| 🎯 Competencia     | RAG + Embeddings + Vector DBs + Evaluación de Retrieval |

---

## 🎯 Caso de uso empresarial

**Problema:** Una empresa del sector bancario o inmobiliario tiene cientos de documentos internos (políticas, contratos, FAQ, reglamentos) y los empleados pierden tiempo buscando información manualmente. Necesitan un sistema de Q&A que responda con precisión citando la fuente.

**Solución:** Sistema RAG que:

- Ingiere documentos heterogéneos (PDF, Word, HTML, CSV)
- Aplica chunking estratégico según el tipo de contenido
- Busca con embeddings semánticos + reranking
- Genera respuestas fundamentadas en el documento (faithful, no alucinaciones)
- Evalúa la calidad del retrieval con métricas estándar (RAGAS)

**KPIs de impacto:**

- Faithfulness ≥ 0.85 (respuestas ancladas en documentos reales)
- Answer Relevancy ≥ 0.80
- Latencia P95 ≤ 3s con caché de embeddings
- Context Precision@3 ≥ 0.75

---

## 🏗️ Arquitectura

```
Ingesta                    Indexación                   Retrieval + Generación
──────                     ──────────                   ──────────────────────

┌──────────┐              ┌──────────────┐             ┌────────────────────┐
│ PDF/DOCX │──►           │   Chunking   │             │    Query API       │
│ HTML/CSV │  Loaders     │  Estratégico │             │  (FastAPI)         │
│ Markdown │◄──           │  (Recursive/ │             └────────┬───────────┘
└──────────┘              │   Semantic/  │                      │
                          │  Hierarchic) │                      ▼
                          └──────┬───────┘             ┌───────────────────┐
                                 │                     │  Query Expansion  │
                                 ▼                     │  (HyDE / Multi-Q) │
                          ┌──────────────┐             └────────┬──────────┘
                          │  Embeddings  │                      │
                          │ OpenAI /     │                      ▼
                          │ Sentence-T   │             ┌───────────────────┐
                          └──────┬───────┘             │ Vector Search (k) │
                                 │                     │ Chroma/pgvector   │
                                 ▼                     └────────┬──────────┘
                          ┌──────────────┐                      │
                          │  Vector Store│                      ▼
                          │  Chroma      │             ┌───────────────────┐
                          │  (dev)       │             │  Reranking        │
                          │  pgvector    │             │  (Cohere /        │
                          │  (prod)      │             │   Cross-encoder)  │
                          └──────────────┘             └────────┬──────────┘
                                                                │
                          ┌──────────────┐                      ▼
                          │   RAGAS      │◄────────   ┌───────────────────┐
                          │  Evaluación  │            │   LLM Generation  │
                          │  Framework   │            │   + Citations     │
                          └──────────────┘            └───────────────────┘
```

---

## ✨ Features

### Ingesta de documentos

- [ ] Loaders para PDF (PyPDFLoader), DOCX, HTML, CSV, Markdown
- [ ] Chunking strategies comparadas:
  - `RecursiveCharacterTextSplitter` — baseline
  - `SemanticChunker` (LangChain experimental) — semántico
  - Chunking jerárquico (párrafo → sección) — para documentos estructurados
- [ ] Preservación de metadatos en chunks (fuente, página, sección)
- [ ] Deduplicación de documentos ya indexados

### Retrieval avanzado

- [ ] Embeddings: OpenAI `text-embedding-3-small` vs `text-embedding-3-large` vs `sentence-transformers`
- [ ] Búsqueda híbrida: semántica + BM25 (keyword)
- [ ] Query expansion: HyDE (Hypothetical Document Embeddings) y Multi-Query
- [ ] Reranking: Cohere Rerank o Cross-encoder local
- [ ] Metadata filtering: por fecha, tipo de documento, categoría

### Generación con citations

- [ ] Respuesta citando chunk(s) fuente (nombre archivo + página)
- [ ] Modo "no sé" cuando el contexto no contiene la respuesta
- [ ] Detección de out-of-scope queries
- [ ] Structured output: respuesta + fuentes + confianza

### Evaluación con RAGAS

- [ ] Dataset de evaluación: 50+ pares pregunta-respuesta-contexto
- [ ] Métricas: faithfulness, answer relevancy, context recall, context precision
- [ ] Pipeline de evaluación automatizado con reporte JSON/HTML
- [ ] Comparativa de estrategias de chunking y retrieval

### Infraestructura

- [ ] API FastAPI con endpoints de ingesta, query y evaluación
- [ ] Chroma en desarrollo → pgvector en producción (Docker)
- [ ] Caché de embeddings para reducir costes
- [ ] Langfuse tracing de queries y retrievals

---

## 🛠️ Stack técnico

| Tecnología                | Propósito                                |
| ------------------------- | ---------------------------------------- |
| **LangChain**             | Orquestación RAG y loaders de documentos |
| **OpenAI Embeddings**     | `text-embedding-3-small/large`           |
| **Sentence-Transformers** | Embeddings locales para comparativa      |
| **Chroma**                | Vector store en desarrollo               |
| **pgvector + PostgreSQL** | Vector store en producción               |
| **RAGAS**                 | Framework de evaluación RAG              |
| **Cohere**                | Reranking de resultados                  |
| **FastAPI**               | API REST                                 |
| **Langfuse**              | Trazabilidad y observabilidad            |
| **Docker Compose**        | Infraestructura reproducible             |

---

## 📁 Estructura del proyecto

```bash
docurag/
├── app/
│   ├── main.py
│   ├── config.py               # Vector store selection, modelo
│   ├── routers/
│   │   ├── ingest.py           # Endpoints de ingesta
│   │   ├── query.py            # Endpoints de Q&A
│   │   └── evaluate.py         # Endpoints de evaluación
│   ├── services/
│   │   ├── document_loader.py  # Loaders multi-formato
│   │   ├── chunker.py          # Estrategias de chunking
│   │   ├── embedder.py         # Modelos de embedding
│   │   ├── vector_store.py     # Abstracción Chroma/pgvector
│   │   ├── retriever.py        # Retrieval + reranking
│   │   └── generator.py        # LLM + prompt de generación
│   └── models/
│       ├── document.py
│       └── query.py
├── evals/
│   ├── golden_set.json         # 50+ pares Q&A con ground truth
│   ├── run_ragas.py            # Script de evaluación RAGAS
│   └── results/                # Resultados por estrategia
├── docs/
│   ├── architecture.md
│   ├── chunking_comparison.md  # Análisis de estrategias
│   └── playbook.md
├── tests/
│   ├── test_chunker.py
│   ├── test_retriever.py
│   └── test_e2e.py
├── docker-compose.yml          # App + PostgreSQL + pgvector + Langfuse
├── Makefile
├── pyproject.toml
└── .env.example
```

---

## 📡 API Endpoints

```http
# Gestión de documentos
POST   /api/v1/documents/ingest      # Subir e indexar documento
GET    /api/v1/documents             # Listar documentos indexados
DELETE /api/v1/documents/{id}        # Eliminar documento del índice

# Queries
POST   /api/v1/query                 # Q&A con contexto recuperado
POST   /api/v1/query/stream          # Q&A con streaming de respuesta

# Body de /query:
# {
#   "question": "¿Cuál es la política de cancelación?",
#   "k": 4,
#   "strategy": "hybrid",    // semantic | hybrid | mmr
#   "rerank": true,
#   "filters": {"category": "policies"}
# }

# Evaluación
POST   /api/v1/evaluate/ragas        # Ejecutar evaluación RAGAS
GET    /api/v1/evaluate/report       # Último reporte de evaluación
```

---

## 📊 Estrategias de chunking a comparar

```python
# Experimento: 3 estrategias sobre mismo corpus, evaluadas con RAGAS

# 1. Baseline: RecursiveCharacterTextSplitter
chunker_baseline = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200,
)

# 2. Semántico: basado en cambios de tema
from langchain_experimental.text_splitter import SemanticChunker
chunker_semantic = SemanticChunker(
    embeddings=OpenAIEmbeddings(),
    breakpoint_threshold_type="percentile"
)

# 3. Jerárquico: preserva estructura (sección > párrafo)
# Chunking manual respetando headers H1, H2, H3
# con metadatos de sección en cada chunk
```

---

## 🧪 Resultado esperado del benchmark RAGAS

| Estrategia          | Faithfulness | Answer Rel. | Context Prec. | Coste (USD/1K q) |
| ------------------- | ------------ | ----------- | ------------- | ---------------- |
| Baseline (1000c)    | 0.78         | 0.75        | 0.70          | ~$0.003          |
| Semántico           | 0.85         | 0.82        | 0.77          | ~$0.004          |
| Jerárquico + Rerank | 0.89         | 0.87        | 0.83          | ~$0.006          |

_Nota: valores ilustrativos, reemplazar con resultados reales_

---

## 🗓️ Plan mensual

### Semana 1 — Ingesta y chunking

- Setup Docker: PostgreSQL + pgvector + Langfuse + FastAPI
- Implementar loaders multi-formato con LangChain
- Comparar las 3 estrategias de chunking en un corpus de prueba

### Semana 2 — Retrieval avanzado

- Vector store con Chroma (dev) y pgvector (prod)
- Embeddings: benchmarking OpenAI vs sentence-transformers
- Implementar búsqueda híbrida BM25 + semántica
- HyDE y Multi-Query para mejora de recall

### Semana 3 — Evaluación con RAGAS

- Crear golden set: 50 pares Q&A sobre el corpus
- Pipeline RAGAS automatizado
- Comparativa de estrategias con tabla de resultados
- Reranking (Cohere o cross-encoder) y su impacto en métricas

### Semana 4 — Polish y documentación

- Caché de embeddings (reduce coste ~70% en queries repetidas)
- Langfuse: dashboard de queries, latencia y costes
- Documento: análisis de qué chunking usar y cuándo
- API completa + tests + Docker + README con resultados

---

## ✅ Definición de "hecho"

- [ ] Pipeline RAG funcional con al menos 2 estrategias de chunking comparadas
- [ ] Evaluación RAGAS con ≥ 50 pares Q&A: faithfulness ≥ 0.82
- [ ] Búsqueda híbrida (semántica + BM25) implementada
- [ ] Reranking implementado y evaluado
- [ ] Langfuse dashboard con métricas de retrieval en tiempo real
- [ ] Docker Compose levanta todo con `make up`
- [ ] Documento de análisis: qué funciona y por qué
- [ ] README con arquitectura, trade-offs y resultados reales

---

## 💼 Lo que demuestra al reclutador

| Habilidad             | Evidencia                                                       |
| --------------------- | --------------------------------------------------------------- |
| RAG avanzado          | 3 estrategias de chunking comparadas con métricas reales        |
| Embeddings            | Benchmark OpenAI vs sentence-transformers con análisis de coste |
| Vector DBs            | Chroma (dev) + pgvector (prod) + búsqueda híbrida               |
| Evaluación rigurosa   | RAGAS: faithfulness, answer relevancy, context precision        |
| Reranking             | Cohere Rerank / cross-encoder para mejorar precisión            |
| Observabilidad        | Langfuse: trazas, coste, latencia por query                     |
| Pensamiento analítico | Trade-off analysis: calidad vs coste vs latencia documentado    |
