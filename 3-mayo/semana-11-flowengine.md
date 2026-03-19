# 💳 Semana 11 — PayFlow

> **Sistema de billing y subscripciones con Stripe webhooks, idempotencia y FastAPI**

| Campo              | Detalle                       |
| ------------------ | ----------------------------- |
| 📅 Fechas          | 16-17 de mayo 2026            |
| 🏷️ Categoría       | Data Engineering & Automation |
| ⏱️ Tiempo estimado | 10-12 horas                   |
| 📊 Dificultad      | ⭐⭐⭐⭐ Intermedio-alto      |

---

## 🎯 Descripción

PayFlow es un sistema de billing para SaaS que integra **Stripe** para gestión de suscripciones, pagos recurrentes y facturación. Los webhooks de Stripe actúan como eventos externos que disparan toda la lógica de negocio: activar suscripciones, generar facturas, manejar fallos de pago y cancelaciones.

**Billing es el corazón de todo SaaS** — y es un proyecto que rara vez aparece en portafolios. El proyecto aplica **Layered Architecture**, usa webhooks como **Event-Driven Architecture** (eventos externos reales), garantiza **idempotencia** crítica (un webhook procesado dos veces no debe cobrar dos veces), utiliza **Alembic** para migraciones, y **Testcontainers** para tests.

---

## 🏗️ Arquitectura (Layered + Event-Driven via Webhooks)

```
┌──────────────────────────────────────────────────────┐
│                    Stripe (externo)                    │
│  checkout.session.completed                           │
│  invoice.paid / invoice.payment_failed                │
│  customer.subscription.updated / deleted              │
└───────────────────┬──────────────────────────────────┘
                    │ Webhooks (POST /api/v1/webhooks/stripe)
┌───────────────────▼──────────────────────────────────┐
│              Controllers Layer                        │
│  WebhookController (validación de firma Stripe)       │
│  SubscriptionController  InvoiceController            │
│  Request DTO → Validación → Response DTO              │
├──────────────────────────────────────────────────────┤
│               Services Layer                          │
│  WebhookService (dispatch por event type)             │
│  SubscriptionService (activar, cancelar, upgrade)     │
│  InvoiceService (generar, marcar pagada/fallida)      │
│  IdempotencyService (deduplicación por event ID)      │
├──────────────────────────────────────────────────────┤
│                Events Layer                           │
│  SubscriptionActivated → listener envía email         │
│  PaymentFailed → listener notifica + retry            │
│  SubscriptionCancelled → listener desactiva acceso    │
├──────────────────────────────────────────────────────┤
│             Repositories Layer                        │
│  SubscriptionRepo  InvoiceRepo  CustomerRepo         │
│  → PostgreSQL (Alembic)                               │
│  IdempotencyRepo (registro de webhooks procesados)    │
└──────────────────────────────────────────────────────┘
```

---

## ✨ Features

### Stripe Integration

- [ ] Crear sesión de checkout (Stripe Checkout)
- [ ] Webhook receiver con validación de firma
- [ ] Manejo de eventos: `checkout.session.completed`, `invoice.paid`, `invoice.payment_failed`, `customer.subscription.deleted`
- [ ] Modo de prueba (Stripe test keys)

### Suscripciones

- [ ] Planes: Free, Pro, Enterprise
- [ ] Activación automática tras pago exitoso
- [ ] Upgrade/downgrade de plan
- [ ] Cancelación (inmediata o al final del período)
- [ ] Trial period configurable

### Facturación

- [ ] Generación de factura tras cada pago
- [ ] Historial de facturas por cliente
- [ ] Estado: pending, paid, failed, refunded
- [ ] Exportación de facturas (JSON/PDF)

### Idempotencia (Crítica)

- [ ] **Cada webhook tiene un ID único** (Stripe event ID)
- [ ] Tabla de idempotencia: si el webhook ya fue procesado → skip
- [ ] Protección contra webhooks duplicados (Stripe los reenvía hasta 3 veces)
- [ ] Tests específicos de idempotencia

### Eventos de Dominio (Event-Driven)

- [ ] `SubscriptionActivated` — listener envía email de bienvenida
- [ ] `PaymentFailed` — listener envía notificación + marca para retry
- [ ] `SubscriptionCancelled` — listener desactiva acceso al servicio
- [ ] `InvoiceGenerated` — listener registra en historial

### API para clientes

- [ ] Ver plan actual y estado de suscripción
- [ ] Ver historial de pagos/facturas
- [ ] Portal de billing (redirect a Stripe Customer Portal)

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                              |
| ------------------ | -------------------------------------- |
| **FastAPI**        | API REST (Layered Architecture)        |
| **Stripe API**     | Pagos, suscripciones, webhooks         |
| **PostgreSQL**     | Almacenamiento                         |
| **Alembic**        | Migraciones de esquema BD              |
| **SQLAlchemy**     | ORM + Repository pattern               |
| **Pydantic**       | DTOs (Request/Response schemas)        |
| **Docker Compose** | Infraestructura                        |
| **pytest**         | Testing (TDD)                          |
| **Testcontainers** | Tests con PostgreSQL real              |
| **stripe-mock**    | Mock del API de Stripe para tests      |

---

## 📡 Endpoints de la API

```
# Webhooks (Stripe → PayFlow)
POST   /api/v1/webhooks/stripe                 # Receiver (validación de firma)

# Checkout
POST   /api/v1/checkout/session                # Crear sesión de pago Stripe
GET    /api/v1/checkout/success                 # Callback de pago exitoso
GET    /api/v1/checkout/cancel                  # Callback de pago cancelado

# Subscriptions
GET    /api/v1/subscriptions/me                 # Mi suscripción actual
POST   /api/v1/subscriptions/upgrade            # Upgrade de plan
POST   /api/v1/subscriptions/cancel             # Cancelar suscripción
GET    /api/v1/subscriptions/portal             # Redirect a Stripe Portal

# Invoices
GET    /api/v1/invoices                         # Historial de facturas
GET    /api/v1/invoices/{id}                    # Detalle de factura
GET    /api/v1/invoices/{id}/pdf                # Descargar factura PDF

# Plans
GET    /api/v1/plans                            # Listar planes disponibles
```

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                          |
| -------------- | ------------------------------------------------------------------ |
| 🌅 9:00-10:00  | Diseño UML (secuencia webhook flow) + contrato OpenAPI             |
| 🌅 10:00-10:30 | TDD: tests de idempotencia + tests de activación de suscripción    |
| 🌅 10:30-12:00 | Alembic: migraciones (customers, subscriptions, invoices, idempotency) |
| 🌞 12:00-13:00 | Layered: Controllers + Services + Repositories + DTOs              |
| 🌞 14:00-16:00 | Stripe: checkout session + webhook receiver + validación de firma  |
| 🌆 16:00-18:00 | Webhook handlers: invoice.paid → activar suscripción + idempotencia|

### Domingo

| Hora           | Actividad                                                    |
| -------------- | ------------------------------------------------------------ |
| 🌅 9:00-10:30  | Eventos: SubscriptionActivated, PaymentFailed + listeners    |
| 🌅 10:30-12:00 | Upgrade/downgrade + cancelación                              |
| 🌞 13:00-14:30 | Historial de facturas + export PDF                           |
| 🌞 14:30-16:00 | Testcontainers + stripe-mock: tests end-to-end               |
| 🌆 16:00-17:00 | README con diagrama de secuencia del webhook flow            |

---

## ✅ Definición de "hecho"

- [ ] Contrato OpenAPI definido antes del código (API First)
- [ ] TDD: tests de idempotencia escritos primero
- [ ] Layered Architecture: Controller → Service → Repository → DTO
- [ ] **Idempotencia**: webhook procesado 2x no cobra 2x
- [ ] **Stripe webhooks**: al menos 3 event types manejados
- [ ] Eventos de dominio: SubscriptionActivated, PaymentFailed
- [ ] Migraciones versionadas con Alembic
- [ ] Tests con Testcontainers + stripe-mock
- [ ] Diagrama de secuencia UML del webhook flow
- [ ] Docker Compose funcional
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                     |
| ---------------------- | --------------------------------------------- |
| **Billing / SaaS**     | Stripe integration, suscripciones, facturación|
| **Idempotencia**       | Deduplicación crítica de webhooks              |
| **Webhooks**           | Eventos externos reales (no simulados)         |
| **Event-Driven**       | Webhooks → eventos internos → listeners        |
| Layered Architecture   | Controller → Service → Repository → DTO        |
| TDD                    | Tests de idempotencia escritos primero          |
| Migraciones BD         | Alembic con versionado de esquema               |
| Relevancia industrial  | Todo SaaS necesita billing                      |
