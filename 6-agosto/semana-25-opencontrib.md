# 🤝 Semana 25 — OpenContrib

> **Contribuciones Open Source con Code Review, documentación técnica y TDD**

| Campo              | Detalle                 |
| ------------------ | ----------------------- |
| 📅 Fechas          | 22-23 de agosto 2026    |
| 🏷️ Categoría       | Open Source & Community |
| ⏱️ Tiempo estimado | 10-12 horas             |
| 📊 Dificultad      | ⭐⭐⭐ Variable         |

---

## 🎯 Descripción

Esta semana no se trata de construir un proyecto propio desde cero, sino de demostrar la capacidad de leer, entender y mejorar bases de código ajenas. El objetivo es realizar al menos una contribución significativa (PR merged o approved) a una librería o herramienta utilizada durante el portafolio.

**Enfoque especial en Code Review y documentación técnica** — soft skills clave de la propuesta laboral. Incluye participar en reviews de PRs de otros contribuidores, escribir documentación técnica profesional, y buscar issues en repos de **FastAPI, LangChain, pytest** (herramientas usadas en el portafolio).

---

## ✨ Estrategia

### 1. Identificación (Pre-fin de semana)

- Buscar issues etiquetados como `good first issue`, `documentation`, `help wanted`
- Candidatos ideales: **FastAPI plugins, LangChain, pytest plugins, Pydantic**
- Revisar repos de herramientas usadas en el portafolio

### 2. Ejecución (TDD)

- Forkear y clonar el repositorio
- Reproducir el bug o entender la feature request
- **TDD: Escribir tests que fallen primero** (Rojo)
- Implementar la solución (Verde)
- Refactorizar (Refactor)
- Pasar todos los tests existentes y los nuevos
- Actualizar documentación si es necesario

### 3. Code Review (Soft Skill clave)

- **Participar en reviews de PRs de otros** contribuidores del mismo repo
- Dar feedback técnico constructivo, sin egos
- Escribir un **PR Description profesional**: título claro, contexto, capturas
- Ser receptivo al feedback de los maintainers
- Documentar aprendizajes sobre convenciones del proyecto

### 4. Documentación Técnica

- Diagrama de secuencia UML del fix/feature implementada
- Guía de onboarding del módulo contribuido
- Documentación del "Por qué" y el "Cómo" en el PR

---

## 🛠️ Herramientas

| Tecnología              | Propósito                                         |
| ----------------------- | ------------------------------------------------- |
| **Git & GitHub**        | Flujo de contribución (Fork, Branch, PR)          |
| **Pytest / Jest**       | Ejecución de suites de tests (TDD)                |
| **Virtualenv / Docker** | Aislar entornos de desarrollo ajenos              |
| **Markdown**            | Documentación del PR                              |
| **PlantUML / Mermaid**  | Diagramas técnicos del fix/feature                |

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                                                       |
| -------------- | ------------------------------------------------------------------------------- |
| 🌅 9:00-11:00  | Selección del issue + setup del entorno local del proyecto                     |
| 🌅 11:00-13:00 | "Arqueología de código": leer y entender el módulo afectado                    |
| 🌞 13:00-14:00 | TDD: escribir test que falla reproduciendo el problema                         |
| 🌞 14:00-16:00 | Implementar el fix (Verde) + Refactorizar                                      |
| 🌆 16:00-18:00 | Verificar no regresiones + **revisar 1-2 PRs de otros** (Code Review)          |

### Domingo

| Hora           | Actividad                                                                            |
| -------------- | ------------------------------------------------------------------------------------ |
| 🌅 9:00-11:00  | Pulir código + documentación técnica (diagrama UML del fix)                         |
| 🌅 11:00-12:30 | Preparar PR: título claro, descripción detallada, screenshots                        |
| 🌞 13:00-14:30 | **Code Review**: revisar más PRs + dar feedback constructivo                         |
| 🌞 14:30-16:00 | Buscar un segundo issue pequeño en otro repo                                         |
| 🌆 16:00-17:00 | Documentar la experiencia en `CONTRIBUTION_LOG.md`                                   |

---

## ✅ Definición de "hecho"

- [ ] TDD aplicado: test que falla → fix → refactor
- [ ] Al menos 1 Pull Request enviado a un repositorio público activo
- [ ] Al menos 2 Code Reviews a PRs de otros contribuidores
- [ ] PR con documentación técnica profesional (diagrama UML incluido)
- [ ] El PR pasa los checks automáticos (CI) del repositorio
- [ ] `CONTRIBUTION_LOG.md` con links, aprendizajes y screenshots de reviews

---

## 💼 Lo que demuestra al reclutador

| Habilidad              | Evidencia                                   |
| ---------------------- | ------------------------------------------- |
| **Code Review**        | Reviews constructivos a PRs de otros        |
| **Documentación**      | PR description profesional, diagramas       |
| **TDD**                | Test primero en el fix del issue             |
| Trabajo en equipo      | Interacción con normas de código ajenas     |
| Code Reading           | Capacidad de navegar codebases desconocidos |
| Proactividad           | Mejora proactiva del ecosistema             |
