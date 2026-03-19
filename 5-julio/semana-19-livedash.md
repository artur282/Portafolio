# 📊 Semana 19 — LiveDash

> **Dashboard en tiempo real con WebSockets, eventos de dominio y Layered Architecture**

| Campo              | Detalle                  |
| ------------------ | ------------------------ |
| 📅 Fechas          | 11-12 de julio 2026      |
| 🏷️ Categoría       | Full-Stack Integration   |
| ⏱️ Tiempo estimado | 10-12 horas              |
| 📊 Dificultad      | ⭐⭐⭐⭐ Intermedio-alto |

---

## 🎯 Descripción

LiveDash es un dashboard que muestra datos en tiempo real usando WebSockets. Un backend FastAPI genera o recibe datos en vivo (simulando métricas de servidor, sensores IoT, o tráfico web), los transmite via WebSocket a un frontend React que los renderiza en gráficos interactivos que se actualizan automáticamente.

El backend aplica **Layered Architecture**, utiliza **Alembic** para migraciones en PostgreSQL, y emite **eventos de dominio** (`MetricReceived`, `ThresholdExceeded`) para desacoplar la recepción de datos de las acciones de alerta y broadcasting.

---

## ✨ Features

### Tiempo real

- [ ] WebSocket connection (FastAPI ↔ React)
- [ ] Datos actualizados cada 1-2 segundos
- [ ] Reconexión automática
- [ ] Indicador de estado de conexión

### Eventos de Dominio (Event-Driven Architecture)

- [ ] `MetricReceived` — disparado al recibir métrica, listener persiste y broadcast
- [ ] `ThresholdExceeded` — disparado al superar umbral, listener envía alerta
- [ ] `ConnectionEstablished` — disparado al conectar cliente, listener registra

### Dashboard

- [ ] Gráfico de líneas en tiempo real (últimos N datos)
- [ ] Gráfico de barras actualizable
- [ ] Cards con métricas (KPIs)
- [ ] Historial de datos (últimas 24h)
- [ ] Selección de métricas/fuentes

### Backend (Layered Architecture)

- [ ] Controller: WebSocket endpoint + REST endpoints
- [ ] Service: MetricService, AlertService, BroadcastService
- [ ] Repository: MetricRepository → PostgreSQL (Alembic)
- [ ] DTOs: Request/Response con Pydantic
- [ ] Generador de datos simulados (configurable)

### UI

- [ ] Layout tipo dashboard (grid)
- [ ] Animaciones suaves en actualizaciones
- [ ] Responsive
- [ ] Tema oscuro optimizado para dashboards

---

## 🛠️ Stack técnico

| Tecnología              | Propósito                         |
| ----------------------- | --------------------------------- |
| **FastAPI**             | Backend (Layered Architecture)    |
| **React**               | Frontend                          |
| **Recharts / Chart.js** | Gráficos                          |
| **TailwindCSS**         | Estilos                           |
| **WebSockets**          | Comunicación en tiempo real       |
| **PostgreSQL**          | Almacenamiento de métricas        |
| **Alembic**             | Migraciones de esquema BD         |
| **Docker**              | Containerización                  |
| **pytest**              | Testing (TDD)                     |

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                         |
| -------------- | ----------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (secuencia flujo WebSocket + eventos) + OpenAPI        |
| 🌅 10:00-10:30 | TDD: tests del flujo de métricas                                  |
| 🌅 10:30-12:00 | Alembic: migraciones + Layered Architecture backend               |
| 🌞 12:00-13:00 | Eventos: MetricReceived, ThresholdExceeded + listeners            |
| 🌞 14:00-16:00 | Frontend: layout dashboard + WebSocket client                     |
| 🌆 16:00-18:00 | Frontend: gráfico de líneas en tiempo real                        |

### Domingo

| Hora           | Actividad                                   |
| -------------- | ------------------------------------------- |
| 🌅 9:00-10:30  | KPI cards + gráfico de barras               |
| 🌅 10:30-12:00 | Datos históricos (API REST)                 |
| 🌞 13:00-14:30 | Reconexión automática + indicador de estado |
| 🌞 14:30-16:00 | Polish: animaciones, responsive, dark mode  |
| 🌆 16:00-17:00 | README con screenshots, diagramas UML       |

---

## ✅ Definición de "hecho"

- [ ] TDD: tests escritos primero (Rojo→Verde→Refactor)
- [ ] Layered Architecture: Controller → Service → Repository → DTO
- [ ] Eventos de dominio: MetricReceived, ThresholdExceeded
- [ ] Migraciones versionadas con Alembic
- [ ] Datos en tiempo real via WebSocket
- [ ] Al menos 2 tipos de gráficos
- [ ] Diagrama de secuencia UML del flujo WebSocket
- [ ] Docker Compose funcional
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad            | Evidencia                                 |
| -------------------- | ----------------------------------------- |
| Layered Architecture | Controller → Service → Repository         |
| Event-Driven         | MetricReceived, ThresholdExceeded eventos  |
| Migraciones BD       | Alembic versionado                         |
| WebSockets           | Comunicación bidireccional en tiempo real  |
| UML                  | Diagrama de secuencia del flujo            |
| Full-stack           | Integración frontend ↔ backend             |
