# 🤖 KnowledgeForge

> Plataforma enterprise de gestión de conocimiento potenciada por IA. Ingiere documentos, los indexa con búsqueda híbrida (Elasticsearch + pgvector) y genera respuestas fundamentadas con RAG — todo expuesto nativamente vía MCP.

[![Stack](https://img.shields.io/badge/Stack-FastAPI_|_LangChain_|_pgvector_|_Elasticsearch_|_RAGAS_|_Langfuse-blue?style=flat-square)](.)
[![Track](https://img.shields.io/badge/Track-Backend_+_GenAI_+_Observabilidad-purple?style=flat-square)](.)
[![Mes](https://img.shields.io/badge/Mes-Mayo_2026-green?style=flat-square)](.)

---

## 🎯 Visión general

KnowledgeForge consolida cuatro capacidades críticas en una sola plataforma:

| Módulo | Origen conceptual | Qué resuelve |
|--------|------------------|--------------|
| **Ingesta + Chunking** | DataHarvest pattern | Procesa PDFs, Markdown, HTML de forma asíncrona |
| **Búsqueda Híbrida** | SearchMaster | Elasticsearch BM25 + pgvector semántico con reranking |
| **RAG Conversacional** | ConversAI + DocuMind | Q&A con fuentes citadas y memoria de sesión |
| **Servidor MCP** | MCPForge | Expone `search_knowledge` / `summarize_doc` como tools nativas |

---

## 🛠️ Tecnologías principales

| Categoría | Tecnología | Versión objetivo |
|-----------|-----------|-----------------|
| Backend | FastAPI con `lifespan` async | 0.115+ |
| ORM / DB | SQLAlchemy async + Alembic | 2.0+ |
| Vectores | pgvector (extensión PostgreSQL) | 0.7+ |
| Búsqueda | Elasticsearch | 8.x |
| LLM / RAG | LangChain LCEL + `create_retrieval_chain` | 0.3+ |
| Embeddings | `text-embedding-3-small` (OpenAI) | — |
| MCP | `mcp` SDK Python | 1.x |
| Evaluación RAG | RAGAS (`EmbeddingModelEvaluator`) | Mide faithfulness, answer relevancy, context precision |
| Observabilidad LLM | Langfuse (self-hosted vía Docker) | Tracing de cada cadena LCEL, latencia por paso, costos |
| Testing | pytest + Testcontainers (PG + ES) | — |

---

## 🏗️ Arquitectura

```mermaid
flowchart TD
    Client --> |HTTP| GW[FastAPI Gateway]
    GW --> |ingest| Ing[Ingestion Service]
    GW --> |search| HS[Hybrid Search Service]
    GW --> |chat| RAG[RAG Chain Service]
    GW --> |mcp| MCP[MCP Server]
    GW --> |eval| EVAL[Eval Service]

    Ing --> Splitter[Text Splitter\nRecursive / Semantic]
    Splitter --> Embed[Embedding\ntext-embedding-3-small]
    Embed --> PG[(pgvector)]
    Embed --> ES[(Elasticsearch)]

    HS --> PG
    HS --> ES
    HS --> Rerank[Cohere Reranker]

    RAG --> Rerank
    RAG --> LLM[ChatOpenAI / gpt-4.1-mini]
    RAG --> Mem[(chat_messages)]
    RAG --> |trace| LF[Langfuse\nObservabilidad]

    MCP --> HS
    MCP --> RAG

    EVAL --> |dataset Q&A| RAGAS[RAGAS\nFaithfulness · Relevancy]
    RAGAS --> LF
```

### Patrones clave

- **Layered Architecture**: `router → service → repository → schema`
- **Lifespan context manager** (FastAPI 0.115+): inicializa conexiones a ES y PG al arranque
- **LCEL RAG chain** con callback de Langfuse (LangChain 0.3):
  ```python
  from langfuse.callback import CallbackHandler

  langfuse_handler = CallbackHandler()  # Lee LANGFUSE_* env vars

  chain = (
      {"context": retriever | format_docs, "question": RunnablePassthrough()}
      | ChatPromptTemplate.from_messages([("system", SYSTEM_PROMPT), ("human", "{question}")])
      | ChatOpenAI(model="gpt-4.1-mini")
      | StrOutputParser()
  )

  # El callback traza automáticamente cada paso: retriever, prompt, LLM, output
  response = await chain.ainvoke(
      {"question": user_question},
      config={"callbacks": [langfuse_handler]},
  )
  ```
- **BackgroundTasks** con DI (FastAPI) para procesamiento asíncrono de documentos:
  ```python
  @router.post("/documents")
  async def upload_document(
      file: UploadFile,
      background_tasks: BackgroundTasks,
      ingestion_svc: Annotated[IngestionService, Depends()],
  ):
      background_tasks.add_task(ingestion_svc.process, file)
      return {"status": "queued"}
  ```
- **Evaluación con RAGAS**: evalúa el pipeline RAG sobre un dataset curado de pares `(pregunta, respuesta_esperada, contexto)`:
  ```python
  from ragas import evaluate
  from ragas.metrics import faithfulness, answer_relevancy, context_precision
  from datasets import Dataset

  # Dataset de evaluación: Q&A golden set sobre los documentos ingestados
  eval_dataset = Dataset.from_list([
      {
          "question": "¿Cuál es la política de devoluciones?",
          "answer": response_from_rag,
          "contexts": retrieved_chunks,
          "ground_truth": expected_answer,
      },
      # ... más ejemplos
  ])

  result = evaluate(
      dataset=eval_dataset,
      metrics=[faithfulness, answer_relevancy, context_precision],
  )
  # result.to_pandas() → CSV publicable en docs/eval_report.csv
  print(result)  # faithfulness: 0.92 | answer_relevancy: 0.88 | context_precision: 0.85
  ```

---

## ✅ Definition of Done

- [ ] **API First**: contrato OpenAPI definido en `docs/openapi.yaml` antes de codificar
- [ ] **Ingesta asíncrona**: endpoint `POST /documents` con `BackgroundTasks`
- [ ] **Búsqueda híbrida**: combinar score pgvector + BM25 con Reciprocal Rank Fusion (RRF)
- [ ] **RAG con fuentes**: respuesta incluye `sources: [{doc_id, chunk_index, score}]`
- [ ] **Servidor MCP**: tools `search_knowledge(query)` y `summarize_document(doc_id)`
- [ ] **Langfuse activo**: cada request al endpoint `/chat` genera una traza visible en `localhost:3000` con latencia por paso y tokens consumidos
- [ ] **RAGAS evaluación**: `POST /eval/run` ejecuta el dataset curado y retorna `faithfulness`, `answer_relevancy` y `context_precision` — reporte publicado en `docs/eval_report.csv`
- [ ] **Tests de integración**: Testcontainers para PostgreSQL + Elasticsearch
- [ ] **Memoria de sesión**: `chat_messages` persiste el historial por `session_id`
- [ ] **Docker Compose**: PG (con pgvector), Elasticsearch, Langfuse (self-hosted), app levantados con un comando

---

## 📐 Endpoints principales

```
POST   /documents                → Ingesta + chunking (async)
GET    /documents/{id}           → Estado del documento
DELETE /documents/{id}           → Eliminar doc y sus chunks

POST   /search                   → Búsqueda híbrida (body: {query, k, filters})
GET    /search/suggest           → Autocompletado (Elasticsearch suggest)

POST   /chat                     → RAG Q&A (body: {session_id, question})
GET    /chat/{session_id}/history → Historial de la sesión

GET    /mcp/tools                → Lista las tools MCP disponibles

POST   /eval/run                 → Ejecuta RAGAS sobre el dataset curado
GET    /eval/reports             → Historial de reportes de evaluación
```

---

## 🗄️ Esquema de base de datos

```sql
-- Documentos ingestados
CREATE TABLE documents (
    id           UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    filename     TEXT NOT NULL,
    content_hash TEXT NOT NULL UNIQUE,  -- deduplicación
    status       TEXT DEFAULT 'pending', -- pending | processing | ready | failed
    uploaded_at  TIMESTAMPTZ DEFAULT now()
);

-- Chunks con embeddings
CREATE TABLE document_chunks (
    id           UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    document_id  UUID REFERENCES documents(id) ON DELETE CASCADE,
    chunk_index  INT NOT NULL,
    content      TEXT NOT NULL,
    embedding    vector(1536),           -- text-embedding-3-small
    metadata     JSONB DEFAULT '{}'
);
CREATE INDEX ON document_chunks USING ivfflat (embedding vector_cosine_ops);

-- Sesiones y mensajes de chat
CREATE TABLE chat_sessions (
    id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    created_at TIMESTAMPTZ DEFAULT now()
);

CREATE TABLE chat_messages (
    id           UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    session_id   UUID REFERENCES chat_sessions(id) ON DELETE CASCADE,
    role         TEXT NOT NULL CHECK (role IN ('user', 'assistant')),
    content      TEXT NOT NULL,
    context_used JSONB DEFAULT '[]',    -- chunks usados como contexto
    created_at   TIMESTAMPTZ DEFAULT now()
);
```

---

<details>
<summary>📚 Referencias de documentación usada</summary>

- [FastAPI BackgroundTasks + DI](https://fastapi.tiangolo.com/tutorial/background-tasks)
- [FastAPI Dependencies with yield](https://fastapi.tiangolo.com/tutorial/dependencies/dependencies-with-yield)
- [LangChain LCEL create_retrieval_chain](https://docs.langchain.com/oss/python)
- [pgvector ivfflat index](https://github.com/pgvector/pgvector)
- [MCP Python SDK](https://modelcontextprotocol.io/docs/sdk/python)
- [Langfuse — LangChain callback integration](https://langfuse.com/docs/integrations/langchain)
- [RAGAS — RAG evaluation metrics](https://docs.ragas.io/en/latest/concepts/metrics/index.html)

</details>
