# 🏗️ Marzo — Backend Foundations

> _"Todo gran sistema empieza con una API bien diseñada."_

## 🎯 Objetivo del mes

Establecer las bases sólidas de ingeniería backend construyendo APIs profesionales, sistemas de autenticación robustos, herramientas de scraping y utilidades CLI en Rust. En paralelo, iniciar el track de AI con un sistema de gestión de prompts versionado.

---

## 📅 Proyectos del mes

### 🏗️ Backend Track (fines de semana)

| Semana | Fechas    | Proyecto                                              | Estado       |
| ------ | --------- | ----------------------------------------------------- | ------------ |
| 01     | 7-8 Mar   | [TaskFlow API](./semana-01-taskflow-api.md)           | ✅ Completo  |
| 02     | 14-15 Mar | [AuthGuard](./semana-02-authguard.md)                 | ✅ Completo  |
| 03     | 21-22 Mar | [DataHarvest](./semana-03-dataharvest.md)             | ✅ Completo  |
| 04     | 28-29 Mar | [RustCLI (DashTUI)](./semana-04-rustcli.md)           | ✅ Completo  |

### 🤖 AI Track (entre semana ~40h)

| Proyecto                                | Tecnologías clave              | Descripción                                                   |
| --------------------------------------- | ------------------------------ | ------------------------------------------------------------- |
| [PromptLab](./mes-01-promptlab.md)      | OpenAI, Anthropic, Langfuse    | Sistema de versionado de prompts con evaluación comparativa   |

---

## 🧠 Habilidades que se desarrollan

**Backend:**
- Diseño de APIs RESTful con FastAPI (Layered Architecture, TDD)
- Autenticación y autorización (JWT, roles, permisos)
- Web scraping responsable y procesamiento de datos
- Desarrollo CLI/TUI con Rust 🦀
- Containerización con Docker
- Testing con pytest y Testcontainers

**AI:**
- Ingeniería de prompts sistemática y versionada
- Evaluación comparativa de LLMs (GPT-4o vs Claude vs Llama)
- Métricas de calidad NLP (ROUGE, BERTScore, LLM-as-judge)
- Observabilidad con Langfuse

---

## 🔗 Cómo se conectan los proyectos

```
Backend Track:
  Semana 01: TaskFlow API
      │  FastAPI, PostgreSQL, Docker, CRUD, TDD
      ▼
  Semana 02: AuthGuard
      │  JWT, roles, permisos → reutilizable en futuros proyectos
      ▼
  Semana 03: DataHarvest
      │  Selenium, Pandas, scraping → base para proyectos de datos
      ▼
  Semana 04: RustCLI (DashTUI)
         Rust, Clap, Ratatui → herramientas CLI de alto rendimiento

AI Track:
  PromptLab
      Sistema de catálogo y versionado de prompts con evaluación
      comparativa entre múltiples modelos LLM
```

---

## 📊 Progreso

**Backend:**
- [x] Semana 01 — TaskFlow API
- [x] Semana 02 — AuthGuard
- [x] Semana 03 — DataHarvest
- [x] Semana 04 — RustCLI (DashTUI)

**AI:**
- [ ] PromptLab
