# Plan de Portafolio — Luis Cruz

> **Fuente de verdad pública.** Todo lo demás (planes por proyecto, estándares, contexto) cuelga de este documento.
> **Actualizado:** 2026-10-17 · **Estado:** proyectos definidos — **Caliber** (Python/IA) y **Continuum** (Rust) desde cero; ejecución en diciembre
> **Cambios (oct 2026):** Caliber y Continuum reemplazan a NexusBoard y Cortex tras una revisión de encaje con el mercado. Nuevo estándar: **front de evidencia** por proyecto. Se descarta la i18n trilingüe de los productos (interfaces en inglés).

---

## 1. Objetivo

Conseguir entrevistas para puestos **backend/backend-AI** en España (feb 2027) y, después, en **Suiza/Alemania**. El portafolio es el vehículo: dos sistemas de producción reales, desplegados, medidos y documentados.

**Presupuesto: 0 €.** Todo open source y local. Modelos de IA vía **OpenRouter `:free`**.

## 2. Qué se construye

| # | Proyecto | Qué es | Lenguaje dominante | Mercado objetivo |
|---|---|---|---|---|
| **A** | **Caliber** | Laboratorio de calidad para búsqueda y RAG: corpus + preguntas de oro, experimentos comparables (texto/vectorial/híbrido, chunking, embeddings), métricas reproducibles (recall@k, MRR, nDCG) y **consola de evidencia** | **Python** (FastAPI) | España |
| **B** | **Continuum** | Motor de ejecución durable en Rust: workflows que sobreviven caídas (journal event-sourced), reintentos, timers, señales y **efectos exactly-once**, con benchmarks publicados | **Rust** (Axum/Tokio) | Suiza/Alemania |
| **C** | **Web personal (terminal)** | Página estilo terminal interactiva que presenta los proyectos con sus métricas y demos, CV descargable y blog | Next.js + React | Ambos |

**Independientes entre sí.** Cada uno se levanta solo con `docker compose up` y arranca con datos precargados (nada de pantallas vacías).

## 3. Decisiones cerradas

| # | Decisión | Elección |
|---|---|---|
| 1 | Origen de los proyectos | **Desde cero.** Los anteriores (KnowledgeForge, SaaSForge y las specs archivadas) sirven solo como referencia de patrones |
| 2 | Ejecución | **Agentes IA (OpenCode)** bajo mi dirección — backend, frontend, tests, CI, infraestructura y documentación |
| 3 | Alcance | **Completo, con recortes definidos de antemano** por proyecto (`docs/PLAN.md`); los estándares nunca se recortan |
| 4 | Presupuesto | **0 €** — OpenRouter `:free`; sin servicios de pago; sin dominio propio |
| 5 | Idiomas | **Inglés** en código, README e interfaces; documentación de aprendizaje en español; CV en ES/EN/DE |
| 6 | Datos de demo | **Precargados siempre** (seed determinista al arrancar) |
| 7 | Repositorios | **Públicos desde el inicio** |
| 8 | Posicionamiento | Semi-senior / senior, sin cerrarse puertas |
| 9 | Mercado y fechas | Aplicar en **enero** a España **y** remote-EU; mudanza en **febrero** |
| 10 | Blog técnico | **Sí**, 2 posts (uno por proyecto) en enero |
| 11 | Ventana de trabajo | **Diciembre** a tiempo completo (preparación ligera en octubre-noviembre) |
| 12 | Compensación del riesgo "no sé defender el código" | **Documentación de aprendizaje en español por épica** (`docs/learning/`) — entregable obligatorio, no extra |
| 13 | Evidencia primero | Cada proyecto cierra con **front de evidencia** (lo que un revisor ve en 60 s), visita guiada, demo pública, GIF y vídeo |
| 14 | Proyectos (oct 2026) | **Caliber + Continuum desde cero**; se descartan NexusBoard y Cortex (quedan como referencia) |

## 4. Evidencia que fundamenta el plan

La investigación de mercado —**100 ofertas reales** de LinkedIn (España + CH/DE, agosto 2026)— se mantiene en privado; estas son las conclusiones que definieron el plan:

**Los cinco datos que más pesan:**

1. **España:** Python 26,5% de las ofertas · Rust 15 · IA 11 (10 de 11 en Barcelona). Ciudades: Barcelona 36, Madrid 20, Valencia 13, Málaga 10.
2. **Suiza/Alemania:** **Rust 70,6%**, Python 0 → se necesita un proyecto Rust fuerte para ese mercado.
3. **El hueco real de mi perfil:** ningún repo mío demuestra Kafka, gRPC, Kubernetes ni Rust de backend — justo lo que piden las ofertas mejor pagadas.
4. **La brecha más barata de cerrar: CI.** 0 workflows en mis repos mientras mi perfil prometía "CI/CD con GitHub Actions".
5. **Lo que de verdad valoran quienes contratan:** README con **métricas medibles**, diagrama de arquitectura, ADRs y una sección honesta de limitaciones. El sitio personal pesa mucho menos de lo que se cree; el **blog técnico** sí genera oportunidades inbound.

## 5. Modelo de ejecución

| Aspecto | Cómo funciona |
|---|---|
| **Quién construye** | Agentes IA con **OpenCode**, dentro de cada carpeta de proyecto |
| **Cómo arrancan** | Leyendo el `AGENTS.md` de su carpeta, que les dirige a `docs/PLAN.md`, `docs/STANDARDS.md` y `docs/CONTEXT.md` |
| **Ritmo** | Tarea por tarea según `docs/PLAN.md`; un commit por tarea; nada avanza con tests rojos |
| **Cierre de cada épica** | Tests verdes + CI verde + **documento de aprendizaje en español** + su parte visible en el front cuando aplique |
| **Qué lee el dueño** | `docs/learning/` en español: qué se construyó, cómo funciona, por qué así, las 5 preguntas de entrevistador con respuestas, cómo verificarlo |
| **Cierre de cada proyecto** | `docs/learning/00-mapa-del-repo.md` (leer el repo en 30 min) + `99-preparacion-entrevista.md` (pitch de 60 s, 10 preguntas probables, 3 números para memorizar) |

## 6. Estándares obligatorios (detalle en `docs/STANDARDS.md` de cada proyecto)

1. **CI desde el primer commit**, con servicios reales (Postgres/Redis) y badge en el README
2. **README evidencia-first:** primera pantalla = GIF + demo pública + tabla de métricas reales; después pitch → problema → arquitectura → trade-offs → resultados medidos → stack → cómo ejecutarlo → "Suggested review path" → limitaciones honestas → licencia
3. **ADRs** en `docs/decisions/` (mínimo 4 por proyecto)
4. **Métricas publicadas** — si no está medido, no se publica (p95, throughput, recall@k, cobertura, tiempo de recuperación)
5. **Demo de un comando** con datos precargados + **vídeo de 45-90 s** + GIF de 10-15 s
6. **Front de evidencia + visita guiada de 60 s**: cada pantalla prueba una capacidad; el tour dispara las acciones clave y narra la tecnología
7. **Higiene:** sin secretos en el historial, `.gitignore` correcto, LICENSE, topics, rama `main`
8. **Documentación de aprendizaje** en español (ver §5)

## 7. Cronograma

| Periodo | Trabajo |
|---|---|
| **Septiembre** | Inventario del perfil · rotación de claves del `.env` filtrado · retirada de repos que no aportan |
| **Octubre-noviembre** (2-3 h/semana) | Specs de Caliber y Continuum (escritas el 17 oct) · plan de la web terminal · crear los repos vacíos con CI · diseñar los datos semilla · escribir el CV ATS |
| **Diciembre** (tiempo completo) | Caliber completo · Continuum completo · web terminal publicada |
| **Enero** | Aplicar a España y remote-EU · escribir los 2 posts técnicos · actualizar el perfil de GitHub |
| **Febrero** | Mudanza y entrevistas |

## 8. Definición de Hecho global

Un proyecto está terminado cuando:

- [ ] `docker compose up` levanta todo con datos precargados y sin errores
- [ ] CI verde en GitHub con badge visible
- [ ] Suites de tests pasan (cobertura ≥80% en el núcleo)
- [ ] README evidencia-first con métricas reales
- [ ] 4+ ADRs escritos
- [ ] **Front de evidencia desplegado (demo pública) con visita guiada de 60 s**
- [ ] Vídeo de 45-90 s + GIF en el README
- [ ] `docs/learning/` completo (una por épica + mapa + preparación de entrevista)
- [ ] Topics, LICENSE y rama `main`

## 9. Riesgos

| Riesgo | Mitigación |
|---|---|
| Diciembre es un solo mes | Preparación ligera en octubre-noviembre + recortes ya decididos en `docs/PLAN.md` (sin tocar estándares) |
| Dependencia de modelos gratuitos | Backoff y caché en las llamadas; embeddings locales (no dependen de API) |
| No poder defender el código | Documentación de aprendizaje obligatoria por épica (§5) |
| Scope del front vs núcleo | El front es capa de evidencia con alcance fijo (60 s); todo extra va a la lista de recortes del proyecto |
| Dispersión | Este documento es la única fuente de verdad; el resto cuelga de él |
