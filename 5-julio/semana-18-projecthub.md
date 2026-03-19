# 📋 Semana 18 — ProjectHub

> **App full-stack de gestión de proyectos — Showcase de Layered Architecture + TDD + Event-Driven**

| Campo              | Detalle                  |
| ------------------ | ------------------------ |
| 📅 Fechas          | 4-5 de julio 2026        |
| 🏷️ Categoría       | Full-Stack Integration   |
| ⏱️ Tiempo estimado | 12-14 horas              |
| 📊 Dificultad      | ⭐⭐⭐⭐⭐ Avanzado      |

---

## 🎯 Descripción

ProjectHub es una aplicación full-stack de gestión de proyectos que combina un frontend moderno con React y TailwindCSS con un backend robusto en FastAPI. Incluye tablero Kanban, gestión de tareas, y autenticación de usuarios.

**Este es EL proyecto showcase de la propuesta laboral.** El backend implementa **Layered Architecture estricta** (Controller → Service → Repository → Request DTO → Response DTO), **TDD estricto** con Testing Trophy (más tests de integración que unitarios), **API First** con contrato OpenAPI, **eventos de dominio** (`ProjectCreated`, `TaskStatusChanged`, `TaskAssigned`), **Alembic** para migraciones, y **Testcontainers** para tests de integración. Es la demostración completa de las actividades descritas en la propuesta.

---

## 🏗️ Arquitectura (Layered Architecture + Event-Driven)

```
┌─────────────────────────────────────────────────┐
│                Frontend (React)                  │
│  Dashboard + Kanban + Auth                      │
└────────────────────┬────────────────────────────┘
                     │ REST API (contrato OpenAPI)
┌────────────────────▼────────────────────────────┐
│              Controllers Layer                    │
│  ProjectController  TaskController  AuthController│
│  RequestDTO → Validación → ResponseDTO            │
├──────────────────────────────────────────────────┤
│               Services Layer                      │
│  ProjectService  TaskService  AuthService         │
│  Lógica de negocio pura                           │
│  Publica eventos: ProjectCreated, TaskChanged     │
├──────────────────────────────────────────────────┤
│             Repositories Layer                    │
│  ProjectRepository  TaskRepository  UserRepository│
│  SQLAlchemy → PostgreSQL                          │
├──────────────────────────────────────────────────┤
│               Events Layer                        │
│  Publishers: ProjectCreated, TaskStatusChanged,   │
│              TaskAssigned                          │
│  Listeners: NotifyAssignee, UpdateStats,          │
│             AuditLog                               │
├──────────────────────────────────────────────────┤
│            Infrastructure Layer                   │
│  PostgreSQL (Alembic) + JWT Auth                  │
└──────────────────────────────────────────────────┘
```

---

## ✨ Features

### Frontend (React)

- [ ] Dashboard con resumen de proyectos
- [ ] Tablero Kanban drag & drop
- [ ] Lista de tareas con filtros
- [ ] Formularios de crear/editar proyecto y tarea
- [ ] Autenticación (login/registro)
- [ ] Diseño responsive con TailwindCSS
- [ ] Dark mode

### Backend (FastAPI — Layered Architecture estricta)

#### Controllers (Request/Response handling)
- [ ] `ProjectController` — endpoints CRUD de proyectos
- [ ] `TaskController` — endpoints CRUD de tareas + cambio de estado
- [ ] `AuthController` — login, registro, refresh token
- [ ] Request DTOs y Response DTOs separados con Pydantic

#### Services (Lógica de negocio)
- [ ] `ProjectService` — crear, actualizar, listar proyectos
- [ ] `TaskService` — crear, mover, asignar tareas + reglas de negocio
- [ ] `AuthService` — autenticación JWT + roles

#### Repositories (Acceso a datos)
- [ ] `ProjectRepository` — queries PostgreSQL
- [ ] `TaskRepository` — filtros, paginación, búsqueda
- [ ] `UserRepository` — gestión de usuarios

#### Eventos de Dominio (Event-Driven Architecture)
- [ ] `ProjectCreated` — listener actualiza estadísticas del dashboard
- [ ] `TaskStatusChanged` — listener registra historial de cambios
- [ ] `TaskAssigned` — listener envía notificación al asignado
- [ ] `TaskCompleted` — listener actualiza métricas del proyecto

### Testing (TDD + Testing Trophy)

- [ ] TDD estricto: Rojo → Verde → Refactor
- [ ] **Testing Trophy**: más tests de integración que unitarios
- [ ] Testcontainers: PostgreSQL real en tests de integración
- [ ] Fixtures compartidos en conftest.py

### Integración

- [ ] React Query para data fetching
- [ ] Estado global con Zustand (o Context)
- [ ] Manejo de errores unificado
- [ ] Loading states y skeletons

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                         |
| ------------------ | --------------------------------- |
| **React 18**       | Frontend                          |
| **TailwindCSS**    | Estilos                           |
| **React Query**    | Data fetching                     |
| **FastAPI**        | Backend (Layered Architecture)    |
| **PostgreSQL**     | Base de datos                     |
| **Alembic**        | Migraciones de esquema BD         |
| **SQLAlchemy**     | ORM + Repository pattern          |
| **Pydantic**       | DTOs (Request/Response schemas)   |
| **pytest**         | Testing (TDD)                     |
| **Testcontainers** | Tests integración con PostgreSQL  |
| **Docker Compose** | Frontend + Backend + DB           |

---

## 📁 Estructura del backend (Layered Architecture)

```text
backend/
├── docs/
│   ├── openapi.yaml                     # Contrato API First
│   └── uml/
│       ├── class-diagram.puml           # Clases del dominio
│       ├── sequence-create-project.puml # Secuencia: crear proyecto
│       └── sequence-move-task.puml      # Secuencia: mover task en Kanban
├── src/
│   ├── controllers/
│   │   ├── project_controller.py        # Endpoints de proyectos
│   │   ├── task_controller.py           # Endpoints de tareas
│   │   └── auth_controller.py           # Endpoints de autenticación
│   ├── services/
│   │   ├── project_service.py           # Lógica de proyectos
│   │   ├── task_service.py              # Lógica de tareas
│   │   └── auth_service.py              # Lógica de autenticación
│   ├── repositories/
│   │   ├── project_repository.py        # Acceso a datos proyectos
│   │   ├── task_repository.py           # Acceso a datos tareas
│   │   └── user_repository.py           # Acceso a datos usuarios
│   ├── schemas/
│   │   ├── project_request.py           # Request DTOs
│   │   ├── project_response.py          # Response DTOs
│   │   ├── task_request.py
│   │   ├── task_response.py
│   │   ├── auth_request.py
│   │   └── auth_response.py
│   ├── events/
│   │   ├── publishers.py                # ProjectCreated, TaskStatusChanged
│   │   └── listeners.py                 # UpdateStats, NotifyAssignee
│   └── models/
│       ├── project.py                   # Entidad Project
│       ├── task.py                      # Entidad Task
│       └── user.py                      # Entidad User
├── migrations/                          # Alembic migrations
├── tests/
│   ├── unit/
│   │   └── test_task_service.py
│   ├── integration/                     # Testing Trophy (más integración)
│   │   ├── test_project_flow.py         # Testcontainers
│   │   ├── test_task_kanban.py
│   │   └── test_auth_flow.py
│   └── conftest.py
```

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                         |
| -------------- | ----------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (clases + secuencia) + contrato OpenAPI                |
| 🌅 10:00-11:00 | TDD: escribir tests de integración (create project, move task)    |
| 🌅 11:00-12:00 | Alembic: migraciones para Project, Task, User                     |
| 🌞 12:00-13:00 | Layered: Controllers + Services + Repositories + DTOs             |
| 🌞 14:00-16:00 | Eventos: ProjectCreated, TaskStatusChanged + listeners            |
| 🌆 16:00-18:00 | Frontend: layout principal, routing, login                        |

### Domingo

| Hora           | Actividad                                                 |
| -------------- | --------------------------------------------------------- |
| 🌅 9:00-11:00  | Frontend: Tablero Kanban (drag & drop) + CRUD tareas      |
| 🌅 11:00-12:00 | Testcontainers: tests de integración con PostgreSQL       |
| 🌞 13:00-14:30 | Polish: dark mode, responsive                             |
| 🌞 14:30-16:00 | Tests adicionales (Testing Trophy balance)                |
| 🌆 16:00-17:00 | README con screenshots y diagramas UML                    |

---

## ✅ Definición de "hecho"

- [ ] **API First**: contrato OpenAPI definido antes del código
- [ ] **TDD estricto**: tests escritos primero (Rojo→Verde→Refactor)
- [ ] **Testing Trophy**: más tests de integración que unitarios
- [ ] **Layered Architecture**: Controller → Service → Repository → DTO
- [ ] **Eventos de dominio**: ProjectCreated, TaskStatusChanged, TaskAssigned
- [ ] **Migraciones**: Alembic versionadas
- [ ] **Testcontainers**: tests con PostgreSQL real
- [ ] **Diagramas UML**: clases + secuencia (crear proyecto, mover task)
- [ ] Kanban drag & drop funcional
- [ ] Responsive y dark mode
- [ ] Docker Compose (frontend + backend + DB)
- [ ] **GitFlow**: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                       |
| ---------------------- | ----------------------------------------------- |
| **Layered Architecture** | Controller → Service → Repository → DTO completo |
| **TDD + Testing Trophy** | Tests primero, más integración que unitarios    |
| **API First**           | Contrato OpenAPI previo a implementación         |
| **Event-Driven**        | ProjectCreated, TaskStatusChanged publishers     |
| **Migraciones BD**      | Alembic con versionado de esquema                |
| **Testcontainers**      | Tests de integración con PostgreSQL real         |
| **UML / Documentación** | Diagramas de clases y secuencia                  |
| Full-stack              | React + FastAPI + PostgreSQL                     |
| Producto                | App funcional de punta a punta                   |
