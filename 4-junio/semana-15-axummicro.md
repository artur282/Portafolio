# 🟢 Semana 15 — AxumMicro

> **Microservicio de alto rendimiento con Rust Axum, comunicación inter-servicio y Circuit Breaker**

| Campo              | Detalle             |
| ------------------ | ------------------- |
| 📅 Fechas          | 13-14 de junio 2026 |
| 🏷️ Categoría       | DevOps & Cloud      |
| ⏱️ Tiempo estimado | 10-12 horas         |
| 📊 Dificultad      | ⭐⭐⭐⭐ Intermedio-alto |

---

## 🎯 Descripción

AxumMicro es un microservicio ultraligero construido con **Rust y Axum**, enfocado en el alto rendimiento, eficiencia de memoria y modularidad. Utiliza SQLx para consultas directas y seguras a la base de datos sin la sobrecarga de un ORM, y Serde para serialización ultrarrápida.

**Este proyecto es clave para la propuesta** ya que demuestra **Rust Axum** (valorado en la oferta), arquitectura de **microservicios** con comunicación inter-servicio, **migraciones versionadas**, **Circuit Breaker** para resiliencia, y **tracing distribuido** con TraceIDs. Se despliega usando imágenes Docker **Distroless** (20-30MB).

---

## 🏗️ Arquitectura (Microservicios + Layered + Resiliencia)

```
┌────────────────────────┐     ┌────────────────────────┐
│   AxumMicro (Rust)      │────▶│  Servicio Auxiliar      │
│   Main Service          │◀────│  (Python FastAPI demo)  │
│                         │     │                         │
│  ┌───────────────────┐  │     └────────────────────────┘
│  │  Handlers Layer   │  │          HTTP/gRPC
│  │  (request/response│  │     Circuit Breaker + Retry
│  │   + DTOs Serde)   │  │
│  ├───────────────────┤  │
│  │  Services Layer   │  │     TraceID propagado
│  │  (business logic) │  │     entre servicios via
│  ├───────────────────┤  │     headers HTTP
│  │  Repository Layer │  │
│  │  (SQLx queries)   │  │
│  ├───────────────────┤  │
│  │  Migrations       │  │
│  │  (SQLx migrate)   │  │
│  └───────────────────┘  │
│                         │
│  Distroless (20-30MB)   │
└────────────────────────┘
```

---

## ✨ Features

### Core (URL Shortener / Task API)

- [ ] Acortar URL → generar código único alfanumérico
- [ ] Redirigir URL acortada a la original
- [ ] Estadísticas de clicks y telemetría ligera
- [ ] Configuración de expiración de enlaces

### Arquitectura y Layered Design

- [ ] **Handlers layer**: Endpoints Axum con DTOs Serde (Request/Response)
- [ ] **Services layer**: Lógica de negocio aislada
- [ ] **Repository layer**: SQLx queries verificadas en compilación
- [ ] Manejo de errores implementando `IntoResponse` para respuestas tipadas

### Comunicación Inter-Servicio (Microservicios)

- [ ] Llamadas HTTP a un servicio auxiliar (FastAPI demo)
- [ ] **Circuit Breaker**: degradación elegante si el servicio auxiliar falla
- [ ] **Retry** con backoff exponencial
- [ ] **Fallback**: respuesta degradada cuando el circuito está abierto
- [ ] TraceID propagado entre servicios via headers

### Migraciones y Datos

- [ ] **SQLx migrate**: migraciones versionadas de esquema
- [ ] Verificación de queries en tiempo de compilación
- [ ] PostgreSQL como base de datos principal

### Tracing Distribuido

- [ ] TraceID generado en cada request entrante
- [ ] TraceID propagado a llamadas inter-servicio
- [ ] Logs estructurados con TraceID para correlación

### Calidad y Despliegue

- [ ] Tests integrados con base de datos de pruebas (Testcontainers)
- [ ] **Docker + Distroless**: Imagen base mínima sin shell ni utilidades
- [ ] Binario final estáticamente enlazado (peso de 20-30MB)
- [ ] Clippy / Rustfmt para calidad de código

---

## 🛠️ Stack técnico

| Tecnología     | Propósito                                |
| -------------- | ---------------------------------------- |
| **Rust**       | Lenguaje base (valorado en la propuesta) |
| **Axum**       | Web Framework asíncrono y modular        |
| **SQLx**       | Database Driver + migraciones            |
| **Serde**      | DTOs (serialización/deserialización)     |
| **Tokio**      | Runtime asíncrono                        |
| **reqwest**    | Cliente HTTP para comunicación inter-svc |
| **Distroless** | Base de imagen Docker ligera y segura    |
| **Docker**     | Containerización multi-stage             |

---

## 📁 Estructura del proyecto

```text
axum_micro/
├── docs/
│   └── uml/
│       ├── class-diagram.puml           # Arquitectura Axum layers
│       └── sequence-inter-service.puml  # Comunicación inter-servicio
├── src/
│   ├── main.rs                          # Entry point y configuración
│   ├── handlers.rs                      # Handlers layer (Controllers)
│   ├── services.rs                      # Services layer (Business logic)
│   ├── repositories.rs                  # Repository layer (SQLx)
│   ├── models.rs                        # DTOs (Serde)
│   ├── error.rs                         # Manejo de errores centralizado
│   ├── circuit_breaker.rs               # Circuit Breaker pattern
│   └── tracing.rs                       # TraceID generation/propagation
├── migrations/                          # SQLx migrations versionadas
├── auxiliary-service/                   # Servicio demo FastAPI
│   ├── main.py
│   └── Dockerfile
├── tests/
│   └── integration_test.rs
├── Dockerfile                           # Multi-stage + Distroless
├── docker-compose.yml                   # AxumMicro + Auxiliary + PostgreSQL
├── Cargo.toml
└── README.md
```

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                         |
| -------------- | ----------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (clases + secuencia inter-servicio)                    |
| 🌅 10:00-12:00 | SQLx migrate: setup + migraciones                                 |
| 🌞 12:00-13:00 | Layered: Handlers + Services + Repositories + DTOs Serde          |
| 🌞 14:00-16:00 | Core logic: URL shortener endpoints                               |
| 🌆 16:00-18:00 | Servicio auxiliar (FastAPI) + comunicación HTTP + Circuit Breaker  |

### Domingo

| Hora           | Actividad                                              |
| -------------- | ------------------------------------------------------ |
| 🌅 9:00-10:30  | TraceID: generación y propagación entre servicios      |
| 🌅 10:30-12:00 | Tests de integración + Testcontainers (PostgreSQL)     |
| 🌞 13:00-14:30 | Dockerfile (Multi-stage + Distroless)                  |
| 🌞 14:30-16:00 | Ajustes finales (Clippy, Rustfmt, error handling)      |
| 🌆 16:00-17:00 | README con diagramas UML y documentación               |

---

## ✅ Definición de "hecho"

- [ ] Layered Architecture: Handlers → Services → Repositories → DTOs Serde
- [ ] Migraciones versionadas con SQLx migrate
- [ ] Comunicación inter-servicio con Circuit Breaker + Retry + Fallback
- [ ] TraceID propagado entre servicios
- [ ] Tests de integración con Testcontainers
- [ ] Diagramas UML (clases + secuencia inter-servicio)
- [ ] Imagen Docker Distroless ≤ 30MB
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                             |
| ---------------------- | ------------------------------------- |
| **Rust Axum**          | Valorado en la propuesta (poliglotismo)|
| **Microservicios**     | Comunicación inter-servicio real      |
| **Circuit Breaker**    | Resiliencia ante fallos de servicio   |
| **Tracing distribuido**| TraceID entre servicios               |
| Layered Architecture   | Handlers → Services → Repositories   |
| Migraciones BD         | SQLx migrate versionadas             |
| Docker                 | Distroless, imágenes minimalistas     |
| Testcontainers         | Tests con PostgreSQL real             |
