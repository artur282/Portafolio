# 🏢 Semana 10 — TenantAPI

> **API Multi-tenant SaaS con Row-Level Security, aislamiento de datos y FastAPI**

| Campo              | Detalle                       |
| ------------------ | ----------------------------- |
| 📅 Fechas          | 9-10 de mayo 2026             |
| 🏷️ Categoría       | Data Engineering & Automation |
| ⏱️ Tiempo estimado | 10-12 horas                   |
| 📊 Dificultad      | ⭐⭐⭐⭐ Intermedio-alto      |

---

## 🎯 Descripción

TenantAPI es una API multi-tenant que simula un SaaS real. Múltiples organizaciones (tenants) comparten la misma instancia de base de datos pero con **aislamiento completo de datos** usando **Row-Level Security (RLS)** de PostgreSQL. Cada tenant tiene sus propios usuarios, datos y configuración, sin poder acceder a datos de otros tenants.

Este es un patrón **extremadamente valorado** en empresas SaaS. El proyecto aplica **Layered Architecture**, **Alembic** con migraciones multi-tenant, **TDD**, **API First**, y **eventos de dominio** (`TenantCreated`, `UserInvited`). Implementa el **Repository Pattern** con inyección automática del tenant_id para aislamiento transparente.

---

## 🏗️ Arquitectura (Layered + Multi-tenant RLS)

```
┌────────────────────────────────────────────────┐
│  Request HTTP (JWT con tenant_id en claims)     │
│  Authorization: Bearer <token>                  │
└───────────────────┬────────────────────────────┘
                    │
┌───────────────────▼────────────────────────────┐
│            Middleware Layer                      │
│  TenantMiddleware: extrae tenant_id del JWT     │
│  → SET app.current_tenant = 'tenant_123'        │
├────────────────────────────────────────────────┤
│            Controllers Layer                    │
│  TenantController  UserController  DataController│
│  Request DTO → Validación → Response DTO        │
├────────────────────────────────────────────────┤
│             Services Layer                      │
│  TenantService  UserService  DataService        │
│  Lógica de negocio (sin preocuparse por tenant) │
├────────────────────────────────────────────────┤
│            Events Layer                         │
│  TenantCreated → listener crea schema/policies  │
│  UserInvited → listener envía email             │
├────────────────────────────────────────────────┤
│           Repositories Layer                    │
│  BaseRepository (inyecta tenant_id automático)  │
│  → PostgreSQL con RLS Policies                  │
│  → Alembic (migraciones multi-tenant)           │
└────────────────────────────────────────────────┘
```

---

## ✨ Features

### Multi-tenancy

- [ ] Registro de nuevos tenants (organizaciones)
- [ ] Aislamiento por Row-Level Security (RLS) en PostgreSQL
- [ ] Middleware que inyecta `tenant_id` desde JWT automáticamente
- [ ] **Zero data leakage**: un tenant nunca ve datos de otro
- [ ] Test de aislamiento: intentar acceder a datos de otro tenant → 403

### Gestión de Tenants

- [ ] CRUD de tenants (crear, leer, actualizar, desactivar)
- [ ] Configuración por tenant (timezone, idioma, plan)
- [ ] Límites por plan (Free: 5 usuarios, Pro: 50, Enterprise: ilimitado)
- [ ] Onboarding: seed de datos iniciales al crear tenant

### Usuarios y Roles

- [ ] Registro e invitación de usuarios dentro de un tenant
- [ ] Roles: Admin, Member, Viewer (por tenant)
- [ ] JWT con claims: `{ tenant_id, user_id, role }`
- [ ] Un usuario puede pertenecer a múltiples tenants

### Datos Multi-tenant (ejemplo: Proyectos)

- [ ] CRUD de proyectos (scoped al tenant automáticamente)
- [ ] Búsqueda y filtros (solo del tenant actual)
- [ ] Paginación

### Eventos de Dominio

- [ ] `TenantCreated` — listener crea RLS policies y seed data
- [ ] `UserInvited` — listener envía email de invitación
- [ ] `PlanUpgraded` — listener ajusta límites

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                             |
| ------------------ | ------------------------------------- |
| **FastAPI**        | API REST (Layered Architecture)       |
| **PostgreSQL**     | BD con Row-Level Security             |
| **Alembic**        | Migraciones multi-tenant              |
| **SQLAlchemy**     | ORM + Repository pattern + RLS aware  |
| **Pydantic**       | DTOs (Request/Response schemas)       |
| **PyJWT**          | Autenticación con tenant claims       |
| **Docker Compose** | Infraestructura                       |
| **pytest**         | Testing (TDD)                         |
| **Testcontainers** | Tests con PostgreSQL + RLS real       |

---

## 📡 Endpoints de la API

```
# Tenants (super-admin)
POST   /api/v1/tenants                        # Crear tenant
GET    /api/v1/tenants                         # Listar tenants
GET    /api/v1/tenants/{id}                    # Obtener tenant
PATCH  /api/v1/tenants/{id}                    # Actualizar tenant

# Auth (per-tenant)
POST   /api/v1/auth/login                     # Login (devuelve JWT con tenant_id)
POST   /api/v1/auth/register                  # Registro dentro del tenant

# Users (scoped al tenant del JWT)
GET    /api/v1/users                           # Listar usuarios del tenant
POST   /api/v1/users/invite                    # Invitar usuario al tenant
PATCH  /api/v1/users/{id}/role                 # Cambiar rol

# Projects (scoped al tenant automáticamente via RLS)
POST   /api/v1/projects                        # Crear proyecto
GET    /api/v1/projects                        # Listar (solo del tenant)
GET    /api/v1/projects/{id}                   # Obtener (solo si es del tenant)
PUT    /api/v1/projects/{id}                   # Actualizar
DELETE /api/v1/projects/{id}                   # Eliminar
```

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                         |
| -------------- | ----------------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (clases multi-tenant + secuencia RLS) + contrato OpenAPI |
| 🌅 10:00-10:30 | TDD: tests de aislamiento (tenant A no ve datos de tenant B)     |
| 🌅 10:30-12:00 | Alembic: migraciones + RLS policies en PostgreSQL                 |
| 🌞 12:00-13:00 | Middleware: TenantMiddleware + BaseRepository con tenant_id       |
| 🌞 14:00-16:00 | Layered: Controllers + Services + Repositories + DTOs             |
| 🌆 16:00-18:00 | Auth: JWT con tenant claims + roles                               |

### Domingo

| Hora           | Actividad                                                     |
| -------------- | ------------------------------------------------------------- |
| 🌅 9:00-10:30  | CRUD projects (RLS en acción — aislamiento automático)        |
| 🌅 10:30-12:00 | Eventos: TenantCreated, UserInvited + listeners               |
| 🌞 13:00-14:30 | Límites por plan (Free/Pro/Enterprise)                        |
| 🌞 14:30-16:00 | Testcontainers: tests con PostgreSQL + RLS real + aislamiento |
| 🌆 16:00-17:00 | README con diagramas UML y documentación de seguridad         |

---

## ✅ Definición de "hecho"

- [ ] Contrato OpenAPI definido antes del código (API First)
- [ ] TDD: tests de aislamiento escritos primero
- [ ] Layered Architecture: Controller → Service → Repository → DTO
- [ ] **Row-Level Security** funcionando (zero data leakage)
- [ ] Test de seguridad: tenant A no puede acceder a datos de tenant B
- [ ] Eventos: TenantCreated, UserInvited
- [ ] Migraciones Alembic con RLS policies
- [ ] JWT con tenant_id en claims
- [ ] Tests con Testcontainers (PostgreSQL con RLS)
- [ ] Diagramas UML (clases + secuencia del flujo multi-tenant)
- [ ] Docker Compose funcional
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                       |
| ---------------------- | ----------------------------------------------- |
| **Multi-tenancy**      | RLS, aislamiento, zero data leakage             |
| **PostgreSQL avanzado**| Row-Level Security, policies, claims-based       |
| **Arquitectura SaaS**  | Patrón real usado en producción                 |
| Layered Architecture   | Controller → Service → Repository → DTO         |
| TDD / API First        | Tests de seguridad primero                       |
| Event-Driven           | TenantCreated, UserInvited eventos               |
| Testcontainers         | Tests con PostgreSQL + RLS real                  |
| Seguridad              | JWT multi-tenant, roles, permisos                |
