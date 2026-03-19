# 🔔 Semana 21 — NotifyHub

> **Sistema de notificaciones con RabbitMQ, patrón SAGA, idempotencia y Layered Architecture**

| Campo              | Detalle                  |
| ------------------ | ------------------------ |
| 📅 Fechas          | 25-26 de julio 2026      |
| 🏷️ Categoría       | Full-Stack Integration   |
| ⏱️ Tiempo estimado | 12-14 horas              |
| 📊 Dificultad      | ⭐⭐⭐⭐⭐ Avanzado      |

---

## 🎯 Descripción

NotifyHub es un sistema de notificaciones que envía mensajes por múltiples canales: email, webhook y en-app. Usa Celery para procesamiento asíncrono, Redis como broker de mensajes, y un frontend React para configurar y monitorear las notificaciones.

**Este es un proyecto clave para Event-Driven avanzado.** Complementa Celery con **RabbitMQ** como broker de eventos, implementa el **patrón SAGA** para notificaciones multi-canal (compensación si falla un canal), garantiza **idempotencia** en el procesamiento de notificaciones (deduplicación), incluye **Dead Letter Queue** con reintentos configurables, y utiliza **Testcontainers** para tests con Redis + RabbitMQ + PostgreSQL.

---

## 🏗️ Arquitectura (Layered + Event-Driven + SAGA)

```
┌──────────────────────────────────────────────────────────┐
│                   Controllers Layer                       │
│  NotificationController — Request DTOs → Response DTOs   │
├──────────────────────────────────────────────────────────┤
│                    Services Layer                         │
│  NotificationService — Orquesta envío multi-canal         │
│  SAGAOrchestrator — Patrón SAGA para compensación        │
├──────────────────────────────────────────────────────────┤
│                Event Broker Layer                         │
│  ┌─────────────┐    ┌─────────────┐                     │
│  │  RabbitMQ    │    │  Celery     │                     │
│  │  (Events)    │    │  (Workers)  │                     │
│  └──────┬──────┘    └──────┬──────┘                     │
│         │                  │                              │
│  ┌──────▼──────┐    ┌──────▼──────┐                     │
│  │ Consumer     │    │ Email Task  │                     │
│  │ (idempotente)│    │ Webhook Task│                     │
│  └─────────────┘    │ InApp Task  │                     │
│                      └─────────────┘                     │
│                                                          │
│  Dead Letter Queue → Reintentos → Fallback               │
│  SAGA: Email ✓ → Webhook ✗ → Compensar Email             │
├──────────────────────────────────────────────────────────┤
│                  Repositories Layer                       │
│  NotificationRepository → PostgreSQL (Alembic)           │
│  TemplateRepository → Jinja2 templates                   │
└──────────────────────────────────────────────────────────┘
```

---

## ✨ Features

### Canales de notificación

- [ ] **Email** — Enviar notificaciones por SMTP
- [ ] **Webhook** — POST a URL configurable
- [ ] **In-App** — Notificaciones consultables por API

### Event-Driven con RabbitMQ

- [ ] RabbitMQ como broker de eventos de notificación
- [ ] Exchanges y queues por canal (email, webhook, in-app)
- [ ] **Idempotencia**: cada notificación tiene ID único, deduplicación en consumer
- [ ] **Dead Letter Queue**: mensajes fallidos van a DLQ para revisión
- [ ] Retry configurables con backoff exponencial

### Patrón SAGA (Compensación multi-canal)

- [ ] Orquestación de envío multi-canal como una SAGA
- [ ] Si falla un canal después de enviar otro → compensación (marcar como parcial)
- [ ] Log de cada paso de la SAGA para auditoría
- [ ] Estado final: ALL_SENT, PARTIAL, FAILED

### Procesamiento asíncrono (Celery)

- [ ] Celery workers para envío por canal
- [ ] Redis como backend de resultados
- [ ] Rate limiting por canal

### API (Layered Architecture)

- [ ] Controller: NotificationController con Request/Response DTOs
- [ ] Service: NotificationService, SAGAOrchestrator, TemplateService
- [ ] Repository: NotificationRepository, TemplateRepository
- [ ] Templates de notificación (Jinja2)
- [ ] Historial con estado de entrega (pending, sent, failed, partial)

### Panel de gestión (React)

- [ ] Dashboard: notificaciones enviadas, fallidas, pendientes
- [ ] Configurar canales y destinatarios
- [ ] Ver historial con filtros
- [ ] Crear y editar templates

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                         |
| ------------------ | --------------------------------- |
| **FastAPI**        | API REST (Layered Architecture)   |
| **RabbitMQ**       | Broker de eventos de notificación |
| **Celery**         | Workers de procesamiento async    |
| **Redis**          | Backend de resultados + caché     |
| **PostgreSQL**     | Almacenamiento                    |
| **Alembic**        | Migraciones de esquema BD         |
| **Pydantic**       | DTOs (Request/Response schemas)   |
| **React**          | Panel de gestión                  |
| **Jinja2**         | Templates de notificación         |
| **SMTP**           | Canal email                       |
| **Docker Compose** | Todos los servicios               |
| **pytest**         | Testing (TDD)                     |
| **Testcontainers** | Tests con Redis + RabbitMQ + PG   |

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                         |
| -------------- | ----------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (secuencia SAGA + diagrama de eventos)                 |
| 🌅 10:00-10:30 | TDD: tests de integración del flujo multi-canal                   |
| 🌅 10:30-12:00 | Alembic + Layered: Controller → Service → Repository → DTOs      |
| 🌞 12:00-13:00 | RabbitMQ: exchanges, queues, producer + consumer idempotente      |
| 🌞 14:00-16:00 | Canal email (SMTP + Celery task) + canal webhook                  |
| 🌆 16:00-18:00 | SAGA Orchestrator: envío multi-canal con compensación             |

### Domingo

| Hora           | Actividad                                              |
| -------------- | ------------------------------------------------------ |
| 🌅 9:00-10:30  | Dead Letter Queue + retry + rate limiting              |
| 🌅 10:30-12:00 | Templates Jinja2 + canal in-app                        |
| 🌞 13:00-14:30 | Panel React: dashboard + historial                     |
| 🌞 14:30-16:00 | Testcontainers: tests con Redis + RabbitMQ + PostgreSQL|
| 🌆 16:00-17:00 | README con diagramas UML (SAGA + eventos)              |

---

## ✅ Definición de "hecho"

- [ ] TDD: tests escritos primero (Rojo→Verde→Refactor)
- [ ] Layered Architecture: Controller → Service → Repository → DTO
- [ ] **RabbitMQ** como broker de eventos
- [ ] **Idempotencia** en consumer (deduplicación por ID)
- [ ] **Patrón SAGA** para envío multi-canal con compensación
- [ ] **Dead Letter Queue** con reintentos
- [ ] Migraciones versionadas con Alembic
- [ ] Tests de integración con Testcontainers (Redis + RabbitMQ + PG)
- [ ] Diagrama de secuencia UML de la SAGA
- [ ] Al menos 2 canales funcionales
- [ ] Docker Compose (API + Celery + Redis + RabbitMQ + PG)
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                 |
| ---------------------- | ----------------------------------------- |
| **RabbitMQ**           | Broker de eventos, exchanges, queues      |
| **Patrón SAGA**        | Compensación multi-canal documentada      |
| **Idempotencia**       | Deduplicación de eventos en consumer      |
| **Dead Letter Queue**  | Manejo de fallos permanentes              |
| Layered Architecture   | Controller → Service → Repository → DTO   |
| Event-Driven avanzado  | Broker + idempotencia + SAGA              |
| Testcontainers         | Tests con Redis + RabbitMQ + PostgreSQL   |
| Async                  | Celery, Redis, tareas asíncronas          |
