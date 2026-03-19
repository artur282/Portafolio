# 🤖 Semana 22 — AutomateAI

> **Plataforma de automatización potenciada por IA — Capstone con Patrones GoF y Event-Driven**

| Campo              | Detalle             |
| ------------------ | ------------------- |
| 📅 Fechas          | 1-2 de agosto 2026  |
| 🏷️ Categoría       | Capstone Project    |
| ⏱️ Tiempo estimado | 12-14 horas         |
| 📊 Dificultad      | ⭐⭐⭐⭐⭐ Avanzado |

---

## 🎯 Descripción

AutomateAI es el primer proyecto Capstone. Es una plataforma que integra automatización de flujos de trabajo potenciada por IA. Permite describir una tarea en lenguaje natural y la plataforma genera y ejecuta el flujo de automatización correspondiente.

Combina: Backend robusto (FastAPI con **Layered Architecture**), **Patrones GoF** (Strategy, Observer, Command), IA (LangChain/OpenAI), automatización con **Event-Driven Architecture** nativa (Celery workers como listeners de eventos), **Alembic** para migraciones, y **Testcontainers**. Documenta la arquitectura con **diagramas UML** (clases, secuencia, estado).

---

## 🏗️ Arquitectura (Layered + Event-Driven + GoF Patterns)

```
┌─────────────────────────────────────────────────────────────┐
│                       Frontend (React)                       │
│  (Chat Interface + Visual Workflow Builder + Dashboard)     │
└─────────────────────────────┬───────────────────────────────┘
                              │ REST / WebSocket
┌─────────────────────────────▼───────────────────────────────┐
│                  Controllers Layer                            │
│  WorkflowController  ChatController  ExecutionController     │
│  Request DTOs → Validación → Response DTOs                   │
├──────────────────────────────────────────────────────────────┤
│                   Services Layer                              │
│  WorkflowService (Command Pattern — GoF)                     │
│  AIService (Strategy Pattern — GoF) → LangChain              │
│  ExecutionService (Observer Pattern — GoF)                    │
├──────────────────────────────────────────────────────────────┤
│                    Events Layer                               │
│  WorkflowCreated → ExecutionStarted → NodeCompleted          │
│  Celery Workers como Listeners                                │
│  Circuit Breaker en integraciones externas                    │
├──────────────────────────────────────────────────────────────┤
│                 Repositories Layer                            │
│  WorkflowRepository  ExecutionRepository                     │
│  → PostgreSQL (Alembic)                                      │
└──────────────────────────────────────────────────────────────┘
```

---

## ✨ Features

### IA Core (Strategy Pattern)

- [ ] **Generador de Workflows**: texto → JSON Workflow (Strategy Pattern para diferentes LLMs)
- [ ] **Agente Inteligente**: decisión condicional basada en contenido
- [ ] **Data Mapping AI**: mapeo automático de campos entre servicios

### Motor de Ejecución (Command + Observer Patterns)

- [ ] Cada nodo del workflow es un Command (GoF)
- [ ] Observer: listeners reciben eventos de ejecución en tiempo real
- [ ] Logs de ejecución en tiempo real
- [ ] Retry policies inteligentes
- [ ] **Circuit Breaker** en integraciones externas

### Eventos de Dominio (Event-Driven Architecture)

- [ ] `WorkflowCreated` — listener valida y prepara ejecución
- [ ] `ExecutionStarted` — listener notifica al frontend via WebSocket
- [ ] `NodeCompleted` — listener ejecuta siguiente nodo
- [ ] `ExecutionFailed` — listener ejecuta compensación

### Integraciones (Nodos)

- [ ] **Trigger**: Webhook, Cron, Email recibido
- [ ] **Action**: HTTP Request, Email Send, Slack Message
- [ ] **Logic**: If/Else (AI based), Loop
- [ ] **Data**: Database Insert/Query

### UML y Documentación

- [ ] Diagrama de clases: Strategy, Command, Observer patterns
- [ ] Diagrama de secuencia: generación y ejecución de workflow
- [ ] Diagrama de estado: estados del workflow (Draft → Running → Completed → Failed)

---

## 🛠️ Stack técnico

| Tecnología             | Propósito                             |
| ---------------------- | ------------------------------------- |
| **FastAPI**            | Backend (Layered Architecture)        |
| **LangChain**          | IA y generación de flujos             |
| **Celery**             | Workers (listeners de eventos)        |
| **Redis**              | Broker de mensajería                  |
| **PostgreSQL**         | Persistencia                          |
| **Alembic**            | Migraciones de esquema BD             |
| **React + React Flow** | Frontend interactivo                  |
| **Docker Compose**     | Infraestructura completa              |
| **pytest**             | Testing (TDD)                         |
| **Testcontainers**     | Tests con Redis + PostgreSQL          |

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                                        |
| -------------- | -------------------------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (clases GoF + secuencia + estado) + Alembic migraciones              |
| 🌅 10:00-11:00 | TDD: tests de integración del flujo texto → workflow → ejecución                |
| 🌅 11:00-12:00 | Layered: Controllers + Services (Strategy, Command, Observer) + Repos           |
| 🌞 12:00-13:00 | AI Engine: Strategy Pattern para LLMs + prompt para generar workflow JSON       |
| 🌞 14:00-16:00 | Execution Engine: Command Pattern + eventos NodeCompleted + Circuit Breaker     |
| 🌆 16:00-18:00 | Integraciones: nodos Webhook, HTTP, Log                                          |

### Domingo

| Hora           | Actividad                                                                    |
| -------------- | ---------------------------------------------------------------------------- |
| 🌅 9:00-11:00  | Frontend: React Flow para visualización                                      |
| 🌅 11:00-12:30 | Frontend: Chat interface para interactuar con AI                             |
| 🌞 13:00-14:30 | Testcontainers: tests con Redis + PostgreSQL + Eventos end-to-end            |
| 🌞 14:30-16:00 | Eventos: WorkflowCreated, ExecutionStarted + listeners + Circuit Breaker     |
| 🌆 16:00-17:00 | README con diagramas UML (clases, secuencia, estado)                         |

---

## ✅ Definición de "hecho"

- [ ] TDD: tests escritos primero
- [ ] Layered Architecture: Controller → Service → Repository → DTO
- [ ] **Patrones GoF**: Strategy (IA), Command (nodos), Observer (ejecución)
- [ ] **Eventos de dominio**: WorkflowCreated, ExecutionStarted, NodeCompleted
- [ ] **Circuit Breaker** en integraciones externas
- [ ] Migraciones Alembic
- [ ] **Diagramas UML**: clases (GoF), secuencia, estado del workflow
- [ ] Tests con Testcontainers
- [ ] Interfaz visual funcional
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                                    |
| ---------------------- | ------------------------------------------------------------ |
| **Patrones GoF**       | Strategy, Command, Observer aplicados de forma práctica      |
| **UML avanzado**       | Diagramas de clases, secuencia y estado                      |
| **Event-Driven**       | Arquitectura nativa de eventos con Celery listeners          |
| **Circuit Breaker**    | Resiliencia en integraciones externas                        |
| Layered Architecture   | Controller → Service → Repository → DTO                     |
| System Design          | Arquitectura compleja de múltiples componentes               |
| GenAI Applied          | LLMs para control lógico                                     |
