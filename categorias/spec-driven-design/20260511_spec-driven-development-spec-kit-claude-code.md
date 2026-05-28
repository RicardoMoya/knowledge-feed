# Spec-Driven Development with Spec Kit and Claude Code
### 🇪🇸 Desarrollo Dirigido por Especificaciones con Spec Kit y Claude Code

**Autor:** Saeed Zarinfam
**Publicación:** AI-Driven / VibeCodingPub
**URL:** [enlace](https://medium.com/vibecodingpub/spec-driven-development-with-spec-kit-and-claude-code-7e2957fd2c9b)
**Fecha:** 2026-05-27
**Categoría:** spec-driven-design
**Fuente:** Medium
**Tags:** `spec-driven-development` `spec-kit` `claude-code` `software-design` `tutorial` `best-practices` `sdlc` `agile`

---

## Contexto y tesis principal

Los agentes de IA están devolviendo los requisitos al centro del ciclo de desarrollo del software. El patrón habitual —spec escrita al inicio, enterrada bajo tickets de Jira a las pocas semanas, código convertido en la única fuente de verdad— tiene un fallo conocido a escala de equipo: el agente no tiene comprensión compartida de la intención del sistema, y cada desarrollador lo influye con prompts ligeramente distintos. El resultado es rápido, inconsistente y difícil de evolucionar.

**Spec-Driven Development (SDD)** invierte ese orden: la especificación —no el código— es el artefacto primario. El código es algo que se *genera* a partir de ella. El artículo explica el concepto, cómo reshapea el SDLC, y lo demuestra construyendo una API REST de reservas de salas con **Spec Kit** (GitHub, open-source) y **Claude Code**.

## Puntos clave

### 1. El ecosistema de tooling SDD

| Herramienta | Origen | Modelo |
|---|---|---|
| **Spec Kit** | GitHub | Open-source, CLI |
| **Kiro** | AWS | Producto propio |
| **Tessl** | Tessl | Producto propio |

Spec Kit es la opción que integra más limpiamente con los agentes ya existentes: Claude Code, Cursor, Copilot, Gemini y Codex.

### 2. Instalación y comandos disponibles
Instalación y configuración inicial:
```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
specify --version
specify init . --ai claude   # conecta plantillas con Claude Code
```

Tras ejecutar `specify init`, aparecen en Claude Code **8 slash commands**:

| Comando | Función |
|---|---|
| `/speckit.constitution` | Define los no-negociables de todo el proyecto |
| `/speckit.specify` | Genera spec estructurada desde descripción en lenguaje natural |
| `/speckit.plan` | Genera el plan de implementación desde la spec |
| `/speckit.tasks` | Convierte el plan en lista de tareas ordenada |
| `/speckit.implement` | Ejecuta una o varias tareas |
| `/speckit.clarify` | Señala requisitos ambiguos antes de planificar |
| `/speckit.analyze` | Cruza spec, plan y tasks para detectar inconsistencias |
| `/speckit.checklist` | Genera checklists de revisión para el revisor humano |

### 3. Diferencia clave: `CLAUDE.md` vs `constitution.md`
Ambos ficheros coexisten en la raíz del proyecto pero responden preguntas distintas:
- **`CLAUDE.md`** → descriptivo: *"el proyecto está estructurado así, los tests se lanzan con este comando"*
- **`constitution.md`** → prescriptivo: *"estas son las reglas no-negociables que toda feature debe respetar"*

Los comandos `/speckit.specify`, `.plan`, `.tasks` e `.implement` **no tienen equivalente nativo en Claude Code**; hay que instalar Spec Kit o escribir los prompts manualmente.

### 4. El flujo de 4 pasos en la práctica
El sistema de ejemplo es un servicio REST para gestionar reservas de salas de reuniones con **Spring Boot 4.0.x**, **Java 25**, **PostgreSQL 16+**, **JUnit 5** y **Testcontainers**.

**Paso 1 — Especificación (`/speckit.specify`):** Se describe el problema en lenguaje natural. El agente escribe el resultado en `specs/001-.../spec.md`. **No hay Spring Boot, ni JPA, ni verbos REST en este fichero** — solo el problema. También genera `checklists/requirements.md` con Quality Checklist en **3 secciones** (Content Quality, Requirement Completeness, Feature Readiness). El agente repara fallos en **hasta 3 iteraciones**.

**Paso 2 — Plan (`/speckit.plan`):** Se añaden las decisiones tecnológicas (Java 25, Spring Boot 4.0.x, PostgreSQL). El agente genera `plan.md` más ficheros de soporte: `data-model.md`, `contracts/openapi.yml` y `research.md`.

> 📊 Datos técnicos clave del plan generado: prevención de solapamientos con restricción `EXCLUDE USING GIST` sobre columna `tstzrange` a nivel PostgreSQL (no solo aplicación); bajo concurrencia, **exactamente 1 booking tiene éxito**, el resto recibe **HTTP 409**; objetivo: **95% de operaciones en menos de 2 segundos** bajo carga organizacional; virtual threads habilitados: `spring.threads.virtual.enabled: true`.

**Paso 3 — Tareas (`/speckit.tasks`):** Sin prompt adicional. El agente genera `tasks.md` con tareas agrupadas por User Story, marcadas con `[P]` si pueden ejecutarse en paralelo y con la ruta exacta del fichero a crear o modificar. Ejecutar `/speckit-analyze` en este punto cruza `spec.md`, `plan.md` y `tasks.md` entre sí y señala inconsistencias antes de que se conviertan en código.

**Paso 4 — Implementación (`/speckit.implement`):** Consejo crítico del autor: nunca ejecutar todas las tareas de golpe. Implementar **tarea a tarea** con **Plan Mode activado** (Shift+Tab en Claude Code). Plan Mode obliga al agente a escribir el plan fichero por fichero y esperar aprobación antes de tocar código.

### 5. Cómo SDD reorganiza el SDLC
Las 6 fases clásicas no desaparecen, pero cambian de foco:
- **Análisis** → produce especificaciones ejecutables; preguntas ambiguas se convierten en bugs detectados en **horas, no meses**
- **Diseño** → plan explícito con decisiones arquitectónicas en `constitution.md`
- **Implementación** → el agente escribe; el desarrollador revisa intención y verifica comportamiento
- **Testing** → los criterios de aceptación de la spec son los tests iniciales que genera el agente
- **Mantenimiento** → mayor beneficio a largo plazo: incorporar un nuevo ingeniero o sesión de agente no requiere hacer reverse-engineering del codebase

## Datos relevantes

| Métrica | Valor |
|---------|-------|
| Slash commands disponibles tras `specify init` | 8 |
| Iteraciones de reparación de checklist | hasta 3 |
| Tiempo objetivo operaciones REST | <2 segundos (p95) |
| Respuesta bajo concurrencia (doble booking) | HTTP 409 |
| Threshold operaciones en tiempo | 95% bajo 2s |

## Conceptos adicionales

- **Bug conocido en la versión actual de Spec Kit:** En `.specify/extensions.yml`, el hook `before_specify` tiene `optional: false`, lo que causa un error al ejecutar `/speckit-specify`. Fix: cambiar `optional: false` a `optional: true` antes de empezar.
- Para cambios futuros (p. ej., reservas recurrentes): actualizar `spec.md` primero, luego regenerar las partes afectadas de `plan.md`, `tasks.md` y los ficheros Java relevantes.
- Los mismos comandos `/speckit-*` producen los mismos artefactos independientemente del agente (Claude Code, Cursor, Copilot, Gemini), por lo que el trabajo es **agnóstico al agente**.

## Conclusión

SDD no es una bala de plata ni reemplaza el juicio de ingeniería: pone más peso en las partes que la IA no puede hacer — clarificar intención, elegir trade-offs y verificar que lo construido es lo que se necesitaba. **El agente maneja el tipeo; el desarrollador maneja el pensamiento.** Spec Kit añade valor como capa de disciplina: estructura de ficheros consistente, plantillas de prompt probadas, y `constitution.md` que cada feature debe respetar. Y dado que los mismos comandos producen los mismos artefactos independientemente del agente, el trabajo viaja con el desarrollador aunque cambie de herramienta.

## Tags
`spec-driven-development` `spec-kit` `claude-code` `software-design` `tutorial` `best-practices` `sdlc` `agile`
