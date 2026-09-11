# Análisis de demanda — Python/Rust en ofertas (España, Suiza, Alemania)
Fuente: `ofertas_espana_backend.md` (LinkedIn, consulta ago 2026). Análisis cuantitativo del **texto real del archivo**.

## 0. Aviso metodológico (leer antes de usar los números)

1. **El archivo solo contiene títulos** (`- Empresa · Puesto — Ciudad [URL]`). No hay descripciones de puesto, ni años de experiencia, ni salarios (única excepción: NXT Hero, "hasta 85k€"), ni listas de frameworks, nubes, colas o testing.
2. Por tanto: **el "stack" que se puede medir es el que aparece en los títulos**. Todo lo demás (FastAPI, Kafka, AWS, Docker, pytest…) **no está en el archivo** y no puede afirmarse a partir de él.
3. **Recuento real ≠ recuento anunciado**:
   - Lote 1: **25** ofertas (coincide con el encabezado).
   - Lote 2: **48** ofertas (coincide).
   - Lote 3: **27** ofertas, aunque el encabezado dice 30 → España **10** ✓, **Suiza 7 (dice 10)**, Alemania **10** ✓.
   - **Total real del archivo: 100 ofertas** (101 si se cuenta NVIDIA Zurich "(×2)" como dos vacantes). El contexto del proyecto hablaba de 103: ese número **no se sostiene** contra el fichero; la sección "Suiza (10)" tiene 7 bullets.
4. El archivo **no contiene ninguna oferta de Python para Suiza/Alemania** (las 30/27 del Lote 3 son todas Rust salvo una mixta Rust/Python).

---

## 1. Distribución por lenguaje principal (base: 100 ofertas)

| Lenguaje / tipo (bucket primario, según el título) | Ofertas | % |
|---|---|---|
| **Rust** | **27** | **27,0 %** |
| **Python** (incl. Django) | **22** | **22,0 %** |
| Genérico backend (sin lenguaje en el título) | 28 | 28,0 % |
| IA/ML (puesto IA, sin lenguaje declarado) | 11 | 11,0 % |
| PHP / Symfony | 3 | 3,0 % |
| Node.js / Nest | 2 | 2,0 % |
| Go / Golang | 2 | 2,0 % |
| .NET / ASP.NET | 2 | 2,0 % |
| Scala | 1 | 1,0 % |
| Java | 1 | 1,0 % |
| **Rust + Python** (Munich Electrification) | **1** | **1,0 %** |
| **Total** | **100** | **100 %** |

Agregando familias: **Rust = 28 (28 %)**, **Python = 23 (23 %)**, **Rust o Python = 51 (51 %)**. El 28 % restante son títulos "Backend/Software Engineer" sin lenguaje declarado, que en su mayoría están en ciudades donde Python y Rust dominan (Barcelona/Madrid) pero **el archivo no lo permite afirmar**.

Desglose por lote:

| Lenguaje | Lote 1 (25) | Lote 2 (48) | Lote 3 ES (10) | Lote 3 CH (7) | Lote 3 DE (10) |
|---|---|---|---|---|---|
| Rust | 5 | 0 | 10 | 5 | 7 |
| Python | 15 | 7 | 0 | 0 | 0 |
| Rust+Python | 0 | 0 | 0 | 0 | 1 |
| IA (sin lenguaje) | 1 | 10 | 0 | 0 | 0 |
| Genérico | 4 | 20 | 0 | 2 | 2 |
| Otros lenguajes | 0 | 11 | 0 | 0 | 0 |

---

## 2. Top 15 señales más repetidas en el archivo (frecuencia literal de mención)

⚠️ Es una mezcla de **nivel, rol y tecnología** porque el archivo no da más granularidad. Las "habilidades" en sentido estricto son las filas marcadas (tech).

| # | Señal mencionada | Nº de las 100 ofertas | % |
|---|---|---|---|
| 1 | "Backend" / "Back-End" (rol) | 42 | 42 % |
| 2 | Senior / Sr. (nivel) | 39 | 39 % |
| 3 | **Rust** (tech) | 28 | 28 % |
| 4 | **Python** (tech) | 23 | 23 % |
| 5 | "Software Engineer" (rol) | 21 | 21 % |
| 6 | **IA / AI / ML / GenAI / Agentic** (tech) | 16 | 16 % |
| 7 | Developer / Desarrollador / Programador (rol) | 35 | 35 % |
| 8 | Puesto 100 % IA/ML (rol) | 11 | 11 % |
| 9 | **Django** (tech) | 2 | 2 % |
| 10 | **Sistemas distribuidos** (domain) | 2 | 2 % |
| 11 | **Trading** (domain) | 2 | 2 % |
| 12 | Architect (rol) | 2 | 2 % |
| 13 | **Node.js / Nest** (tech) | 2 | 2 % |
| 14 | Mid / Middle (nivel) | 4 | 4 % |
| 15 | **Cripto (Kraken + Keyrock)** (domain) | 5 | 5 % |

Mencionadas **una sola vez**: Cloud, APIs, SQL/Oracle/PostgreSQL, DevOps, Angular, Fullstack, Banking, Data Pipeline, VPN, Scala, Java.

### Lo que NO aparece en el archivo (y por tanto no se puede medir)
- **FastAPI: 0 menciones.** El stack principal declarado del usuario (FastAPI) **no aparece ni una vez**; el único framework web nombrado es Django (2). Esto es un dato, no un problema — pero implica que "FastAPI" no es una palabra clave que las ofertas de este archivo usen.
- Mensajería/colas (Kafka, RabbitMQ, SQS): **0**.
- Cloud por proveedor (AWS/GCP/Azure): **0** (solo la palabra genérica "Cloud", 1 vez).
- Bases de datos: solo 1 oferta las nombra (UST: SQL / Oracle / PostgreSQL). **No hay forma de medir PostgreSQL vs MySQL con este archivo.**
- Testing (pytest, unittest), CI/CD, Docker/Kubernetes: **0**.
- Años de experiencia: **0 ofertas** los indican.
- Una única referencia salarial: NXT Hero, Berlín, "hasta 85k€".

---

## 3. Ofertas Python: qué se repite

- Volumen: **22 ofertas** (15 en Lote 1, 7 en Lote 2). Es el **lenguaje más pedido de España** después de los títulos genéricos.
- **Nivel**: 15 de 22 dicen "Senior/Sr." (**65 %**) — no hay un solo junior y **0 mids**. Es un mercado senior.
- Frameworks: **Django 2** (Plexus Tech Madrid, knowmad mood Las Rozas). FastAPI 0. Angular aparece 1 vez (SII Group) → perfil fullstack Python+Angular.
- Datos: 1 sola oferta explicita BD (UST: SQL/Oracle/PostgreSQL, 100 % remoto).
- Cloud/APIs: 1 sola oferta lo nombra (Managing Composites: "Python, Cloud & APIs").
- IA dentro de ofertas Python: **3** (METRICA "Senior Python Engineer con IA", PrimeIT "Backend Engineer (Python & IA)", Windsor.ai por el dominio). Alrededor hay 11 puestos IA puros adicionales, casi todos en Barcelona.
- Sector: música/media (BMAT), seguros/comparadores (Windsor.ai), pagos (Aircall), facturación (Manychat), consultoría/outsourcing (ALTEN, Indra ×2, Capitole, Plexus Tech, knowmad mood, Scalian ×2, UST, SII, PrimeIT, Shipco IT), retail industrial (Mecalux), telco/marketing (Delectatech, Expereo).

## 4. Ofertas Rust: qué dominio

Las 28 ofertas Rust (5 en España Lote 1 + 23 en Lote 3) se reparten en dominios concretos:

| Dominio | Ofertas | Ejemplos del archivo |
|---|---|---|
| **Cripto / trading de alta frecuencia** | 4+ | Keyrock (Madrid; Zug y Zurich) "Rust Engineer – Trading Systems", Kraken ×3 (Payward, Consumer, España/remoto) |
| **Infra / plataforma / sistemas distribuidos** | 4 | Wasmer (Madrid) "Rust Engineer, Distributed Systems", Rivero (Zurich) "Core Platform", Coralogix (Berlín) "Data Pipeline", Code Compass (Zurich) |
| **Banca / fintech** | 1 | Scalable Capital (Munich) "Rust SW Engineer Banking Platform" |
| **ML inference / sistemas embebidos y audio** | 2 | ai-coustics (Berlín) "Rust, ML Inference", Munich Electrification (Rust/Python) |
| **Robótica / drones / defensa** | 4 | Quantum Systems (Munich), Helsing (Berlín/Munich), Loki Robotics (Zurich), THEKER Robotics (Barcelona, IA) |
| **Automatización/workflow y SaaS empresarial** | 5 | Workato ×4 (Barcelona, Madrid, Berlín ×2), Nelly Solutions (Berlín), Fusion Consulting (Barcelona) |
| **Hardware/GPU y sanidad** | 4 | NVIDIA (Zurich), Roche (Sant Cugat, Rotkreuz), ERNI ×3 (Madrid, Valencia, Basilea) |
| **Telco / comunicaciones** | 1 | RingCentral (Valencia) |

Rasgo común: **Rust = sistemas con latencia baja, concurrencia real y datos críticos**, no CRUD. Distribuido, trading, streaming, inferencia, embedded.

---

## 5. Empresas destacadas por lenguaje y ciudad

**Python (España)**
- Madrid: Plexus Tech, Windsor.ai, ALTEN, METRICA, Centribal (Majadahonda), Managing Composites, Aircall, PrimeIT, UST (remoto), knowmad mood (Las Rozas), Scalian.
- Barcelona y área: RDT, BMAT Music Innovators, Expereo, Delectatech, Capitole, Indra ×2, Manychat, Mecalux (Cornellà), King, Shipco IT, SII Group (remoto).
- Nacional/remoto: Scalian (Spain), UST (100 % remoto).

**Rust (España)** — 15 ofertas
- Madrid: Workato (System Architect GO/Rust), Wasmer, ERNI, Keyrock (trading).
- Barcelona: Workato, Fusion Consulting. · Sant Cugat del Vallès: Roche (Lead).
- Valencia: ERNI, RingCentral.
- España/remoto: Kraken (Consumer).

**Rust (Suiza)** — 7 ofertas: NVIDIA (Zurich, ×2), Rivero (Zurich), Code Compass (Zurich), Loki Robotics (Zurich), Keyrock (Zug y Zurich), Roche (Rotkreuz), ERNI (Basilea).

**Rust (Alemania)** — 10 ofertas: Workato ×2, Nelly Solutions, ai-coustics, NXT Hero (100 % remoto, ≤85k€), Coralogix (Berlín); Helsing (Berlín y Munich); Munich Electrification (Rust/Python), Scalable Capital (banking), Quantum Systems (Munich).

**Empresas que repiten** (señal de contratación activa y multi-país):
| Empresa | Nº | Ubicaciones / lenguaje |
|---|---|---|
| Workato | 5 | Barcelona, Madrid, Berlín ×2 (Rust) |
| Kraken | 3 | España/remoto (Rust, cripto) |
| ERNI | 3 | Madrid, Valencia, Basilea (Rust) |
| Keyrock | 2 | Madrid, Zug/Zurich (Rust, trading) |
| Roche | 2 | Sant Cugat, Rotkreuz (Rust) |
| RingCentral | 2 | Valencia (Rust) |
| Wasmer | 2 | Madrid (Rust, sistemas distribuidos) |
| Indra | 2 | Barcelona (Python) |
| Scalian | 2 | España / Madrid (Python) |
| King | 2 | Barcelona (Python / game services) |
| Valsea | 2 | Marbella (Go y Scala) |
| Vortech | 2 | Valencia (.NET) |

---

## 6. España vs Suiza/Alemania

| Dimensión | España (83 ofertas) | Suiza + Alemania (17 ofertas) |
|---|---|---|
| Lenguaje dominante | **Python (22, 26,5 %)** + mucho genérico (24) y Rust (15) | **Rust (12, 70,6 %)** + 1 mixta Rust/Python + 4 genéricas |
| Rust | 15 (18,1 %) — Barcelona, Madrid, Valencia | 12 (70,6 %) — Zurich, Rotkreuz, Zug, Basilea, Berlín, Munich |
| Python | 22 (26,5 %) | **0** (guardado: 4 genéricas de Coralogix/Helsing/Loki/Rivero, cuyo stack no consta) |
| IA/ML | 11 puestos (13,3 %), 10 de ellos en Barcelona | 1 (ai-coustics, Rust + ML inference) |
| Dominio | CRUD/SaaS/consultoría, gaming, banca, IA aplicada | Sistemas, trading cripto, banca, defensa/drones, GPU, inferencia |
| Ciudades | Barcelona área 36 · Madrid 20 · Valencia/CV 13 · Málaga/Andalucía 10 · nacional/remoto 4 | Berlín 6 · Munich 3 · Berlín+Munich 1 · Zurich 4 (5 vacantes) · Zug/Zurich 1 · Rotkreuz 1 · Basilea 1 |
| Remote explícito | 4 ofertas (UST, SII, Kraken, Scalian-Spain) | 2 (NXT Hero Berlín; Kraken España/remoto contado en ES) |
| Salario | 0 datos en el archivo | 1 dato: ≤85k€ (NXT Hero, Berlín, remoto) |

Conclusión direccional: **España contrata Python (aplicación/negocio) + IA; el mercado Rust fuerte está fuera de España (CH/DE), y en España se concentra en trading cripto, sistemas distribuidos y telco.**

---

## 7. Seniority y años de experiencia

- **El archivo no contiene años de experiencia en ninguna oferta.** Imposible responder "pide X años" con estos datos.
- Nivel por título (100 ofertas): **Senior/Sr. 39 (39 %)**, Mid/Middle 4 (4 %), Lead 2 (2 %), Founding 1, "Engineer II" 1, Architect 2, sin nivel explícito 51 (51 %). **Junior/trainee: 0.**
- Desglose por lenguaje:
  - **Rust**: 28 ofertas → Senior 14 (**50 %**), Mid 0, sin nivel explícito 14 (incluye "Rust Engineer" a secas, que en la práctica es mid/senior).
  - **Python**: 23 ofertas → Senior 15 (**65 %**), Mid 0, sin nivel explícito 8.
  - **Mids mencionados**: Valeria HR (AI, Barcelona), hiberus (Java, Valencia), FMIT (Málaga), Shalion (AI, Barcelona). Todos en España.
- Lectura: un portafolio **debe posicionarse como senior/mid-alto**; no hay entrada junior en este conjunto.

---

## 8. Recomendación: 5 capacidades que el portafolio debe demostrar sí o sí

Ordenadas por evidencia del archivo (no por gusto personal):

1. **Servicio backend Python en producción, no un script.** Python = 23 ofertas (23 %), 65 % senior, y **todas** son de backend/API. El proyecto debe mostrar API versionada, autenticación, migraciones, manejo de errores, observabilidad y tests — aunque el archivo no nombre FastAPI, es el patrón de puesto que domina. *(Nota honesta: Django se nombra 2 veces y FastAPI 0; el título mide cargo, no framework. No usar el archivo para elegir framework.)*
2. **Rust para sistemas concurrentes y de baja latencia**, no CLI ni ejercicios de sintaxis. Rust = 28 ofertas (28 %), y los dominios son trading (Keyrock, Kraken), sistemas distribuidos (Wasmer, Rivero), banca (Scalable Capital), data pipelines (Coralogix), inferencia (ai-coustics) y defensa/robótica (Helsing, Quantum Systems). Un proyecto con Tokio, servidor Axum, backpressure, benchmarks y un número medido de latencia/throughput responde exactamente a ese patrón.
3. **Integración real de IA/LLM con Rust o Python** (RAG, agentes, inferencia). IA aparece en **16 ofertas (16 %)** y hay **11 puestos IA puros, 10 de ellos en Barcelona**. Debe ser un servicio desplegado con LLM/embeddings detrás de una API, con evaluación y coste controlados — no un notebook.
4. **Fundamentos de datos transaccional y consistencia (SQL real + idempotencia).** Es el único requisito técnico de datos que el archivo explicita (UST: SQL/Oracle/PostgreSQL) y es transversal a los dominios de mayor valor que sí aparecen: pagos/billing (4 ofertas), trading (2), banca (1). Demostrar transacciones, índices, migraciones, idempotencia y manejo de concurrencia.
5. **Alcance internacional/remoto y trabajo en inglés.** 4 ofertas remotas explícitas + 20 ofertas de CH/DE + empresas multi-país (Workato 5, ERNI 3, Kraken 3, Keyrock 2) → un proyecto con README en inglés, CI público y despliegue reproducible es el mínimo para CH/DE, donde el archivo **no ofrece ninguna vía Python**.

**Ciudades objetivo por retorno**: Barcelona (36 ofertas, + IA), Madrid (20, Python + trading), Valencia/CV (13, Rust telco + Python), y en el exterior Zurich/Berlín/Munich (Rust únicamente).

### Riesgos / huecos del dataset a cerrar antes de decidir el portafolio
- Faltan las **descripciones** de las 100 ofertas: sin ellas no se puede medir FastAPI, Kafka, AWS, Kubernetes, testing ni años de experiencia — precisamente lo que pide el enunciado.
- Faltan los **últimos 3 bullets de Suiza** (encabezado dice 10, hay 7).
- Scalian "Python Developer" (líneas 66 y 73) y King (49, 79) pueden ser duplicados o vacantes distintas: no verificable desde este archivo.
- 0 ofertas para Python en Suiza/Alemania en este dataset → si el objetivo es CH/DE, el archivo solo justifica Rust.
