# 🏆 AutoPlatform

## 1. Visión general
AutoPlatform funciona como un proyecto integrador (capstone). Su objetivo es unificar conceptos de APIs construyendo un hub de automatización (tipo Zapier simplificado) con ayuda de IA. Al mismo tiempo cierra el ciclo de ingeniería exponiendo estos servicios en un Marketplace, levantando un Portfolio profesional, y publicando la primera contribución Open Source del roadmap.

## 2. Tecnologías principales
* **Backend Core**: FastAPI, PostgreSQL, LangChain
* **Frontend y Portfolio**: React, Next.js (opcional), TailwindCSS, Vercel
* **Flujos**: Estrategia de patrones de diseño (GoF Strategy / Command patterns)
* **DevOps**: Docker, Github Actions para despliegue
* **Comunidad**: Git avanzado, Markdown, OSS

## 3. Arquitectura
* **AutomateAI (Engine)**: Un motor backend donde el usuario define workflows (Ej: Si llega email -> clasificar con LLM -> guardar en DB).
* **APIMarket**: Frontend React que sirve como directorio documentado de todos los endpoints de AutomateAI, de forma auto-generada estilo Swagger pero customizado.
* **PortfolioOS**: Aplicación frontend estática y muy pulida visualmente que consolida métricas, repositorios y descripciones de todos los proyectos de los 6 meses.
* **OpenContrib & Retrospectiva**: Fases de documentación estricta para liberar código, hacer un Pull Request a una librería third-party, y documentar lecciones aprendidas (ADRs consolidados).

## 4. Requerimientos / Features Clave
1. **Motor de Reglas IA**: Procesador de eventos con un LLM en medio (decission node).
2. **Dashboard de Portfolio**: Listado 3D o interactivo de proyectos, integrando el feed de GitHub.
3. **Marketplace UI**: Portal de desarrolladores interactivo.
4. **Contribución Real OSS**: Identificar un "good first issue" en un proyecto como FastAPI o LangChain y hacer un PR siguiendo guías de contribución.
5. **Reporte Final**: Compilación Markdown + Mermaid con topologías completas enviadas a un documento de retrospectiva.

## 5. Endpoints Principales
* `POST /workflows/create` -> Define un DAG de nodos para procesar acciones.
* `POST /workflows/trigger/{id}` -> Dispara una automatización vía webhook.
