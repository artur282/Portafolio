# Plan de Portafolio — Luis Cruz

> **Fuente de verdad única.** Todo lo demás (planes por proyecto, estándares, contexto) cuelga de este documento.
> **Actualizado:** 2026-09-10 · **Estado:** decisiones cerradas, listo para ejecutar en diciembre

---

## 1. Objetivo

Conseguir entrevistas para puestos **backend/backend-AI** en España (feb 2027) y, después, en **Suiza/Alemania**. El portafolio es el vehículo: dos sistemas de producción reales, desplegados, medidos y documentados.

**Presupuesto: 0 €.** Todo open source y local. Modelos de IA vía **OpenRouter `:free`**.

## 2. Qué se construye

| # | Proyecto | Qué es | Lenguaje dominante | Mercado objetivo |
|---|---|---|---|---|
| **A** | **NexusBoard** | Base de conocimiento colaborativa con IA: multi-tenant, búsqueda híbrida (full-text + semántica), asistente RAG, edición en tiempo real, versionado y **CLI en Rust** para que agentes IA consulten y alimenten la base | **Python** (FastAPI) | España |
| **B** | **Cortex** | Orquestador de agentes IA: **message bus propio en Rust**, pipelines, tracing, métricas y **benchmarks publicados contra un baseline Python** | **Rust** (Axum/Tokio) | Suiza/Alemania |
| **C** | **Web personal** | Una página limpia que presenta los proyectos con sus métricas reales, CV descargable y enlace al blog | React | Ambos |

**Independientes entre sí.** Cada uno se levanta solo con `docker compose up` y arranca con datos precargados (nada de pantallas vacías).

## 3. Decisiones cerradas

Estas decisiones están tomadas; no se vuelven a discutir salvo causa mayor.

| # | Decisión | Elección |
|---|---|---|
| 1 | Origen de los proyectos | **Desde cero.** Los anteriores (KnowledgeForge, SaaSForge, la spec LedgerCore) sirven solo como referencia de patrones |
| 2 | Ejecución | **Todos los agentes IA (OpenCode)** — backend, frontend, CLI, tests, CI, infraestructura y documentación |
| 3 | Alcance | **Sin recortes** |
| 4 | Presupuesto | **0 €** — OpenRouter `:free`; sin servicios de pago; sin dominio propio |
| 5 | Idiomas de las interfaces | **ES / EN / DE** obligatorios, con test de paridad de claves |
| 6 | Datos de demo | **Precargados siempre** (seed determinista al arrancar) |
| 7 | Repositorios | **Públicos desde el inicio** |
| 8 | Posicionamiento | Semi-senior / senior, sin cerrarse puertas |
| 9 | Mercado y fechas | Aplicar en **enero** a España **y** remote-EU; mudanza en **febrero** |
| 10 | Blog técnico | **Sí**, 2 posts (uno por proyecto) en enero |
| 11 | Ventana de trabajo | **Diciembre** a tiempo completo (preparación ligera en octubre-noviembre) |
| 12 | Compensación del riesgo "no sé defender el código" | **Documentación de aprendizaje en español por épica** (`docs/learning/`) — entregable obligatorio, no extra |

## 4. Evidencia que fundamenta el plan

Todo el material crudo está en `datos/`:

| Archivo | Qué contiene |
|---|---|
| `datos/ofertas_espana_backend.md` | **100 ofertas reales** de LinkedIn (España + CH/DE) con enlaces |
| `datos/analisis_demanda_python_rust.md` | Distribución por lenguaje, dominios Rust, ciudades, empresas que repiten |
| `datos/investigacion-portafolios-2026.md` | Qué funciona en portafolios, con fuentes verificables (HN, dev.to, Medium, GitHub) |

**Los cinco datos que más pesan:**

1. **España:** Python 26,5% de las ofertas · Rust 15 · IA 11 (10 de 11 en Barcelona). Ciudades: Barcelona 36, Madrid 20, Valencia 13, Málaga 10.
2. **Suiza/Alemania:** **Rust 70,6%**, Python 0 → el portafolio necesita un proyecto Rust fuerte para ese mercado.
3. **El hueco real de mi perfil:** ningún repo mío demuestra **Kafka, gRPC, Kubernetes ni Rust de backend** — justo lo que piden las ofertas mejor pagadas.
4. **La brecha más barata de cerrar: CI.** **0 workflows en 11 repos** mientras mi perfil anunciaba "CI/CD con GitHub Actions".
5. **Lo que de verdad valoran quienes contratan:** README con **métricas medibles**, diagrama de arquitectura, ADRs y una sección honesta de limitaciones. El sitio web personal pesa mucho menos de lo que se cree; el **blog técnico** sí genera oportunidades inbound.

## 5. Modelo de ejecución

| Aspecto | Cómo funciona |
|---|---|
| **Quién construye** | Agentes IA con **OpenCode**, dentro de cada carpeta de proyecto |
| **Cómo arrancan** | Leyendo el `AGENTS.md` de su carpeta, que les dirige a `docs/PLAN.md`, `docs/STANDARDS.md` y `docs/CONTEXT.md` |
| **Ritmo** | Tarea por tarea según `docs/PLAN.md`; un commit por tarea; nada avanza con tests rojos |
| **Cierre de cada épica** | Tests verdes + CI verde + **documento de aprendizaje en español** de esa épica |
| **Qué lee el dueño** | `docs/learning/` en español: qué se construyó, cómo funciona, por qué así, las 5 preguntas de entrevistador con respuestas, cómo verificarlo, conceptos a entender |
| **Cierre de cada proyecto** | `docs/learning/00-mapa-del-repo.md` (leer el repo en 30 min) + `99-preparacion-entrevista.md` (pitch de 60 s, 10 preguntas probables, 3 números para memorizar) |

## 6. Estándares obligatorios (detalle en `docs/STANDARDS.md` de cada proyecto)

1. **CI desde el primer commit**, con servicios reales (Postgres/Redis) y badge en el README
2. **README con estructura fija:** pitch → problema → demo (GIF + un comando) → arquitectura (diagrama) → decisiones/trade-offs → **resultados medidos** → stack → cómo ejecutarlo → **"Suggested review path"** → **"Limitaciones"** honesta → licencia
3. **ADRs** en `docs/decisions/` (mínimo 4 por proyecto)
4. **Métricas publicadas** — si no está medido, no se publica (p95, throughput, recall@k, cobertura, coste)
5. **Demo de un comando** con datos precargados + **vídeo de 45-90 s** + GIF de 10-15 s
6. **i18n ES/EN/DE** verificado por test
7. **Higiene:** sin secretos en el historial, `.gitignore` correcto, LICENSE, topics, rama `main`
8. **Documentación de aprendizaje** en español (ver §5)

## 7. Cronograma

| Periodo | Trabajo |
|---|---|
| **Septiembre** | Reparar el toolchain de Rust (`rustup` está roto) · revisar la higiene (secretos y `.gitignore`) de los repos que queden · retirar los repos que no aportan |
| **Octubre-noviembre** (2-3 h/semana) | Crear los repos vacíos con CI · diseñar los datos semilla · escribir el CV ATS |
| **Diciembre** (tiempo completo) | NexusBoard completo · Cortex completo · web publicada |
| **Enero** | Aplicar a España y remote-EU · escribir los 2 posts técnicos · actualizar el perfil de GitHub |
| **Febrero** | Mudanza y entrevistas |

## 8. Definición de Hecho global

Un proyecto está terminado cuando:

- [ ] `docker compose up` levanta todo con datos precargados y sin errores
- [ ] CI verde en GitHub con badge visible
- [ ] Suites de tests pasan (cobertura ≥80% en el núcleo)
- [ ] README con todas las secciones del estándar, **con métricas reales**
- [ ] 4+ ADRs escritos
- [ ] UI traducida a ES/EN/DE con test de paridad
- [ ] Vídeo de 45-90 s + GIF en el README
- [ ] `docs/learning/` completo (una por épica + mapa + preparación de entrevista)
- [ ] Demo pública accesible o binario instalable
- [ ] Topics, LICENSE y rama `main`

## 9. Riesgos

| Riesgo | Mitigación |
|---|---|
| Diciembre es un solo mes | Preparación ligera en octubre-noviembre + recortes ya decididos en `docs/PLAN.md` si aprieta (sin tocar estándares) |
| Dependencia de modelos gratuitos | Backoff y caché en las llamadas; embeddings locales (no dependen de API) |
| Toolchain de Rust roto | Reparar en septiembre, no en diciembre |
| No poder defender el código | Documentación de aprendizaje obligatoria por épica (§5) |
| Dispersión | Este documento es la única fuente de verdad; el resto cuelga de él |
