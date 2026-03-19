# 🤖 Mes 04 — AgentFlow

> **Agente empresarial multi-paso con function calling, memoria persistente, orquestación LangGraph y métricas de éxito de tarea**

| Campo              | Detalle                                                       |
| ------------------ | ------------------------------------------------------------- |
| 📅 Mes             | Junio 2026                                                    |
| 🏷️ Categoría       | Multi-Step Agents & Workflows                                 |
| ⏱️ Tiempo estimado | ~40 horas (1 mes a tiempo parcial)                            |
| 📦 Repositorio     | [artur282/AgentFlow](https://github.com/artur282/AgentFlow)   |
| 📊 Dificultad      | ⭐⭐⭐⭐ Alto                                                 |
| 🎯 Competencia     | Agentes + Function Calling + LangGraph + Workflows multi-paso |

---

## 🎯 Caso de uso empresarial

**Problema:** Las empresas necesitan automatizar workflows complejos que requieren múltiples pasos, llamadas a servicios externos, decisiones condicionales y memoria de contexto. Un chatbot simple no es suficiente.

**Escenario real:** Agente de onboarding para sector inmobiliario o bancario que:

1. Recoge información del cliente (formulario por conversación)
2. Consulta bases de datos (historial crediticio, propiedades disponibles)
3. Genera documentación (resumen, propuesta comercial)
4. Envía notificaciones (email, CRM)
5. Escala a humano si detecta casos edge

**KPIs medidos:**

- Task Completion Rate: % de onboardings completados sin intervención humana
- Steps to Completion: promedio de pasos por tarea exitosa
- Human Escalation Rate: % de casos que requieren agente humano
- Tool Call Accuracy: % de llamadas a herramientas con parámetros correctos

---

## 🏗️ Arquitectura — LangGraph

```
Usuario / API Client
        │
        ▼
┌───────────────────────────────────────────────────────────┐
│                    AgentFlow API (FastAPI)                 │
│                                                           │
│  ┌─────────────────────────────────────────────────────┐  │
│  │              LangGraph Agent (Stateful)             │  │
│  │                                                     │  │
│  │   ┌──────────┐   ┌──────────┐   ┌──────────────┐  │  │
│  │   │  Planner │──►│ Executor │──►│  Reflector   │  │  │
│  │   │  (GPT-4o)│   │ (Tools)  │   │  (Verificar) │  │  │
│  │   └──────────┘   └────┬─────┘   └──────────────┘  │  │
│  │                       │                            │  │
│  │              Tool Registry                         │  │
│  │   ┌──────┐ ┌───────┐ ┌───────┐ ┌──────────────┐  │  │
│  │   │  DB  │ │Email  │ │  CRM  │ │ RAG (DocuRAG)│  │  │
│  │   │Query │ │ Send  │ │Update │ │   Search     │  │  │
│  │   └──────┘ └───────┘ └───────┘ └──────────────┘  │  │
│  └─────────────────────────────────────────────────────┘  │
│                           │                               │
│  ┌─────────────────────────────────────────────────────┐  │
│  │              Memoria Persistente                    │  │
│  │   PostgreSQL (episódica) + Chroma (semántica)       │  │
│  └─────────────────────────────────────────────────────┘  │
└───────────────────────────────────────────────────────────┘
                           │
                    Langfuse (trazas)
```

---

## ✨ Features

### Agent Architecture con LangGraph

- [ ] Grafo de estado con nodos: Planner → Tool Executor → Reflector → Human Checkpoint
- [ ] Patrón ReAct (Reasoning + Acting) con pasos explícitos
- [ ] Conditional edges: si tool falla → reintentar → si 3 fallos → escalar
- [ ] Human-in-the-loop: checkpoint antes de acciones irreversibles (envío de email, creación de contrato)
- [ ] Streaming de pasos del agente en tiempo real (Server-Sent Events)

### Tool Registry (Function Calling)

- [ ] **DB Query Tool** — Consulta PostgreSQL con parámetros tipados
- [ ] **Document Search Tool** — RAG sobre documentación (conectado a DocuRAG)
- [ ] **Email Tool** — Envío de emails con templates
- [ ] **CRM Tool** — Creación/actualización de contactos
- [ ] **Calculator Tool** — Cálculos financieros (hipotecas, amortizaciones)
- [ ] **Calendar Tool** — Agenda citas (mock API)
- [ ] Validación de inputs/outputs con Pydantic en cada tool

### Memoria Persistente

- [ ] **Memoria episódica** (PostgreSQL): historial completo de conversaciones
- [ ] **Memoria semántica** (Chroma): hechos importantes del usuario indexados con embeddings
- [ ] **Memoria procedimental** (código): flujos fijos como checklist de documentos
- [ ] Compresión de contexto: resumen de conversaciones largas para caber en contexto

### Patrones de Orquestación

- [ ] **ReAct**: Thought → Action → Observation loop
- [ ] **Plan-and-Execute**: Planeación completa antes de ejecutar
- [ ] **Multi-agent**: Sub-agente especializado para onboarding vs para consultas
- [ ] **Self-reflection**: El agente verifica su respuesta antes de enviarla

### Evaluación del Agente

- [ ] Task Completion Rate con golden scenarios
- [ ] Tool Call Accuracy: ¿parámetros correctos?
- [ ] Steps Efficiency: ¿mínimo de pasos para la tarea?
- [ ] Hallucination in Actions: ¿inventó herramientas o datos?
- [ ] Human Escalation Rate

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                                       |
| ------------------ | ----------------------------------------------- |
| **LangGraph**      | Orquestación del agente (grafo de estado)       |
| **LangChain**      | Tool integrations, memory, prompts              |
| **OpenAI API**     | GPT-4o con function calling                     |
| **FastAPI**        | API + WebSocket para streaming                  |
| **PostgreSQL**     | Memoria episódica + tool database mock          |
| **Chroma**         | Memoria semántica con embeddings                |
| **Langfuse**       | Tracing completo del agente (cada paso visible) |
| **Pydantic**       | Validación de schemas de tools y state          |
| **Docker Compose** | Infraestructura completa                        |

---

## 📁 Estructura del proyecto

```bash
agentflow/
├── app/
│   ├── main.py
│   ├── routers/
│   │   ├── agent.py              # Endpoints: invoke, stream, history
│   │   └── tools_admin.py        # Admin de herramientas registradas
│   ├── agent/
│   │   ├── graph.py              # Definición del grafo LangGraph
│   │   ├── nodes/
│   │   │   ├── planner.py        # Nodo de planificación
│   │   │   ├── executor.py       # Nodo de ejecución de tools
│   │   │   └── reflector.py      # Nodo de reflexión/verificación
│   │   ├── state.py              # AgentState Pydantic model
│   │   └── prompts/
│   │       ├── system.yaml       # Prompt sistémico del agente
│   │       └── planner.yaml      # Prompt de planificación
│   ├── tools/
│   │   ├── registry.py           # Tool registry y autodiscovery
│   │   ├── db_query.py           # Tool: consulta base de datos
│   │   ├── document_search.py    # Tool: RAG search
│   │   ├── email_sender.py       # Tool: envío de emails
│   │   ├── crm_tool.py           # Tool: actualización CRM
│   │   └── calculator.py         # Tool: cálculos financieros
│   ├── memory/
│   │   ├── episodic.py           # PostgreSQL memory
│   │   └── semantic.py           # Chroma semantic memory
│   └── monitoring/
│       └── langfuse_callbacks.py # Callbacks para Langfuse tracing
├── evals/
│   ├── scenarios/                # Golden scenarios de evaluación
│   │   ├── onboarding_complete.json
│   │   ├── document_qa.json
│   │   └── escalation_cases.json
│   └── run_agent_evals.py        # Evaluador de agente
├── docs/
│   ├── architecture.md
│   ├── tool_catalog.md           # Documentación de cada tool
│   └── playbook.md               # Guía de operación del agente
├── tests/
│   ├── test_tools.py
│   ├── test_agent_graph.py
│   └── test_memory.py
├── docker-compose.yml
├── Makefile
└── .env.example
```

---

## 📡 API Endpoints

```http
# Agente
POST   /api/v1/agent/invoke         # Invoke síncrono
POST   /api/v1/agent/stream         # Invoke con streaming SSE
GET    /api/v1/agent/sessions/{id}  # Historial de sesión

# Body de /invoke:
# {
#   "session_id": "user-123-session-456",
#   "message": "Necesito averiguar las opciones de hipoteca disponibles",
#   "config": {
#     "max_steps": 10,
#     "human_checkpoint": true,
#     "memory_enabled": true
#   }
# }

# Administración
GET    /api/v1/tools/               # Listar tools registradas
GET    /api/v1/tools/{name}/schema  # Schema JSON de una tool

# Evaluación
POST   /api/v1/eval/scenarios       # Correr golden scenarios
GET    /api/v1/eval/report          # Reporte: task_success, steps_avg, etc.
```

---

## 📊 Schema de LangGraph State

```python
from pydantic import BaseModel
from typing import Annotated
import operator

class AgentState(BaseModel):
    session_id: str
    messages: Annotated[list, operator.add]   # Historial completo
    context: dict                              # Datos recopilados del usuario
    plan: list[str]                            # Pasos planificados
    current_step: int
    tool_results: Annotated[list, operator.add]
    human_approval_needed: bool = False
    task_completed: bool = False
    escalation_reason: str | None = None
    metrics: dict = {                          # Métricas de ejecución
        "total_tokens": 0,
        "tool_calls": 0,
        "llm_calls": 0,
        "start_time": None
    }
```

---

## 🗓️ Plan mensual

### Semana 1 — Diseño del grafo y tool registry

- Definir el caso de uso (onboarding inmobiliario/bancario) con flujo completo
- Diseñar el grafo LangGraph: nodos, edges, state schema
- Implementar tool registry con autodiscovery y validación Pydantic
- Crear mocks de APIs externas (DB, CRM, Email)

### Semana 2 — Implementación del agente core

- Nodo Planner: genera plan de pasos con GPT-4o
- Nodo Executor: ejecuta tools con manejo de errores y reintentos
- Nodo Reflector: verifica resultado y decide continuar/escalar
- Human-in-the-loop: checkpoint con timeout

### Semana 3 — Memoria y evaluación

- Memoria episódica (PostgreSQL) y semántica (Chroma)
- Compresión de contexto para conversaciones largas
- Golden scenarios para evaluación: 10 flujos completos
- Métricas: task completion, steps efficiency, tool accuracy

### Semana 4 — Polish y documentación

- Streaming SSE con LangGraph (ver pasos en tiempo real)
- Langfuse: tracing completo con costes por sesión
- Playbook operacional del agente
- README con arquitectura, decisiones (LangGraph vs Autogen vs CrewAI) y resultados

---

## ✅ Definición de "hecho"

- [ ] Agente LangGraph funcionando con ≥ 5 tools conectadas
- [ ] Memoria persistente activa (episódica + semántica)
- [ ] Human-in-the-loop implementado en acciones irreversibles
- [ ] Task Completion Rate ≥ 80% en golden scenarios
- [ ] Streaming de pasos en tiempo real
- [ ] Langfuse: tracing completo visible por sesión
- [ ] Playbook de uso del agente para usuarios de negocio
- [ ] README con comparativa de patrones de orquestación (ReAct vs Plan-Execute)

---

## 💼 Lo que demuestra al reclutador

| Habilidad                | Evidencia                                                        |
| ------------------------ | ---------------------------------------------------------------- |
| Agentes multi-paso       | LangGraph con grafo de estado, conditional edges, error handling |
| Function/Tool Calling    | ≥5 tools tipadas con Pydantic, registry con autodiscovery        |
| Orquestación             | ReAct, Plan-Execute, Self-reflection implementados y comparados  |
| Memoria de agentes       | Episódica (PostgreSQL) + semántica (Chroma embeddings)           |
| Human-in-the-loop        | Checkpoint antes de acciones irreversibles                       |
| Evaluación de agentes    | Task completion rate, steps efficiency, escalation rate          |
| Arquitectura de sistemas | Decisión documentada: LangGraph vs alternativas con trade-offs   |
