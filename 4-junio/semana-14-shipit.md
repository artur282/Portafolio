# 🚢 Semana 14 — ShipIt

> **Pipeline CI/CD completo con GitHub Actions, Docker, GitFlow y Testcontainers**

| Campo              | Detalle           |
| ------------------ | ----------------- |
| 📅 Fechas          | 6-7 de junio 2026 |
| 🏷️ Categoría       | DevOps & Cloud    |
| ⏱️ Tiempo estimado | 10-12 horas       |
| 📊 Dificultad      | ⭐⭐⭐ Intermedio |

---

## 🎯 Descripción

ShipIt es un proyecto enfocado en construir un pipeline CI/CD profesional desde cero. Toma una aplicación FastAPI existente (o nueva), la containeriza con Docker multi-stage, configura GitHub Actions para linting, testing, build y deploy automático. El resultado: cada push a `main` despliega automáticamente.

**Este proyecto es referencia para GitFlow y trazabilidad.** Implementa **GitFlow** estricto con ramas `feature/*`, `develop` y `main`, convenciones de commit asociadas a tickets, **Testcontainers** en la fase de tests del CI, y un **diagrama de flujo** del pipeline completo.

---

## ✨ Features

### Docker

- [ ] Dockerfile multi-stage (build + production)
- [ ] Imagen optimizada (slim, layers cacheadas)
- [ ] Docker Compose para desarrollo local
- [ ] Health checks en container
- [ ] .dockerignore optimizado

### GitHub Actions

- [ ] Pipeline de CI: lint (Ruff) → test (pytest + Testcontainers) → build (Docker)
- [ ] Pipeline de CD: deploy automático en push a main
- [ ] Caché de dependencias (pip, Docker layers)
- [ ] Matrix testing (Python 3.11, 3.12)
- [ ] Badges de estado en README

### GitFlow (Trazabilidad estricta)

- [ ] Ramas `feature/*` para cada funcionalidad
- [ ] Rama `develop` como integración continua
- [ ] Rama `main` solo para releases
- [ ] Convención de commits: `feat(TICKET-123): descripción`
- [ ] Branch protection rules en GitHub
- [ ] Diagrama de flujo GitFlow documentado

### Deploy

- [ ] Deploy a un servicio cloud (Railway, Render o Fly.io)
- [ ] Variables de entorno seguras (GitHub Secrets)
- [ ] Health check post-deploy
- [ ] Rollback manual documentado

### Calidad

- [ ] Pre-commit hooks (Ruff + mypy)
- [ ] Coverage report en CI con Testcontainers
- [ ] Semantic versioning con tags
- [ ] Changelog automático

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                      |
| ------------------ | ------------------------------ |
| **GitHub Actions** | CI/CD                          |
| **Docker**         | Containerización multi-stage   |
| **FastAPI**        | App de ejemplo                 |
| **pytest**         | Testing (TDD)                  |
| **Testcontainers** | Tests de integración en CI     |
| **Ruff**           | Linting                        |
| **mypy**           | Type checking                  |
| **Railway/Render** | Deploy cloud                   |

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                |
| -------------- | -------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño: diagrama de flujo GitFlow + pipeline CI/CD       |
| 🌅 10:00-10:30 | Setup GitFlow: ramas develop + main + primera feature/*  |
| 🌅 10:30-12:00 | Dockerfile multi-stage optimizado                        |
| 🌞 12:00-13:00 | Docker Compose para dev + DB (Testcontainers)            |
| 🌞 14:00-16:00 | GitHub Actions: pipeline CI con Testcontainers           |
| 🌆 16:00-18:00 | GitHub Actions: pipeline CD + deploy                     |

### Domingo

| Hora           | Actividad                                             |
| -------------- | ----------------------------------------------------- |
| 🌅 9:00-10:30  | Deploy a Railway/Render                               |
| 🌅 10:30-12:00 | Pre-commit hooks + quality gates                      |
| 🌞 13:00-14:30 | Coverage report + badges + convención de commits      |
| 🌞 14:30-16:00 | Tests del pipeline completo (push → CI → CD → deploy) |
| 🌆 16:00-17:00 | README con badges, diagrama GitFlow y docs de pipeline|

---

## ✅ Definición de "hecho"

- [ ] GitFlow estricto: feature/* → develop → main
- [ ] Convención de commits asociados a tickets
- [ ] Pipeline CI funcional con Testcontainers (lint → test → build)
- [ ] Pipeline CD con deploy automático
- [ ] Docker multi-stage optimizado
- [ ] App desplegada y accesible públicamente
- [ ] Diagrama de flujo GitFlow + pipeline CI/CD
- [ ] Pre-commit hooks configurados
- [ ] README con badges y documentación del pipeline

---

## 💼 Lo que demuestra al reclutador

| Habilidad            | Evidencia                           |
| -------------------- | ----------------------------------- |
| **GitFlow**          | Flujo profesional de ramas          |
| **Trazabilidad**     | Commits → tickets, branch rules    |
| **Testcontainers**   | Tests de integración en CI          |
| CI/CD                | GitHub Actions, pipelines completos |
| Docker               | Multi-stage, optimización           |
| DevOps               | Deploy automático, quality gates    |
