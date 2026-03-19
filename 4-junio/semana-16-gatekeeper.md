# 🛡️ Semana 16 — GateKeeper

> **API Gateway con Circuit Breaker, observabilidad, rate limiting y proxy reverso**

| Campo              | Detalle                  |
| ------------------ | ------------------------ |
| 📅 Fechas          | 20-21 de junio 2026      |
| 🏷️ Categoría       | DevOps & Cloud           |
| ⏱️ Tiempo estimado | 10-12 horas              |
| 📊 Dificultad      | ⭐⭐⭐⭐ Intermedio-alto |

---

## 🎯 Descripción

GateKeeper es un API Gateway construido desde cero que actúa como punto de entrada único para múltiples microservicios. Implementa rate limiting, caché de respuestas, autenticación JWT, logging centralizado y proxy reverso.

**Este es un proyecto clave para la propuesta laboral.** Implementa **Circuit Breaker, Retry y Fallback** para resiliencia ante fallos de backends, **tracing distribuido** con propagación de TraceID, **observabilidad** con métricas Prometheus y dashboard Grafana, y **Testcontainers** para Redis en los tests.

---

## 🏗️ Arquitectura (Gateway + Resiliencia + Observabilidad)

```
┌────────────────────────────────────────────────────────────┐
│                      API Gateway (GateKeeper)               │
│                                                             │
│  Request → Auth (JWT) → RateLimit → CircuitBreaker → Proxy │
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐ │
│  │  Auth Layer   │→│ RateLimit    │→│  Circuit Breaker   │ │
│  │  (JWT/APIKey) │  │ (Redis/Window)│ │  + Retry + Fallback│ │
│  └──────────────┘  └──────────────┘  └────────┬─────────┘ │
│                                                │            │
│  ┌──────────────────────────────────────────────┤           │
│  │                   Proxy Layer                │           │
│  │   TraceID propagado en headers               │           │
│  │   ┌──────────────┐ ┌──────────────┐         │           │
│  │   │  Backend A    │ │  Backend B    │         │           │
│  │   └──────────────┘ └──────────────┘         │           │
│  └──────────────────────────────────────────────┘           │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │                   Observabilidad                      │  │
│  │   Prometheus (metrics) → Grafana (dashboard)          │  │
│  │   OpenTelemetry (tracing) → TraceID correlation       │  │
│  └──────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────┘
```

---

## ✨ Features

### Rate Limiting

- [ ] Limitar requests por IP (ej: 100/minuto)
- [ ] Limitar por API key o usuario
- [ ] Sliding window algorithm
- [ ] Headers de rate limit (X-RateLimit-Limit, X-RateLimit-Remaining)
- [ ] Storage en Redis

### Caché

- [ ] Caché de respuestas GET configurable
- [ ] TTL configurable por ruta
- [ ] Invalidación de caché
- [ ] Cache-Control headers

### Autenticación

- [ ] Validación de JWT en gateway
- [ ] API Key authentication
- [ ] Forwarding de user info a backend
- [ ] Rutas públicas vs protegidas

### Resiliencia (Circuit Breaker + Retry + Fallback)

- [ ] **Circuit Breaker** — 3 estados: Closed, Open, Half-Open
- [ ] Apertura automática tras N fallos consecutivos
- [ ] **Retry** con backoff exponencial para errores transientes
- [ ] **Fallback** — Respuesta degradada cuando circuito abierto
- [ ] Métricas de estado del circuito por backend
- [ ] Health check de backends con estado del Circuit Breaker

### Tracing Distribuido

- [ ] TraceID generado si no viene en request
- [ ] Propagación de TraceID al backend via headers
- [ ] Logs correlacionados por TraceID

### Observabilidad (Prometheus + Grafana)

- [ ] Métricas expuestas: requests/s, latencia p50/p95/p99, errores por backend
- [ ] Estado del Circuit Breaker como métrica
- [ ] Dashboard Grafana preconfigurado
- [ ] Health endpoint con estado de todos los backends

### Proxy Reverso

- [ ] Routing a múltiples backends
- [ ] Configuración por YAML
- [ ] Request/Response logging con TraceID

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                             |
| ------------------ | ------------------------------------- |
| **FastAPI**        | Framework del gateway                 |
| **Redis**          | Rate limiting + caché                 |
| **PyJWT**          | Validación de tokens                  |
| **httpx**          | Proxy HTTP async                      |
| **Prometheus**     | Métricas de observabilidad            |
| **Grafana**        | Dashboard de métricas                 |
| **Docker Compose** | Gateway + Redis + backends + Prom + GF|
| **pytest**         | Testing (TDD)                         |
| **Testcontainers** | Tests de integración con Redis        |

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                         |
| -------------- | ----------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (secuencia: request → auth → ratelimit → CB → proxy)  |
| 🌅 10:00-10:30 | TDD: tests de integración del flujo completo                     |
| 🌅 10:30-12:00 | Proxy reverso básico + autenticación JWT                          |
| 🌞 12:00-13:00 | Rate limiting con Redis (sliding window)                          |
| 🌞 14:00-16:00 | Circuit Breaker + Retry + Fallback                                |
| 🌆 16:00-18:00 | Caché de respuestas + TraceID propagation                         |

### Domingo

| Hora           | Actividad                                              |
| -------------- | ------------------------------------------------------ |
| 🌅 9:00-10:30  | Prometheus: métricas de requests, latencia, CB state   |
| 🌅 10:30-12:00 | Grafana: dashboard preconfigurado                      |
| 🌞 13:00-14:30 | Configuración por YAML + health checks                 |
| 🌞 14:30-16:00 | Testcontainers: tests con Redis real                   |
| 🌆 16:00-17:00 | README con diagrama de secuencia y documentación       |

---

## ✅ Definición de "hecho"

- [ ] TDD: tests escritos primero (Rojo→Verde→Refactor)
- [ ] Circuit Breaker con 3 estados (Closed, Open, Half-Open)
- [ ] Retry con backoff exponencial + Fallback
- [ ] TraceID propagado entre gateway y backends
- [ ] Prometheus métricas + Grafana dashboard
- [ ] Rate limiting funcional con Redis
- [ ] Caché de respuestas configurable
- [ ] Autenticación JWT
- [ ] Tests de integración con Testcontainers (Redis)
- [ ] Diagrama de secuencia UML del flujo completo
- [ ] Docker Compose con todos los servicios
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                     |
| ---------------------- | --------------------------------------------- |
| **Circuit Breaker**    | 3 estados, apertura automática, fallback      |
| **Observabilidad**     | Prometheus + Grafana + TraceID correlation     |
| **Resiliencia**        | Retry + Fallback + degradación elegante       |
| **Tracing distribuido**| TraceID propagado gateway → backend           |
| Arquitectura           | API Gateway pattern                            |
| Seguridad              | Rate limiting, JWT, API keys                   |
| Performance            | Caché, Redis                                   |
| Testcontainers         | Tests con Redis real                           |
