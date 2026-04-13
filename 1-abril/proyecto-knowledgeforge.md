# 🤖 KnowledgeForge

## 1. Visión general
KnowledgeForge es una plataforma enterprise de gestión de conocimiento potenciada por IA. Su propósito es ingerir documentos (PDFs, Markdown, HTML), indexarlos y ofrecer capacidades avanzadas de búsqueda y respuesta a preguntas (Q&A) utilizando Retrieval-Augmented Generation (RAG). Además, expone toda la base de conocimiento como herramientas (tools) a través de un servidor Model Context Protocol (MCP).

## 2. Tecnologías principales
* **Backend**: FastAPI, Python 3.11+
* **Bases de datos**: PostgreSQL con extensión `pgvector` (almacenamiento de embeddings), Elasticsearch (búsqueda full-text)
* **IA/GenAI**: LangChain, OpenAI API (o Anthropic), Model Context Protocol (MCP) SDK
* **Despliegue**: Docker, Docker Compose

## 3. Arquitectura
* **Layered Architecture**: Controller -> Service -> Repository -> DTO.
* **Pipeline de Ingesta**: Extracción de texto -> Chunking (Recursive, Semantic) -> Creación de embeddings -> Almacenamiento en pgvector y Elasticsearch.
* **Motor de Búsqueda (SearchMaster)**: Búsqueda híbrida (Semantic Search via pgvector + Lexical Search via Elasticsearch con BM25). Reranking para combinar resultados.
* **Módulo Conversacional (ConversAI)**: Chatbot con memoria episódica que mantiene el contexto de las consultas del usuario.
* **Servidor MCP (MCPForge)**: Implementación de un servidor MCP custom que expone endpoints como `search_knowledge`, `summarize_document` para que clientes externos o LLMs puedan consultar la base de conocimiento de forma nativa.

## 4. Requerimientos / Features Clave
1. **API de Ingesta**: Endpoints para subir y procesar documentos de forma asíncrona.
2. **Motor Híbrido**: API de búsqueda que combine Elasticsearch y pgvector.
3. **Conversación RAG**: Endpoint de chat que reciba la pregunta, recupere contexto relevante y genere la respuesta citando las fuentes.
4. **Tool Calling (MCP)**: Un servidor MCP que registre la base de datos de conocimiento como un conjunto de tools.
5. **Testing**: Tests de integración usando Testcontainers para PostgreSQL y Elasticsearch.

## 5. Diseño de Base de Datos
* `documents`: id, filename, content_hash, uploaded_at
* `document_chunks`: id, document_id, chunk_index, content, embedding (vector)
* `chat_sessions`: id, user_id, created_at
* `chat_messages`: id, session_id, role, content, context_used
