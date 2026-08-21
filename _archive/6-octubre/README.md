# 🎯 Octubre — Rust & Semantic Search

> _"Implementar desde cero lo que usás como caja negra en producción."_

## 🎯 Objetivo del mes

Capstone de Rust: construir un motor de búsqueda vectorial semántica desde los fundamentos matemáticos (cosine similarity, SIMD), exponerlo como gRPC service y documentar benchmarks reales contra pgvector HNSW. Python orquesta; Rust computa.

---

## 📅 Proyectos del mes

### 🏗️ Proyecto Principal: [VectoRust](./proyecto-vectorust.md)
Motor de búsqueda vectorial FLAT con SIMD en Rust puro, expuesto como servicio gRPC (tonic). Python/FastAPI maneja la extracción de embeddings (OpenAI text-embedding-3-small) y enriquecimiento de resultados con metadata. Benchmark documentado comparando FLAT-SIMD vs pgvector HNSW vs IVFFlat con conclusiones de tradeoffs explícitas en un ADR.

- **Tecnologías**: Rust (tonic, portable-simd, mmap), FastAPI, gRPC/Protobuf, pgvector, OpenAI API
- **Estado**: ⬜ Pendiente

---

## 🧠 Habilidades que se desarrollan

- Matemática vectorial aplicada: cosine similarity, dot-product, L2 — implementadas con SIMD AVX2.
- gRPC con tonic (Rust server) + grpcio (Python client): contratos binarios tipados entre lenguajes.
- Memory-mapped files para carga de índices vectoriales de alta dimensión sin heap allocation.
- Metodología de benchmarking rigurosa: Criterion, hyperfine, perf — resultados reproducibles.
- Decision Records (ADR): documentar tradeoffs de arquitectura con datos empíricos.
