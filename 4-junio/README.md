# ☁️ Junio — DevOps, Cloud & Polyglotism

> _"El código que no se puede desplegar es código incompleto."_

## 🎯 Objetivo del mes

Completar el ciclo de vida del software dominando CI/CD con GitFlow, microservicios ultra-ligeros en Rust (Axum), API Gateway patterns y comunicación polyglot con gRPC. En paralelo, construir un agente empresarial multi-paso con LangGraph.

---

## 📅 Proyectos del mes

### 🏗️ Backend Track (fines de semana)

| Semana | Fechas    | Proyecto                                            | Estado       |
| ------ | --------- | --------------------------------------------------- | ------------ |
| 14     | 6-7 Jun   | [ShipIt](./semana-14-shipit.md)                     | ⬜ Pendiente |
| 15     | 13-14 Jun | [AxumMicro](./semana-15-axummicro.md)               | ⬜ Pendiente |
| 16     | 20-21 Jun | [GateKeeper](./semana-16-gatekeeper.md)             | ⬜ Pendiente |
| 17     | 27-28 Jun | [ProtoLink](./semana-17-clouddeploy.md)             | ⬜ Pendiente |

### 🤖 AI Track (entre semana ~40h)

| Proyecto                                  | Tecnologías clave                       | Descripción                                             |
| ----------------------------------------- | --------------------------------------- | ------------------------------------------------------- |
| [AgentFlow](./mes-04-agentflow.md)        | LangGraph, OpenAI, Function Calling     | Agente empresarial multi-paso con tools y memoria       |

---

## 🧠 Habilidades que se desarrollan

**Backend:**
- Pipelines CI/CD con GitHub Actions y GitFlow
- Microservicios ultraligeros con Rust Axum (Distroless 20-30MB)
- API Gateway patterns (rate limiting, caché, auth, Circuit Breaker)
- gRPC + Protobuf: Python server + Rust client (polyglotismo real)
- Multi-stage Docker builds

**AI:**
- Agentes multi-paso con LangGraph (ReAct, Plan-and-Execute)
- Function calling y tool registry con validación Pydantic
- Memoria persistente: episódica (PostgreSQL) + semántica (Chroma)
- Human-in-the-loop para acciones irreversibles

---

## 🔗 Cómo se conectan los proyectos

```
Backend Track:
  Semana 14: ShipIt
      │  CI/CD, GitHub Actions, GitFlow → automatización del deploy
      ▼
  Semana 15: AxumMicro
      │  Rust Axum, Distroless → microservicios ultraligeros
      ▼
  Semana 16: GateKeeper
      │  API Gateway, rate limiting, Circuit Breaker → seguridad
      ▼
  Semana 17: ProtoLink
         gRPC + Protobuf, Python ↔ Rust → comunicación polyglot

AI Track:
  AgentFlow
      Agente empresarial con LangGraph, function calling,
      memoria persistente y orquestación de workflows
```

---

## 📊 Progreso

**Backend:**
- [ ] Semana 14 — ShipIt
- [ ] Semana 15 — AxumMicro
- [ ] Semana 16 — GateKeeper
- [ ] Semana 17 — ProtoLink

**AI:**
- [ ] AgentFlow
