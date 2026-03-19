# 🧪 Mes 01 — PromptLab

> **Sistema de ingeniería de prompts con versionado, catálogo y evaluación comparativa entre modelos LLM**

| Campo              | Detalle                                                     |
| ------------------ | ----------------------------------------------------------- |
| 📅 Mes             | Marzo 2026                                                  |
| 🏷️ Categoría       | Prompt Engineering & LLM Foundations                        |
| ⏱️ Tiempo estimado | ~40 horas (1 mes a tiempo parcial)                          |
| 📦 Repositorio     | [artur282/PromptLab](https://github.com/artur282/PromptLab) |
| 📊 Dificultad      | ⭐⭐ Intermedio                                             |
| 🎯 Competencia     | Prompt Engineering + Model Selection + Versionado           |

---

## 🎯 Caso de uso empresarial

**Problema:** En equipos de IA, los prompts se escriben ad-hoc, se comparten por Slack y no se puede saber qué versión está en producción ni comparar resultados entre modelos o iteraciones.

**Solución:** Un sistema centralizado de gestión de prompts que incluye:

- Catálogo versionado con nomenclatura (`prompt-name/v1.2.3`)
- Evaluación comparativa automatizada entre proveedores (OpenAI, Anthropic, Ollama/Llama)
- Métricas de calidad, coste y latencia por versión
- Historial de cambios (diffs) y rollback

**KPIs de impacto:**

- ↓ Tiempo de iteración de prompts (de días → horas)
- ↑ Calidad medible vs baseline (ROUGE, BERTScore, LLM-as-judge)
- ↓ Coste por interacción mediante selección informada de modelo

---

## 🏗️ Arquitectura

```
┌─────────────────────────────────────────────────────────┐
│                    PromptLab API (FastAPI)               │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │   Catálogo   │  │  Evaluador   │  │  Comparador  │  │
│  │  de Prompts  │  │  de Calidad  │  │  de Modelos  │  │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘  │
│         │                 │                  │          │
│  ┌──────▼─────────────────▼──────────────────▼───────┐  │
│  │              Prompt Registry (SQLite + Git)        │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
         │                                    │
   ┌─────▼─────┐                     ┌───────▼──────┐
   │  LLM Hub  │                     │  Langfuse    │
   │ OpenAI /  │                     │ Observab. &  │
   │ Anthropic │                     │ Tracing      │
   │ Ollama    │                     └──────────────┘
   └───────────┘
```

---

## ✨ Features

### Core

- [ ] CRUD de prompts con versionado semántico (`nombre/vmajor.minor.patch`)
- [ ] Plantillas parametrizadas con variables `{{variable}}`
- [ ] Soporte para tipos: system prompt, user prompt, few-shot templates, chain-of-thought
- [ ] Diff visual entre versiones de prompts
- [ ] Tags y categorías (por dominio: atención al cliente, extracción, resumen, etc.)

### Evaluación

- [ ] Runner de evaluación sobre golden dataset (JSON)
- [ ] Métricas automáticas: ROUGE, BERTScore, exactitud de extracción
- [ ] LLM-as-judge: evaluación de calidad con modelo evaluador externo
- [ ] Comparativa A/B entre v1 y v2 de un prompt
- [ ] Comparativa cross-model: mismo prompt en GPT-4o vs Claude vs Llama

### Observabilidad

- [ ] Integración Langfuse: tracing de cada llamada
- [ ] Dashboard: coste por prompt, latencia P50/P95, tasa de éxito
- [ ] Historial de evaluaciones por versión de prompt
- [ ] Alertas si calidad cae por debajo de umbral

### Infraestructura

- [ ] API REST con FastAPI + Swagger
- [ ] Almacenamiento SQLite + archivos YAML para prompts (versionables en Git)
- [ ] Docker Compose (app + Langfuse)
- [ ] CI/CD: evaluación automática en PR (GitHub Actions)

---

## 🛠️ Stack técnico

| Tecnología            | Propósito                           |
| --------------------- | ----------------------------------- |
| **Python 3.11+**      | Lenguaje principal                  |
| **FastAPI**           | API REST                            |
| **LangChain**         | Abstracción de LLMs y chains        |
| **OpenAI API**        | GPT-4o, GPT-4o-mini, text-embedding |
| **Anthropic API**     | Claude 3.5 Sonnet/Haiku             |
| **Ollama**            | Llama 3.2 local para comparativas   |
| **Langfuse**          | Observabilidad y tracing            |
| **SQLite**            | Almacenamiento de catálogo          |
| **ROUGE / BERTScore** | Métricas de evaluación NLP          |
| **pytest**            | Testing unitario e integración      |
| **Docker Compose**    | Infraestructura reproducible        |

---

## 📁 Estructura del proyecto

```bash
promptlab/
├── app/
│   ├── main.py                    # Entry point FastAPI
│   ├── config.py                  # Settings (API keys, modelos)
│   ├── routers/
│   │   ├── prompts.py             # CRUD catálogo
│   │   ├── evaluate.py            # Endpoints evaluación
│   │   └── compare.py             # Comparativa modelos
│   ├── services/
│   │   ├── prompt_registry.py     # Versionado y storage
│   │   ├── evaluator.py           # Pipeline evaluación
│   │   └── llm_hub.py             # Abstracción multi-proveedor
│   └── models/
│       ├── prompt.py              # Schemas Pydantic
│       └── evaluation.py
├── prompts/                       # Catálogo YAML versionado
│   ├── customer-support/
│   │   ├── v1.0.0.yaml
│   │   └── v1.1.0.yaml
│   └── data-extraction/
│       └── v1.0.0.yaml
├── evals/
│   ├── golden_sets/               # Datasets de evaluación
│   │   └── customer-support.json
│   └── results/                   # Resultados históricos
├── tests/
│   ├── test_prompt_registry.py
│   └── test_evaluator.py
├── docker-compose.yml
├── Makefile
├── pyproject.toml
└── .env.example
```

---

## 📡 API Endpoints

```http
# Catálogo de prompts
GET    /api/v1/prompts                    # Listar todos los prompts
POST   /api/v1/prompts                    # Crear nuevo prompt (inicia en v1.0.0)
GET    /api/v1/prompts/{name}/versions    # Listar versiones de un prompt
GET    /api/v1/prompts/{name}/{version}   # Obtener versión específica
POST   /api/v1/prompts/{name}/versions    # Crear nueva versión (patch/minor/major)
GET    /api/v1/prompts/{name}/diff        # Diff entre dos versiones

# Evaluación
POST   /api/v1/evaluate/run              # Ejecutar evaluación sobre dataset
GET    /api/v1/evaluate/results          # Historial de evaluaciones
POST   /api/v1/evaluate/compare          # Comparar dos versiones (A/B)

# Comparativa de modelos
POST   /api/v1/compare/models            # Mismo prompt en múltiples modelos
GET    /api/v1/compare/leaderboard       # Ranking de modelos por métrica

GET    /docs                             # Swagger UI
```

---

## 📊 Formato del catálogo de prompts (YAML)

```yaml
# prompts/customer-support/v1.1.0.yaml
name: customer-support
version: 1.1.0
description: "Agente de soporte al cliente para sector bancario"
type: system_prompt
tags: [customer-service, banking, spanish]
author: artur282
created_at: 2026-03-15
changelog: "v1.1.0: Añadido manejo explícito de casos de escalación"

template: |
  Eres un asistente de soporte bancario profesional y empático.
  Siempre respondes en español y con un tono {{tone}}.

  Políticas clave:
  - Nunca compartas información de cuenta sin verificar identidad
  - Escala a agente humano si el cliente menciona: fraude, urgencia médica, amenaza
  - Máximo {{max_steps}} pasos para resolver una consulta

  Historial de conversación disponible: {{has_history}}

variables:
  - name: tone
    type: string
    default: "formal y cordial"
    options: [formal y cordial, empático, técnico]
  - name: max_steps
    type: integer
    default: 3
  - name: has_history
    type: boolean
    default: true

evaluation:
  golden_set: evals/golden_sets/customer-support.json
  metrics: [rouge_l, bertscore, task_completion_rate]
  min_quality_threshold: 0.75
```

---

## 🗓️ Plan mensual

### Semana 1 — Investigación y diseño

- Investigar mejores prácticas de prompt versioning (LangSmith, Langfuse prompt hub)
- Diseñar esquema YAML del catálogo
- Definir dataset de evaluación (golden set: 30-50 casos con respuestas esperadas)
- Arquitectura de la API y esquemas Pydantic

### Semana 2 — Implementación core

- CRUD de catálogo de prompts con versionado semántico
- Integración con OpenAI y Anthropic (LangChain abstraction)
- Runner de evaluación con métricas ROUGE y BERTScore
- Langfuse tracing básico

### Semana 3 — Evaluación y comparativas

- LLM-as-judge: evaluador externo con structured output
- Comparativa multi-modelo en mismo golden set
- Dashboard de resultados (tabla HTML desde FastAPI)
- A/B test estadístico entre versiones

### Semana 4 — Polish, CI/CD y documentación

- GitHub Actions: evaluación automática en cada PR que cambie un prompt YAML
- Docker Compose con Langfuse incluido
- Playbook de uso para equipos no técnicos
- README con arquitectura, decisiones y resultados reales del benchmark

---

## 🧪 Golden Dataset (ejemplo)

```json
[
  {
    "id": "cs-001",
    "input": "Mi tarjeta fue rechazada en el supermercado, ¿qué hago?",
    "expected_actions": [
      "verificar_saldo",
      "verificar_bloqueo",
      "ofrecer_solución"
    ],
    "expected_tone": "empático",
    "reference_answer": "Entiendo tu preocupación. Lo primero que debemos verificar es el saldo disponible y si la tarjeta está activa. ¿Puedes confirmar si has recibido alguna notificación de bloqueo?",
    "metadata": {
      "category": "tarjetas",
      "escalation_required": false,
      "complexity": "low"
    }
  }
]
```

---

## ✅ Definición de "hecho"

- [ ] API funcional con CRUD de prompts versionados
- [ ] Al menos 2 dominios en el catálogo con 3+ versiones cada uno
- [ ] Evaluación comparativa GPT-4o vs Claude 3.5 Sonnet en golden set
- [ ] Langfuse dashboard activo con trazas y métricas de coste
- [ ] CI/CD: GitHub Actions ejecuta evaluación en cada PR
- [ ] Docker Compose levanta todo con `make up`
- [ ] Playbook de 1 página para usar el sistema
- [ ] README con resultados reales del benchmark y decisiones técnicas

---

## 💼 Lo que demuestra al reclutador

| Habilidad                 | Evidencia                                                         |
| ------------------------- | ----------------------------------------------------------------- |
| Prompt Engineering        | Catálogo con plantillas parametrizadas, few-shot, CoT versionados |
| Versionado sistemático    | Semver + YAML + diffs + changelog en Git                          |
| Evaluación de LLMs        | Golden sets, ROUGE, BERTScore, LLM-as-judge                       |
| Selección de proveedor IA | Benchmark objetivo GPT-4o vs Claude vs Llama con métricas         |
| Observabilidad            | Langfuse: trazas, coste, latencia por versión de prompt           |
| Python + FastAPI          | API REST bien estructurada con tests                              |
| MLOps básico              | CI/CD que evalúa automáticamente cada cambio de prompt            |
