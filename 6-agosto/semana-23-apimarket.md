# 🌐 Semana 23 — APIMarket

> **Marketplace de APIs con Gateway, observabilidad, Circuit Breaker y Event-Driven**

| Campo              | Detalle             |
| ------------------ | ------------------- |
| 📅 Fechas          | 8-9 de agosto 2026  |
| 🏷️ Categoría       | Capstone Project    |
| ⏱️ Tiempo estimado | 10-12 horas         |
| 📊 Dificultad      | ⭐⭐⭐⭐⭐ Avanzado |

---

## 🎯 Descripción

APIMarket es una plataforma que permite a desarrolladores publicar APIs y a otros consumirlas a través de un gateway unificado. Maneja API Keys, rate limiting, facturación simulada y documentación unificada.

El proyecto aplica **Layered Architecture**, **Alembic** para migraciones en PostgreSQL, **Circuit Breaker** en el proxy a servicios, **observabilidad** con Prometheus, **eventos de dominio** (`APIRegistered`, `UsageQuotaExceeded`), y un **diagrama UML** de la plataforma completa.

---

## ✨ Features

### Para Proveedores de API

- [ ] Registro de nuevos endpoints (target URL, method)
- [ ] Definición de planes de uso (Free, Pro, Enterprise)
- [ ] Dashboard de métricas (requests recibidos, latencia, errores)

### Para Consumidores

- [ ] Explorador de APIs (Marketplace UI)
- [ ] Generación y rotación de API Keys
- [ ] Dashboard de consumo y costos estimados
- [ ] Sandbox para probar endpoints

### Core System (Layered Architecture)

- [ ] Controller: GatewayController, ServiceController, PlanController
- [ ] Service: ProxyService (+ Circuit Breaker), BillingService, DocService
- [ ] Repository: ServiceRepository, UsageRepository → PostgreSQL (Alembic)
- [ ] DTOs: Request/Response con Pydantic

### Resiliencia y Observabilidad

- [ ] **Circuit Breaker** en proxy a servicios registrados
- [ ] **Retry** con backoff para servicios caídos
- [ ] **Prometheus**: métricas por servicio (requests/s, latencia, errores)
- [ ] Health check de servicios con estado del Circuit Breaker

### Eventos de Dominio

- [ ] `APIRegistered` — listener genera documentación automática
- [ ] `UsageQuotaExceeded` — listener bloquea acceso al servicio
- [ ] `PaymentProcessed` — listener desbloquea cuota

---

## 🛠️ Stack técnico

| Tecnología            | Propósito                                     |
| --------------------- | --------------------------------------------- |
| **FastAPI**           | Backend (Layered Architecture)                |
| **Redis**             | Rate Limiting + caché                         |
| **PostgreSQL**        | Almacenamiento                                |
| **Alembic**           | Migraciones de esquema BD                     |
| **Prometheus**        | Métricas de observabilidad por servicio       |
| **Pydantic**          | DTOs (Request/Response schemas)               |
| **React + Tailwind**  | Frontend del Marketplace                      |
| **Docker**            | Despliegue de servicios                       |
| **pytest**            | Testing (TDD)                                 |
| **Testcontainers**    | Tests con Redis + PostgreSQL                  |

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                              |
| -------------- | ---------------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (diagrama de la plataforma) + Alembic migraciones           |
| 🌅 10:00-10:30 | TDD: tests de integración del flujo proxy + rate limit                |
| 🌅 10:30-12:00 | Layered: Controllers + Services + Repositories + DTOs                 |
| 🌞 12:00-13:00 | Gateway Core: proxy + Circuit Breaker + autenticación API Key         |
| 🌞 14:00-16:00 | Rate Limiting (Redis) + eventos APIRegistered, UsageQuotaExceeded     |
| 🌆 16:00-18:00 | Prometheus: métricas por servicio + Usage tracking                    |

### Domingo

| Hora           | Actividad                                                         |
| -------------- | ----------------------------------------------------------------- |
| 🌅 9:00-11:00  | Frontend: Catálogo de APIs + Dashboard                            |
| 🌅 11:00-12:30 | Sandbox: probar llamadas tipo Swagger UI                          |
| 🌞 13:00-14:30 | Testcontainers: tests con Redis + PostgreSQL                      |
| 🌞 14:30-16:00 | Deploy de 2 servicios dummy + facturación simulada               |
| 🌆 16:00-17:00 | README con diagrama UML y documentación                           |

---

## ✅ Definición de "hecho"

- [ ] TDD: tests escritos primero
- [ ] Layered Architecture: Controller → Service → Repository → DTO
- [ ] Circuit Breaker en proxy a servicios
- [ ] Prometheus: métricas por servicio
- [ ] Eventos: APIRegistered, UsageQuotaExceeded
- [ ] Migraciones Alembic
- [ ] Tests con Testcontainers
- [ ] Proxy funcional con auth + rate limit
- [ ] Diagrama UML de la plataforma
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad            | Evidencia                                |
| -------------------- | ---------------------------------------- |
| **Circuit Breaker**  | Resiliencia en proxy a servicios         |
| **Observabilidad**   | Prometheus métricas por servicio         |
| **Event-Driven**     | APIRegistered, UsageQuotaExceeded        |
| Layered Architecture | Controller → Service → Repository → DTO |
| Arquitectura SaaS    | Multi-tenancy, gestión de cuotas         |
| Migraciones BD       | Alembic versionado                       |
| Testcontainers       | Tests con Redis + PostgreSQL             |
