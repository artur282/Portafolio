# 📱 Semana 20 — SnapTask

> **App móvil de gestión de tareas con Flutter, API First y backend Layered Architecture**

| Campo              | Detalle                |
| ------------------ | ---------------------- |
| 📅 Fechas          | 18-19 de julio 2026    |
| 🏷️ Categoría       | Full-Stack Integration |
| ⏱️ Tiempo estimado | 10-12 horas            |
| 📊 Dificultad      | ⭐⭐⭐ Intermedio      |

---

## 🎯 Descripción

SnapTask es una aplicación móvil de gestión de tareas construida con **Flutter**, diseñada para funcionar en Android e iOS desde una sola base de código. Incluye un backend con FastAPI para sincronización en la nube, notificaciones push y categorización de tareas.

El backend aplica **Layered Architecture**, **API First** con contrato OpenAPI compartido entre mobile y backend, **TDD** en el backend, **Alembic** para migraciones, y **eventos de dominio** (`TaskSynced`, `TaskConflictResolved`) para desacoplar la lógica de sincronización.

---

## ✨ Features

### Gestión de tareas

- [ ] Crear, editar y eliminar tareas con título, descripción y prioridad
- [ ] Categorías personalizables con colores e íconos
- [ ] Fechas límite con recordatorios locales
- [ ] Estados: pendiente, en progreso, completada
- [ ] Gestos de deslizamiento para acciones rápidas

### Interfaz móvil nativa

- [ ] Navegación fluida con GoRouter
- [ ] Pantalla principal con lista de tareas agrupadas por fecha
- [ ] Dark mode / Light mode
- [ ] Animaciones suaves y transiciones Hero
- [ ] Pull-to-refresh para sincronización

### Backend (FastAPI — Layered Architecture)

- [ ] Controller: TaskController, AuthController (endpoints REST)
- [ ] Service: TaskService, SyncService, AuthService (lógica de negocio)
- [ ] Repository: TaskRepository, UserRepository (PostgreSQL)
- [ ] DTOs: Request/Response con Pydantic
- [ ] Contrato OpenAPI compartido con mobile (API First)
- [ ] Alembic para migraciones versionadas

### Eventos de Dominio

- [ ] `TaskSynced` — disparado tras sincronización exitosa
- [ ] `TaskConflictResolved` — disparado al resolver conflicto de sync
- [ ] `TaskCreatedRemote` — disparado al crear tarea desde mobile

### Sincronización en la nube

- [ ] Autenticación con JWT (login/registro)
- [ ] Sincronización bidireccional de tareas
- [ ] Almacenamiento local con Hive (funciona offline)
- [ ] Resolución de conflictos (última escritura gana)

### Notificaciones

- [ ] Recordatorios programados con flutter_local_notifications
- [ ] Badge de la app con conteo de tareas pendientes

---

## 🛠️ Stack técnico

| Tecnología                    | Propósito                          |
| ----------------------------- | ---------------------------------- |
| **Flutter**                   | Framework móvil                    |
| **Dart**                      | Lenguaje frontend                  |
| **GoRouter**                  | Navegación                         |
| **Provider / Riverpod**       | Gestión del estado                 |
| **Hive**                      | Almacenamiento local               |
| **FastAPI**                   | Backend (Layered Architecture)     |
| **PostgreSQL**                | Base de datos                      |
| **Alembic**                   | Migraciones de esquema BD          |
| **Pydantic**                  | DTOs (Request/Response schemas)    |
| **JWT**                       | Autenticación                      |
| **pytest**                    | Testing backend (TDD)              |
| **Testcontainers**            | Tests integración con PostgreSQL   |

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                          |
| -------------- | ------------------------------------------------------------------ |
| 🌅 9:00-10:00  | Contrato OpenAPI (API First) compartido mobile ↔ backend           |
| 🌅 10:00-11:00 | TDD: tests de integración del backend (sync, auth)                |
| 🌅 11:00-12:00 | Alembic: migraciones + Layered Architecture backend               |
| 🌞 12:00-13:00 | Flutter: pantalla principal + TaskCard                             |
| 🌞 14:00-16:00 | Flutter: detalle + formulario + categorías                        |
| 🌆 16:00-18:00 | Eventos: TaskSynced, TaskConflictResolved + Testcontainers        |

### Domingo

| Hora           | Actividad                                           |
| -------------- | --------------------------------------------------- |
| 🌅 9:00-10:30  | Sincronización: providers de sync + Hive            |
| 🌅 10:30-12:00 | Dark mode + animaciones Hero                        |
| 🌞 13:00-14:30 | Notificaciones locales + recordatorios              |
| 🌞 14:30-16:00 | Tests (flutter_test + backend TDD completo)         |
| 🌆 16:00-17:00 | README con capturas de pantalla y diagramas UML     |

---

## ✅ Definición de "hecho"

- [ ] API First: contrato OpenAPI compartido mobile ↔ backend
- [ ] TDD: tests del backend escritos primero
- [ ] Layered Architecture: Controller → Service → Repository → DTO
- [ ] Eventos de dominio: TaskSynced, TaskConflictResolved
- [ ] Migraciones versionadas con Alembic
- [ ] Tests de integración con Testcontainers
- [ ] CRUD completo de tareas en la app móvil
- [ ] Modo offline con Hive
- [ ] Docker Compose para el backend
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                  |
| ---------------------- | ------------------------------------------ |
| Layered Architecture   | Controller → Service → Repository → DTO    |
| API First              | Contrato OpenAPI compartido mobile ↔ backend|
| TDD                    | Tests escritos primero en backend           |
| Event-Driven           | TaskSynced, TaskConflictResolved eventos    |
| Migraciones BD         | Alembic versionado                          |
| Full-stack mobile      | Flutter + FastAPI + PostgreSQL              |
| Modo Offline-first     | Funciona sin red y sincroniza cuando puede  |
