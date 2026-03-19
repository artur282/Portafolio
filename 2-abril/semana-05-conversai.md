# 💬 Semana 05 — ConversAI

> **Chatbot conversacional con memoria y contexto usando LangChain y FastAPI**

| Campo              | Detalle           |
| ------------------ | ----------------- |
| 📅 Fechas          | 4-5 de abril 2026 |
| 🏷️ Categoría       | IA/ML & GenAI     |
| ⏱️ Tiempo estimado | 10-12 horas       |
| 📊 Dificultad      | ⭐⭐⭐ Intermedio |

---

## 🎯 Descripción

ConversAI es un chatbot conversacional que mantiene contexto a lo largo de una conversación. Usa LangChain para orquestar las llamadas a LLMs, implementa diferentes estrategias de memoria (buffer, summary, window), y expone todo a través de una API REST con FastAPI.

El proyecto sigue **Layered Architecture** estricta, utiliza **TDD** con flujo Rojo→Verde→Refactor, define el contrato **API First** antes de codificar, y emplea **eventos de dominio** para desacoplar la lógica de negocio del procesamiento asíncrono.

---

## 🏗️ Arquitectura (Layered Architecture)

```
┌──────────────────────────────────────────────┐
│               Controllers Layer               │
│  (ChatController, SessionController)          │
│  Request DTO → Validación → Response DTO      │
├──────────────────────────────────────────────┤
│               Services Layer                  │
│  (ChatService, MemoryService, SessionService) │
│  Lógica de negocio + Orquestación LangChain  │
├──────────────────────────────────────────────┤
│             Repositories Layer                │
│  (SessionRepository, MessageRepository)       │
│  Acceso a PostgreSQL via SQLAlchemy           │
├──────────────────────────────────────────────┤
│              Events Layer                     │
│  Publishers: SessionCreated, MessageReceived  │
│  Listeners: LogMessage, UpdateMemory          │
├──────────────────────────────────────────────┤
│           Infrastructure Layer                │
│  PostgreSQL (Alembic) + LLM Provider          │
└──────────────────────────────────────────────┘
```

---

## ✨ Features

### Conversación

- [ ] Chat con contexto persistente por sesión
- [ ] Múltiples sesiones de conversación
- [ ] Historial de mensajes consultable
- [ ] Streaming de respuestas (SSE)
- [ ] Soporte para system prompts personalizados

### Memoria

- [ ] Buffer Memory (últimos N mensajes)
- [ ] Summary Memory (resumen de conversación)
- [ ] Window Memory (ventana deslizante)
- [ ] Selección de estrategia por sesión (Strategy Pattern — GoF)

### API (API First — contrato OpenAPI definido previamente)

- [ ] Crear nueva sesión de chat
- [ ] Enviar mensaje y recibir respuesta
- [ ] Obtener historial de sesión
- [ ] Listar sesiones activas
- [ ] Eliminar sesión

### Eventos de Dominio (Event-Driven Architecture)

- [ ] `SessionCreated` — disparado al crear sesión, listener registra auditoría
- [ ] `MessageReceived` — disparado al recibir mensaje, listener actualiza memoria
- [ ] `ResponseGenerated` — disparado al responder, listener persiste historial

### Prompts

- [ ] Templates de prompt configurables
- [ ] System prompts por "personalidad"
- [ ] Prompt engineering documentado

---

## 🛠️ Stack técnico

| Tecnología        | Propósito                         |
| ----------------- | --------------------------------- |
| **LangChain**     | Orquestación de LLMs              |
| **FastAPI**       | API REST (Layered Architecture)   |
| **OpenAI API**    | Proveedor de LLM                  |
| **PostgreSQL**    | Persistencia de sesiones          |
| **Alembic**       | Migraciones de esquema de BD      |
| **Pydantic**      | DTOs (Request/Response schemas)   |
| **SSE-Starlette** | Streaming                         |
| **Docker**        | Containerización                  |
| **pytest**        | Testing (TDD)                     |
| **Testcontainers**| Tests de integración con PostgreSQL|

---

## 📡 Endpoints de la API

```
POST   /api/v1/sessions              # Crear sesión de chat
GET    /api/v1/sessions               # Listar sesiones
GET    /api/v1/sessions/{id}          # Obtener sesión con historial
DELETE /api/v1/sessions/{id}          # Eliminar sesión

POST   /api/v1/sessions/{id}/chat     # Enviar mensaje
GET    /api/v1/sessions/{id}/stream   # Chat con streaming (SSE)
GET    /api/v1/sessions/{id}/history  # Historial de mensajes

GET    /api/v1/prompts                # Listar prompts disponibles
```

---

## 📁 Estructura del proyecto (Layered Architecture)

```text
conversai/
├── docs/
│   ├── openapi.yaml                 # Contrato API First
│   └── uml/
│       ├── class-diagram.puml       # Diagrama de clases
│       └── sequence-chat.puml       # Diagrama de secuencia del flujo de chat
├── src/
│   ├── controllers/
│   │   ├── session_controller.py    # Endpoints de sesiones
│   │   └── chat_controller.py       # Endpoints de chat
│   ├── services/
│   │   ├── chat_service.py          # Lógica de chat + LangChain
│   │   ├── memory_service.py        # Estrategias de memoria
│   │   └── session_service.py       # Gestión de sesiones
│   ├── repositories/
│   │   ├── session_repository.py    # Acceso a datos de sesiones
│   │   └── message_repository.py    # Acceso a datos de mensajes
│   ├── schemas/
│   │   ├── requests.py              # Request DTOs
│   │   └── responses.py             # Response DTOs
│   ├── events/
│   │   ├── publishers.py            # Publicadores de eventos
│   │   └── listeners.py             # Consumidores de eventos
│   └── models/
│       ├── session.py               # Entidad Session
│       └── message.py               # Entidad Message
├── migrations/                      # Alembic migrations
│   ├── alembic.ini
│   └── versions/
├── tests/
│   ├── unit/
│   │   └── test_memory_service.py
│   ├── integration/
│   │   └── test_chat_flow.py        # Tests con Testcontainers
│   └── conftest.py
├── docker-compose.yml
├── Dockerfile
├── pyproject.toml
└── README.md
```

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                    |
| -------------- | ------------------------------------------------------------ |
| 🌅 9:00-10:00  | Diseño UML (clases + secuencia) + contrato OpenAPI           |
| 🌅 10:00-11:00 | TDD: escribir tests de integración del flujo de chat         |
| 🌅 11:00-12:00 | Alembic: setup + migraciones para Session y Message          |
| 🌞 12:00-13:00 | Layered: Controllers + Services + Repositories               |
| 🌞 14:00-16:00 | Integración LangChain (Chain → Memory → Response)            |
| 🌆 16:00-18:00 | Eventos de dominio: SessionCreated, MessageReceived          |

### Domingo

| Hora           | Actividad                                                |
| -------------- | -------------------------------------------------------- |
| 🌅 9:00-10:30  | Streaming con SSE + Strategy Pattern para memoria        |
| 🌅 10:30-12:00 | Testcontainers: tests de integración con PostgreSQL real |
| 🌞 13:00-14:30 | System prompts y personalidades                          |
| 🌞 14:30-16:00 | Tests adicionales (mocking de API de OpenAI)             |
| 🌆 16:00-17:00 | README, diagramas finales y documentación de prompts     |

---

## ✅ Definición de "hecho"

- [ ] Contrato OpenAPI definido antes del código (API First)
- [ ] TDD: tests escritos primero (Rojo→Verde→Refactor)
- [ ] Layered Architecture: Controller → Service → Repository → DTO
- [ ] Al menos 2 eventos de dominio publicados y consumidos
- [ ] Migraciones versionadas con Alembic
- [ ] Chat funcional con contexto persistente
- [ ] Al menos 2 estrategias de memoria (Strategy Pattern)
- [ ] Tests de integración con Testcontainers
- [ ] Docker Compose funcional
- [ ] Diagramas UML (clases + secuencia) en docs/uml/
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                       |
| ---------------------- | ----------------------------------------------- |
| Layered Architecture   | Controller → Service → Repository → DTO         |
| TDD                    | Tests escritos antes del código                  |
| API First              | Contrato OpenAPI previo a implementación         |
| Event-Driven           | Publishers/Listeners para eventos de dominio     |
| Migraciones BD         | Alembic con versionado de esquema                |
| IA/GenAI               | LangChain, prompts, estrategias de memoria       |
| Patrones GoF           | Strategy Pattern para selección de memoria       |
| Testing Trophy         | Testcontainers para integración con PostgreSQL   |
| Streaming              | SSE para respuestas en tiempo real               |
