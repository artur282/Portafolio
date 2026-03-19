# 📊 Mes 03 — EvalForge

> **Framework de evaluación y observabilidad para agentes IA con paneles de calidad, coste, latencia y tasa de alucinaciones**

| Campo              | Detalle                                                     |
| ------------------ | ----------------------------------------------------------- |
| 📅 Mes             | Mayo 2026                                                   |
| 🏷️ Categoría       | Evaluación & Observabilidad                                 |
| ⏱️ Tiempo estimado | ~40 horas (1 mes a tiempo parcial)                          |
| 📦 Repositorio     | [artur282/EvalForge](https://github.com/artur282/EvalForge) |
| 📊 Dificultad      | ⭐⭐⭐ Intermedio-alto                                      |
| 🎯 Competencia     | Evaluación LLM + Observabilidad + A/B Testing estadístico   |

---

## 🎯 Caso de uso empresarial

**Problema:** Los equipos que despliegan agentes IA no tienen visibilidad sobre: ¿cuánto cuestan?, ¿se están degradando?, ¿qué tasa de alucinaciones tienen?, ¿qué pasa cuando cambiamos el prompt?

**Solución:** Un framework de evaluación + observabilidad que:

- Diseña harness de evaluación (offline con golden sets + online con tráfico real)
- Instrumenta los agentes con Langfuse (trazas, spans, métricas)
- Ejecuta A/B tests estadísticos entre versiones de prompts o modelos
- Detecta regresiones automáticamente en CI/CD
- Presenta dashboards accionables (coste/interacción, task_success_rate, hallucination_rate)

**KPIs que mide este sistema:**

- Calidad: LLM-as-judge score, faithfulness, answer relevancy
- Coste: USD por interacción, USD por tarea completada
- Latencia: P50, P95, P99 por endpoint
- Fiabilidad: task_success_rate, error_rate, escalation_rate
- Seguridad: hallucination_rate, guardrail_trigger_rate

---

## 🏗️ Arquitectura

```
Agentes bajo evaluación
        │
        ▼
┌───────────────────────────────────────────────────────────┐
│                  EvalForge Platform                        │
│                                                           │
│  ┌─────────────────┐    ┌──────────────────────────────┐  │
│  │  Offline Eval   │    │     Observabilidad Online    │  │
│  │                 │    │                              │  │
│  │  Golden Sets    │    │  Langfuse SDK Integration    │  │
│  │  Synthetic Data │    │  Trace → Span → Generation   │  │
│  │  RAGAS Suite    │    │  Métricas en tiempo real     │  │
│  │  LLM-as-Judge   │    │  Alertas automáticas         │  │
│  └────────┬────────┘    └──────────────┬───────────────┘  │
│           │                            │                   │
│           └────────────┬───────────────┘                   │
│                        ▼                                   │
│  ┌─────────────────────────────────────────────────────┐  │
│  │              A/B Testing Engine                      │  │
│  │  Comparar prompts v1 vs v2 / GPT-4o vs Claude       │  │
│  │  t-test + Cohen's d + Bayes factor + Power analysis │  │
│  └────────────────────────┬────────────────────────────┘  │
│                           ▼                               │
│  ┌─────────────────────────────────────────────────────┐  │
│  │            Regression Detector (CI/CD)              │  │
│  │  Compara métricas actuales vs baseline              │  │
│  │  Falla el PR si calidad cae > umbral                │  │
│  └─────────────────────────────────────────────────────┘  │
└───────────────────────────────────────────────────────────┘
                        │
                        ▼
            Dashboard Grafana / Langfuse UI
```

---

## ✨ Features

### Evaluación Offline

- [ ] Runner de evaluación sobre golden datasets (JSON/CSV)
- [ ] Métricas automáticas: ROUGE-L, BERTScore, exactitud estructurada
- [ ] LLM-as-judge: quality, helpfulness, faithfulness, safety (structured output)
- [ ] RAGAS: faithfulness, answer_relevancy, context_precision, context_recall
- [ ] Generación de datos sintéticos para ampliar golden sets
- [ ] Reporte HTML/JSON por experimento

### Observabilidad Online con Langfuse

- [ ] SDK de Langfuse integrado en agentes de ejemplo
- [ ] Trazas completas: input → retrieval → prompt → LLM → output
- [ ] Métricas por traza: tokens (prompt + completion), coste estimado, latencia
- [ ] Scores customizados: LLM-as-judge en producción (evaluación asíncrona)
- [ ] Alertas: si `hallucination_rate > 0.10` o `task_success_rate < 0.80`

### A/B Testing estadístico

- [ ] Comparativa de dos variantes (prompt v1 vs v2)
- [ ] t-test de Welch + Cohen's d (effect size)
- [ ] Análisis bayesiano: probabilidad de que B > A
- [ ] Power analysis: cuántas muestras necesito para detección fiable
- [ ] Reporte de significancia y recomendación de ganador

### Evaluación de Seguridad

- [ ] Hallucination rate: respuestas no ancladas en contexto
- [ ] Toxicity detection usando Perspective API o local model
- [ ] PII leakage detection en outputs
- [ ] Guardrail trigger rate: qué % de queries activa los guardrails

### CI/CD Integration

- [ ] GitHub Actions: `make eval` corre evaluación completa
- [ ] Regression detector: compara con baseline JSON almacenado
- [ ] Comentario automático en PR con tabla de métricas
- [ ] Falla el pipeline si alguna métrica clave cae > 5%

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                                           |
| ------------------ | --------------------------------------------------- |
| **Langfuse**       | Observabilidad: trazas, métricas, prompt management |
| **RAGAS**          | Evaluación de pipelines RAG                         |
| **LangSmith**      | Evaluación alternativa + datasets management        |
| **DeepEval**       | Framework de evaluación con métricas predefinidas   |
| **FastAPI**        | API del servicio de evaluación                      |
| **Grafana**        | Dashboards de métricas operacionales                |
| **PostgreSQL**     | Almacenamiento de resultados de evaluación          |
| **SciPy/NumPy**    | A/B testing estadístico                             |
| **OpenAI API**     | LLM-as-judge                                        |
| **GitHub Actions** | CI/CD con evaluación automática                     |

---

## 📁 Estructura del proyecto

```bash
evalforge/
├── app/
│   ├── main.py
│   ├── routers/
│   │   ├── offline_eval.py        # Evaluación sobre golden set
│   │   ├── ab_test.py             # A/B testing estadístico
│   │   └── regression.py          # Detector de regresiones
│   ├── services/
│   │   ├── evaluators/
│   │   │   ├── ragas_evaluator.py
│   │   │   ├── llm_judge.py       # LLM-as-judge con structured output
│   │   │   ├── nlp_metrics.py     # ROUGE, BERTScore, BLEU
│   │   │   └── safety_metrics.py  # Hallucination, toxicity, PII
│   │   ├── ab_engine.py           # Motor de A/B testing
│   │   ├── regression_detector.py
│   │   └── synthetic_gen.py       # Generación datos sintéticos
│   └── models/
│       ├── experiment.py
│       └── metrics.py
├── agents/                        # Agentes de ejemplo instrumentados
│   ├── rag_agent.py               # Agente RAG con Langfuse SDK
│   └── chat_agent.py              # Chatbot simple instrumentado
├── evals/
│   ├── golden_sets/
│   │   ├── customer_support.json
│   │   ├── document_qa.json
│   │   └── data_extraction.json
│   ├── baselines/                 # Métricas baseline por experimento
│   └── results/
├── dashboards/
│   └── grafana/
│       └── ai_metrics.json        # Dashboard JSON importable
├── .github/
│   └── workflows/
│       └── eval_on_pr.yml         # Evaluación en cada PR
├── docs/
│   ├── evaluation_guide.md        # Guía: cómo crear golden sets
│   └── metrics_glossary.md        # Definición de cada métrica
├── tests/
├── docker-compose.yml             # App + PostgreSQL + Langfuse + Grafana
├── Makefile
└── .env.example
```

---

## 📡 API Endpoints

```http
# Evaluación offline
POST   /api/v1/eval/run              # Ejecutar evaluación sobre golden set
GET    /api/v1/eval/results          # Resultados por experimento
GET    /api/v1/eval/report/{id}      # Reporte HTML de evaluación

# A/B Testing
POST   /api/v1/ab/create             # Crear experimento A/B
POST   /api/v1/ab/{id}/record        # Registrar observación (variant, score)
GET    /api/v1/ab/{id}/analyze       # Análisis estadístico + ganador

# Regresión
POST   /api/v1/regression/check      # Comparar métricas vs baseline
POST   /api/v1/regression/baseline   # Guardar baseline actual

# Datos sintéticos
POST   /api/v1/synthetic/generate    # Generar pares Q&A sintéticos
```

---

## 📊 GitHub Actions — Evaluación en PR

```yaml
# .github/workflows/eval_on_pr.yml
name: AI Evaluation

on:
  pull_request:
    paths:
      - "prompts/**"
      - "app/services/**"

jobs:
  evaluate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run evaluation suite
        run: make eval
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
          LANGFUSE_SECRET_KEY: ${{ secrets.LANGFUSE_SECRET_KEY }}

      - name: Check for regressions
        run: make regression-check

      - name: Comment PR with results
        uses: actions/github-script@v6
        with:
          script: |
            const results = require('./evals/results/latest.json')
            const comment = `## 🧪 Evaluation Results
            | Métrica | Baseline | Actual | Cambio |
            |---------|----------|--------|--------|
            | Faithfulness | ${results.baseline.faithfulness} | ${results.current.faithfulness} | ${results.delta.faithfulness} |
            | Task Success | ${results.baseline.task_success} | ${results.current.task_success} | ${results.delta.task_success} |
            | Hallucination Rate | ${results.baseline.hallucination_rate} | ${results.current.hallucination_rate} | ${results.delta.hallucination_rate} |
            `
            github.rest.issues.createComment({...})
```

---

## 📈 Dashboard de métricas (Langfuse + Grafana)

**Paneles incluidos:**

1. **Calidad en tiempo real** — LLM-judge score y faithfulness (rolling 24h)
2. **Coste** — USD/interacción, USD/1K tokens, coste acumulado
3. **Latencia** — P50, P95, P99 por tipo de query
4. **Fiabilidad** — Task success rate, error rate, timeout rate
5. **Seguridad** — Hallucination rate, guardrail trigger rate, PII detections

---

## 🗓️ Plan mensual

### Semana 1 — Diseño del framework de evaluación

- Definir taxonomía de métricas para el dominio target
- Crear golden sets para 3 tipos de tarea (QA, extracción, resumen)
- Implementar evaluadores: ROUGE, BERTScore, LLM-judge
- Setup Langfuse local con Docker

### Semana 2 — Agentes instrumentados + observabilidad online

- Crear 2 agentes de ejemplo (RAG + chatbot) con Langfuse SDK
- Implementar métricas asíncronas: LLM-judge en producción
- Dashboard básico con métricas de coste y latencia
- Alertas configuradas (Langfuse webhooks)

### Semana 3 — A/B testing y detección de regresiones

- Motor de A/B testing con t-test + Cohen's d
- Regression detector comparando con baseline JSON
- Generación de datos sintéticos para ampliar golden sets
- GitHub Actions con evaluación en PR

### Semana 4 — Polish y documentación

- Dashboard Grafana con todos los paneles
- Guía de evaluación: cómo crear golden sets de calidad
- Glosario de métricas con definición y umbral recomendado
- README con resultados del primer experimento real

---

## ✅ Definición de "hecho"

- [ ] Framework de evaluación con ≥ 5 métricas implementadas
- [ ] 3 golden sets con ≥ 30 casos cada uno
- [ ] Langfuse: trazas completas de agentes de ejemplo en producción
- [ ] A/B test completo documentado con conclusiones estadísticas
- [ ] GitHub Actions: evaluación automática en PR con comentario de resultados
- [ ] Regression detector funcionando: falla PR si calidad cae > 5%
- [ ] Dashboard Grafana importable con los 5 paneles de métricas
- [ ] Guía de evaluación + glosario de métricas

---

## 💼 Lo que demuestra al reclutador

| Habilidad                   | Evidencia                                                |
| --------------------------- | -------------------------------------------------------- |
| Evaluación rigurosa de LLMs | 5+ métricas, golden sets, LLM-as-judge, RAGAS            |
| Observabilidad              | Langfuse: trazas, coste, latencia, métricas customizadas |
| A/B Testing estadístico     | t-test + Cohen's d + análisis bayesiano documentado      |
| Detección de regresiones    | CI/CD que protege la calidad del agente automáticamente  |
| Orientación a datos         | Decisiones basadas en métricas, no en intuición          |
| Seguridad                   | Hallucination rate, PII detection, guardrail monitoring  |
| MLOps                       | Evaluación integrada en pipeline CI/CD de GitHub Actions |
