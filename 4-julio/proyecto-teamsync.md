# 🔗 TeamSync

## 1. Visión general
TeamSync es una plataforma full-stack moderna para la colaboración de equipos y gestión de proyectos. Abarca todo el ciclo de desarrollo sumando frontend web responsivo, backend robusto, notificaciones asíncronas y conectividad en tiempo real (WebSockets), finalizando con un cliente móvil en Flutter.

## 2. Tecnologías principales
* **Frontend Web**: React, TypeScript, TailwindCSS
* **Aplicación Móvil**: Flutter, Dart, Riverpod
* **Backend API**: FastAPI, PostgreSQL
* **Real-time / Async**: WebSockets, RabbitMQ, Celery

## 3. Arquitectura
* **ProjectHub (Core)**: Backend REST que sirve datos de proyectos, kanban boards y tareas. Frontend React que consume esta API usando SWR o React Query.
* **LiveDash (Real-time)**: Sistema de WebSockets en FastAPI que hace broad-casting a los clientes (Web y Móvil) cuando una tarea cambia de estado.
* **SnapTask (Mobile)**: App Flutter que comparte la misma base de datos.
* **NotifyHub (Workers)**: Implementación de RabbitMQ. Si ocurre una actualización de alto impacto, el API manda un mensaje a RabbitMQ; Celery lo procesa de fondo (ej: enviando un email simulado, logging externo o trigger de webhook). Usa Patrón SAGA para manejo de transacciones distribuidas o reintegros si falla.

## 4. Requerimientos / Features Clave
1. **React UI**: Interfaz kanban arrastrable.
2. **Flutter App**: Vistas de proyectos, creación de tareas rápidas y login móvil.
3. **Eventos WebSockets**: Si el usuario web mueve una tarjeta, el usuario móvil la ve moverse sin recargar la página.
4. **Tareas Background**: Colas de mensajería para "Notificar a todo el equipo", procesadas asincrónicamente por Celery Workers.
5. **Docker Compose Extendido**: Contenedores para Frontend, Backend, PostgreSQL, RabbitMQ y Redis (backend de Celery).

## 5. Diseño de Base de Datos
* `projects`: id, name, description, owner_id
* `tasks`: id, project_id, title, status (TODO, IN_PROGRESS, DONE)
* `comments`: id, task_id, user_id, text
