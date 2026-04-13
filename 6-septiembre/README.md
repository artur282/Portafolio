# 🦀 Septiembre — Rust & High-Performance

> _"Cuando la velocidad y la seguridad de memoria no son opcionales."_

## 🎯 Objetivo del mes

Transicionar los conceptos aprendidos arquitectónicamente hacia el desarrollo maduro en `Rust`. Perseguir la máxima eficiencia posible escribiendo código seguro a nivel memoria sin Garbage Collector (GC), aprovechando el checker de compilación fuertemente estático.

---

## 📅 Proyectos del mes

### 🏗️ Proyecto Principal: [RustForge](./proyecto-rustforge.md)
Sistema backend construido completamente en Rust. Empuja frameworks asíncronos pesados resolviendo queries compile-time con SQLx. Se comunica con dashboards de administración por CLI nativo y WebSockets TUI (Ratatui) operando consumos bajísimos CPU/RAM en imágenes dockerizadas microscópicas (~25MB).

- **Tecnologías**: Rust, Axum, Tokio, Serde, SQLx, Ratatui, Clap, Distroless Docker
- **Estado**: ⬜ Pendiente

### 🤖 AI Track: [TailorAI](./mes-06-tailorai.md)
Capstone AI definitivo end-to-end. Integra los 5 proyectos previos de este sub-track (Gestión, Embeddings, Evaluación, Agentes, Seguridad) agrupados centralmente tras un orquestador AI global para ser empaquetado y vendido como un producto multi-nodo de Azure u otra infra genérica empresarial.

- **Tecnologías**: Azure / Multi-Node, LangGraph enterprise
- **Estado**: ⬜ Pendiente

---

## 🧠 Habilidades que se desarrollan

- Lenguaje Rust nivel Intermedio/Avanzado: Types, Lifetimes, Borrow Checker, Traits.
- Uso del entorno Tokio para Thread-pool concurrente masivo en tareas Async.
- Verificación fuerte en DB (Compile-time DB schema validation) a base de proc-macros.
- Re-Tooling y UI minimalista pero efectiva operada totalmente por consola (TUI).
- CI/CD MLOps desplegado bajo arquitectura completa Cloud Corporate en Azure/AWS.
