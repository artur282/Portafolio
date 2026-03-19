# 🔗 Semana 17 — ProtoLink

> **Servicio gRPC con Protobuf — servidor Python + cliente Rust (poliglotismo real)**

| Campo              | Detalle                  |
| ------------------ | ------------------------ |
| 📅 Fechas          | 27-28 de junio 2026      |
| 🏷️ Categoría       | DevOps & Cloud           |
| ⏱️ Tiempo estimado | 10-12 horas              |
| 📊 Dificultad      | ⭐⭐⭐⭐ Intermedio-alto |

---

## 🎯 Descripción

ProtoLink es un servicio de comunicación inter-proceso que implementa **gRPC con Protocol Buffers**. Un servidor en **Python (grpcio + FastAPI)** expone servicios definidos por contrato `.proto`, y un cliente en **Rust (tonic)** los consume. Demuestra comunicación tipada, eficiente y polyglot entre dos lenguajes.

**gRPC es un diferenciador fuerte** porque pocas personas lo implementan desde cero en un portafolio. El proyecto demuestra **poliglotismo real** (Python + Rust comunicándose via Protobuf), **API First** con contrato `.proto` como source of truth, **Layered Architecture** en el servidor Python, y se complementa con una **REST API gateway** que expone los servicios gRPC como endpoints HTTP.

---

## 🏗️ Arquitectura (gRPC + REST Gateway + Layered)

```
┌───────────────────────────────────────────────────────┐
│                  Clientes Externos                     │
│  (Browser, Apps, Postman)                              │
└────────────────────┬──────────────────────────────────┘
                     │ HTTP REST
┌────────────────────▼──────────────────────────────────┐
│           REST Gateway (FastAPI)                       │
│  Controllers Layer: REST → gRPC bridge                │
│  Expone gRPC como endpoints HTTP                      │
└────────────────────┬──────────────────────────────────┘
                     │ gRPC (HTTP/2 + Protobuf)
┌────────────────────▼──────────────────────────────────┐
│           gRPC Server (Python grpcio)                  │
│                                                        │
│  ┌──────────────────────────────────────────┐         │
│  │  Service Layer                           │         │
│  │  UserService  ProductService             │         │
│  │  (lógica de negocio)                     │         │
│  ├──────────────────────────────────────────┤         │
│  │  Repository Layer                        │         │
│  │  → PostgreSQL (Alembic)                  │         │
│  └──────────────────────────────────────────┘         │
└───────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────┐
│           gRPC Client (Rust tonic)                     │
│  CLI que consume los servicios gRPC                    │
│  Demuestra interoperabilidad Python ↔ Rust             │
│  Rust genera stubs desde el mismo .proto               │
└───────────────────────────────────────────────────────┘
```

---

## ✨ Features

### Contrato Protobuf (.proto — API First)

- [ ] Definir servicios y mensajes en `.proto` (source of truth)
- [ ] `UserService`: CRUD de usuarios
- [ ] `ProductService`: CRUD de productos
- [ ] `HealthService`: health check + reflection
- [ ] Server streaming: listar items con streaming
- [ ] Generar stubs automáticos (Python + Rust desde el mismo `.proto`)

### Servidor gRPC (Python)

- [ ] Implementación de servicios con `grpcio`
- [ ] Layered Architecture: Service → Repository
- [ ] PostgreSQL con Alembic
- [ ] Interceptors: logging, autenticación, TraceID
- [ ] Server reflection (permite inspeccionar servicios)
- [ ] Health checking protocol

### REST Gateway (FastAPI)

- [ ] Bridge REST → gRPC (traduce HTTP a llamadas gRPC)
- [ ] Swagger UI generado (permite probar sin cliente gRPC)
- [ ] Controllers con DTOs Pydantic
- [ ] Error mapping: gRPC status codes → HTTP status codes

### Cliente gRPC (Rust — tonic)

- [ ] CLI interactivo que consume los servicios
- [ ] Stubs generados automáticamente desde `.proto` (tonic-build)
- [ ] Subcomandos: `create-user`, `list-products`, `health`
- [ ] Colored output con indicadores de latencia

### Eventos de Dominio

- [ ] `UserCreated` — listener en gRPC server
- [ ] `ProductUpdated` — listener registra cambios

---

## 🛠️ Stack técnico

| Tecnología       | Propósito                             |
| ---------------- | ------------------------------------- |
| **Protobuf**     | Definición de contratos               |
| **grpcio**       | Servidor gRPC (Python)                |
| **FastAPI**      | REST Gateway (Layered Architecture)   |
| **tonic**        | Cliente gRPC (Rust)                   |
| **tonic-build**  | Generación de stubs Rust              |
| **PostgreSQL**   | Base de datos                         |
| **Alembic**      | Migraciones de esquema BD             |
| **Docker Compose**| Server + Gateway + PG                |
| **pytest**       | Testing (TDD)                         |
| **Testcontainers**| Tests con PostgreSQL real            |

---

## 📁 Estructura del proyecto

```text
protolink/
├── proto/                                # Contratos Protobuf (API First)
│   ├── user.proto                        # UserService definition
│   ├── product.proto                     # ProductService definition
│   └── health.proto                      # HealthService definition
│
├── server/                               # gRPC Server (Python)
│   ├── docs/
│   │   └── uml/
│   │       ├── class-diagram.puml
│   │       └── sequence-grpc-call.puml
│   ├── services/
│   │   ├── user_service.py               # Lógica de negocio
│   │   └── product_service.py
│   ├── repositories/
│   │   ├── user_repository.py            # Acceso a datos
│   │   └── product_repository.py
│   ├── interceptors/
│   │   ├── auth_interceptor.py           # Autenticación
│   │   └── logging_interceptor.py        # Logging + TraceID
│   ├── generated/                        # Stubs generados por protoc
│   ├── migrations/                       # Alembic
│   ├── main.py                           # Entry point del servidor gRPC
│   └── tests/
│       ├── unit/
│       └── integration/
│
├── gateway/                              # REST Gateway (FastAPI)
│   ├── controllers/
│   │   ├── user_controller.py            # REST → gRPC bridge
│   │   └── product_controller.py
│   ├── schemas/
│   │   ├── requests.py                   # DTOs Pydantic
│   │   └── responses.py
│   └── main.py
│
├── client/                               # gRPC Client (Rust)
│   ├── src/
│   │   ├── main.rs                       # CLI entry point
│   │   ├── commands.rs                   # Subcomandos
│   │   └── generated/                    # Stubs tonic-build
│   ├── build.rs                          # Proto compilation
│   └── Cargo.toml
│
├── docker-compose.yml                    # Server + Gateway + PG
└── README.md
```

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                       |
| -------------- | --------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño: contratos `.proto` + UML secuencia                      |
| 🌅 10:00-10:30 | TDD: tests de integración del flujo gRPC                        |
| 🌅 10:30-12:00 | Alembic + gRPC server (Python): UserService + Repository        |
| 🌞 12:00-13:00 | gRPC server: ProductService + interceptors (auth, logging)      |
| 🌞 14:00-16:00 | REST Gateway (FastAPI): bridge REST → gRPC + Swagger            |
| 🌆 16:00-18:00 | Streaming gRPC: listar items con server streaming               |

### Domingo

| Hora           | Actividad                                                   |
| -------------- | ----------------------------------------------------------- |
| 🌅 9:00-11:00  | Cliente Rust (tonic): stubs generados + CLI comandos        |
| 🌅 11:00-12:00 | Testcontainers: tests con PostgreSQL real                   |
| 🌞 13:00-14:30 | Server reflection + health checking                         |
| 🌞 14:30-16:00 | Docker Compose: server + gateway + PG                       |
| 🌆 16:00-17:00 | README con diagramas UML y demo de interoperabilidad        |

---

## ✅ Definición de "hecho"

- [ ] **API First**: contratos `.proto` definidos antes del código
- [ ] TDD: tests escritos primero
- [ ] Layered Architecture en servidor: Service → Repository
- [ ] gRPC server funcional con al menos 2 servicios
- [ ] REST Gateway funcional con Swagger UI
- [ ] Cliente Rust funcional consumiendo los servicios
- [ ] Server streaming funcional
- [ ] Interceptors: auth + logging con TraceID
- [ ] Migraciones Alembic
- [ ] Tests con Testcontainers
- [ ] Diagramas UML (clases + secuencia gRPC call)
- [ ] Docker Compose funcional
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                        |
| ---------------------- | ------------------------------------------------ |
| **gRPC + Protobuf**    | Server streaming, interceptors, reflection       |
| **Poliglotismo**       | Python server + Rust client desde mismo .proto   |
| **API First**          | Contrato .proto como source of truth             |
| **Rust**               | Cliente tonic con stubs auto-generados           |
| Layered Architecture   | Service → Repository en gRPC server              |
| REST + gRPC            | Gateway que bridge ambos protocolos              |
| Migraciones BD         | Alembic versionado                               |
| Comunicación inter-svc | gRPC tipado vs REST no tipado                    |
