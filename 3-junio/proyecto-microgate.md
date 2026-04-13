# ☁️ MicroGate

## 1. Visión general
MicroGate es una plataforma demostrativa de arquitectura de microservicios polyglot enfocada en despliegue cloud, alto rendimiento y automatización continua. Combina un API Gateway construido en Python que enruta tráfico hacia un microservicio ultra-ligero en Rust, comunicados eficientemente vía gRPC.

## 2. Tecnologías principales
* **Python Track**: FastAPI (Gateway), Redis (Caché/Ratelimit)
* **Rust Track**: Axum, Distroless
* **Comunicación**: gRPC, Protocol Buffers (Protobuf)
* **DevOps**: Docker Multi-stage, GitHub Actions, GitFlow

## 3. Arquitectura
* **GateKeeper (API Gateway)**: Punto de entrada único (FastAPI). Implementa Rate Limiting con Redis (ej. 100 req/min), autenticación centralizada (valida el JWT), caché de respuestas y el patrón Circuit Breaker.
* **AxumMicro (Microservicio Rust)**: Un servicio hiper-optimizado escrito en Rust con Axum. Empaquetado en una imagen de Docker *Distroless* (sin OS, solo binario, ~25MB).
* **ProtoLink (IPC)**: La comunicación entre GateKeeper (Python) y AxumMicro (Rust) no es por HTTP JSON, sino a través de contratos binarios y fuertemente tipados definidos en archivos `.proto` y compilados vía gRPC.
* **ShipIt (CI/CD)**: Flujo de trabajo en GitHub Actions que ejecuta lint, test, build de las imágenes Docker y empuje a un Docker Registry simulado ante cada PR en `main`.

## 4. Requerimientos / Features Clave
1. **Contratos Protocol Buffer**: Definición unificada de servicios en `.proto` compartida entre Python y Rust.
2. **API Gateway con Auth**: El gateway valida el token. El microservicio Rust confía en el gateway (o recibe payload interno de contexto).
3. **Resiliencia**: Si el microservicio Rust cae, el Circuit Breaker del gateway debe activarse y retornar 503 Service Unavailable rápido en lugar de esperar timeout.
4. **Contenedores Maximizados**: Dockerfile multi-stage en Rust pasando desde el builder (rust:slim) a `gcr.io/distroless/cc-debian12`.
5. **GitFlow Automático**: Acción de GitHub que corra `cargo test` para Rust y `pytest` para Python.

## 5. Endpoints Principales
* `GET /gateway/health` -> Chequea latencia a Redis y a Axum.
* `POST /gateway/process` -> Traduce payload JSON a gRPC Protobuf y lo envía a Rust.
