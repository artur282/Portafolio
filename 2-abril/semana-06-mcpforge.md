# 🔧 Semana 06 — MCPForge

> **Servidor MCP personalizado que expone herramientas como servicio para IA**

| Campo              | Detalle                  |
| ------------------ | ------------------------ |
| 📅 Fechas          | 11-12 de abril 2026      |
| 🏷️ Categoría       | IA/ML & GenAI            |
| ⏱️ Tiempo estimado | 10-12 horas              |
| 📊 Dificultad      | ⭐⭐⭐⭐ Intermedio-alto |

---

## 🎯 Descripción

MCPForge es un servidor Model Context Protocol (MCP) que expone un conjunto de herramientas útiles para asistentes de IA. El proyecto implementa el protocolo MCP desde cero (usando el SDK oficial), creando tools, resources y prompts que cualquier cliente MCP compatible puede consumir.

El proyecto aplica **Layered Architecture** para separar la lógica de herramientas del transporte del protocolo, utiliza **TDD** para garantizar el correcto funcionamiento de cada tool, y emite **eventos de auditoría** (`ToolExecuted`) para trazabilidad. Documenta la arquitectura con **diagramas UML de clases** que modelan la jerarquía de tools/resources/prompts.

---

## 🏗️ Arquitectura (Layered Architecture)

```
┌──────────────────────────────────┐
│     Cliente MCP (Claude, etc.)    │
└───────────────┬──────────────────┘
                │ (stdio / SSE)
       ┌────────▼────────┐
       │  Transport Layer │  ← Controllers
       │  (stdio / SSE)   │
       ├──────────────────┤
       │  Service Layer    │  ← Tool orchestration
       │  (ToolService,    │
       │   ResourceService)│
       ├──────────────────┤
       │  Repository Layer │  ← Data access
       │  (DB, Files, APIs)│
       ├──────────────────┤
       │  Events Layer     │  ← Auditoría
       │  ToolExecuted,    │
       │  ResourceAccessed │
       └──────────────────┘
```

---

## ✨ Features

### Tools (Herramientas)

- [ ] `query_database` — Ejecutar consultas SQL de solo lectura
- [ ] `search_files` — Buscar archivos por nombre o contenido
- [ ] `call_api` — Hacer llamadas HTTP a APIs externas
- [ ] `process_text` — Transformaciones de texto (resumen, traducción, formato)
- [ ] `generate_report` — Generar reportes en Markdown

### Resources (Recursos)

- [ ] Exposición de archivos de configuración
- [ ] Documentación accesible como recurso
- [ ] Schema de base de datos como recurso

### Prompts

- [ ] Template de análisis de código
- [ ] Template de resumen de documentos
- [ ] Templates personalizables

### Eventos de Auditoría (Event-Driven)

- [ ] `ToolExecuted` — Log de cada invocación de herramienta (input, output, duración)
- [ ] `ResourceAccessed` — Registro de accesos a recursos
- [ ] Listener de auditoría que persiste logs estructurados

### Infraestructura

- [ ] Transporte stdio y SSE
- [ ] Logging estructurado con TraceID
- [ ] Manejo de errores del protocolo
- [ ] Configuración por archivo
- [ ] Docker para distribución

---

## 🛠️ Stack técnico

| Tecnología         | Propósito                    |
| ------------------ | ---------------------------- |
| **MCP Python SDK** | Implementación del protocolo |
| **Python 3.11+**   | Lenguaje base                |
| **SQLite**         | Base de datos demo           |
| **httpx**          | Llamadas HTTP async          |
| **Docker**         | Containerización             |
| **pytest**         | Testing (TDD)                |
| **Pydantic**       | Validación de inputs (DTOs)  |

---

## 📁 Estructura del proyecto (Layered Architecture)

```text
mcpforge/
├── docs/
│   └── uml/
│       ├── class-diagram.puml       # Clases: Tool, Resource, Prompt hierarchy
│       └── sequence-tool-exec.puml  # Secuencia: invocación de herramienta
├── server/
│   ├── __init__.py
│   ├── main.py                      # Entry point del servidor MCP
│   ├── controllers/
│   │   └── transport.py             # Capa de transporte (stdio/SSE)
│   ├── services/
│   │   ├── tool_service.py          # Orquestación de herramientas
│   │   └── resource_service.py      # Gestión de recursos
│   ├── tools/
│   │   ├── database.py              # query_database tool
│   │   ├── files.py                 # search_files tool
│   │   ├── api.py                   # call_api tool
│   │   ├── text.py                  # process_text tool
│   │   └── reports.py               # generate_report tool
│   ├── resources/
│   │   ├── config.py                # Recursos de configuración
│   │   └── docs.py                  # Recursos de documentación
│   ├── prompts/
│   │   └── templates.py             # Templates de prompts
│   ├── events/
│   │   ├── publishers.py            # ToolExecuted, ResourceAccessed
│   │   └── listeners.py             # Auditoría y logging
│   └── utils/
│       ├── logger.py
│       └── validators.py
├── tests/
│   ├── unit/
│   │   └── test_tools.py
│   ├── integration/
│   │   └── test_server.py
│   └── conftest.py
├── config/
│   └── server.toml
├── docker-compose.yml
├── Dockerfile
├── pyproject.toml
└── README.md
```

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                  |
| -------------- | ---------------------------------------------------------- |
| 🌅 9:00-10:00  | Diseño UML (diagrama de clases de tools/resources/prompts) |
| 🌅 10:00-10:30 | TDD: escribir tests para query_database y search_files     |
| 🌅 10:30-12:00 | Servidor MCP básico con Layered Architecture               |
| 🌞 12:00-13:00 | Tool: query_database con SQLite (Service + Repository)     |
| 🌞 14:00-16:00 | Tools: search_files, call_api (cada uno con tests)         |
| 🌆 16:00-18:00 | Tools: process_text, generate_report + eventos ToolExecuted|

### Domingo

| Hora           | Actividad                                         |
| -------------- | ------------------------------------------------- |
| 🌅 9:00-10:30  | Resources: config y docs + evento ResourceAccessed|
| 🌅 10:30-12:00 | Prompts: templates                                |
| 🌞 13:00-14:30 | Transporte SSE + testing de integración           |
| 🌞 14:30-16:00 | Docker + configuración                            |
| 🌆 16:00-17:00 | README, diagramas UML finales y docs de cada tool |

---

## ✅ Definición de "hecho"

- [ ] TDD: tests escritos primero para cada tool
- [ ] Layered Architecture: Transport → Service → Repository
- [ ] Servidor MCP funcional con al menos 4 tools
- [ ] Eventos de auditoría: ToolExecuted publicado y consumido
- [ ] Diagrama de clases UML documentado
- [ ] Al menos 2 resources expuestos
- [ ] Al menos 2 prompt templates
- [ ] Funciona con cliente MCP real
- [ ] Docker funcional
- [ ] README con instrucciones de integración
- [ ] GitFlow: ramas feature/*, develop, main

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                              |
| ---------------------- | -------------------------------------- |
| Layered Architecture   | Transport → Service → Repository       |
| TDD                    | Tests escritos antes del código        |
| Event-Driven           | Eventos de auditoría ToolExecuted      |
| UML / Documentación    | Diagrama de clases de herramientas     |
| MCP / IA infra         | Implementación del protocolo desde SDK |
| Python avanzado        | Async, protocolos, SDK                 |
| Innovación             | Tecnología de vanguardia               |
