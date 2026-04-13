# 🦀 RustForge

## 1. Visión general
RustForge es el pináculo en optimización de backend. El objetivo de este proyecto es re-escribir patrones conocidos en Python hacia un sistema 100% en Rust. Se busca máxima eficiencia de memoria, paralelismo verdadero sin Global Interpreter Lock (GIL) y latencias sub-milisegundo. 

## 2. Tecnologías principales
* **Backend Core**: Rust
* **Web Framework**: Axum (sobre Tokio y hyper)
* **Conexión a BD**: SQLx (PostgreSQL async con compile-time check de queries)
* **Middlewares**: Tower (Auth, Rate Limiting, CORS, Metrics)
* **Herramientas de Terminal**: Clap (CLI), Ratatui (TUI)
* **Infra**: Docker Distroless

## 3. Arquitectura
* **Axum API Server**: Servidor HTTP principal muy concurrente. Manejo de WebSockets puro para streaming de datos simulado de alta velocidad.
* **Monitoreo TUI**: En lugar de depender siempre de Grafana, el proyecto provee una interfaz de terminal interactiva (TUI) hecha en `Ratatui` conectada al backend vía WebSockets para plotear CPU/Memoria y QPS en vivo.
* **SQLx Compile Time**: Todas las queries SQL son verificadas por el compilador de Rust antes del run-time contra la base de datos de desarrollo.
* **CLI Engine**: Herramienta de línea de comandos potente (`cargo run --bin cli -- load-test --users 1000`) construida con `Clap`.

## 4. Requerimientos / Features Clave
1. **API REST Ultra-rápida**: Implementar CRUD estándar de alto rendimiento sin memory leaks.
2. **Middleware Avanzado Tower**: Implementar un rate limiter global sin bloquear el thread local usando Tokio.
3. **WebSockets de Alta Frecuencia**: Cargar eventos de CPU/mem al frontend o TUI cliente en tiempo real.
4. **Validación Exhaustiva**: Uso fuerte del sistema de tipos de Rust y Serde para validar contratos entrantes.
5. **Benchmarks**: Proveer una suite de testeo (ej. usando `oha` o `wrk`) comparando este sistema vs una versión equivalente en FastAPI.

## 5. Diseño de Base de Datos
* `events`: id (UUID), type, payload (JSONB), created_at (Timestamp)
* `metrics_snapshots`: id, timestamp, cpu_usage, memory_usage
