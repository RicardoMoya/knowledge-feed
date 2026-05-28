# Spec-Driven Development with Spec Kit and Claude Code

**Autor:** Saeed Zarinfam
**Publicación:** AI-Driven / VibeCodingPub
**URL:** [enlace](https://medium.com/vibecodingpub/spec-driven-development-with-spec-kit-and-claude-code-7e2957fd2c9b)
**Fecha de resumen:** 2026-05-27
**Categoría:** spec-driven-design

---

## Contexto y tesis principal

**Spec-Driven Development (SDD)** invierte el orden habitual: la especificación —no el código— es el artefacto primario. Demostrado construyendo una API REST de reservas de salas con **Spec Kit** (GitHub, open-source) y **Claude Code**.

## 3 competidores de tooling SDD

| Herramienta | Origen | Modelo |
|---|---|---|
| **Spec Kit** | GitHub | Open-source, CLI |
| **Kiro** | AWS | Producto propio |
| **Tessl** | Tessl | Producto propio |

## Instalación

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
specify init . --ai claude
```

Genera **5 ficheros** y **8 slash commands** en Claude Code:

**5 principales:** `/speckit.constitution`, `/speckit.specify`, `/speckit.plan`, `/speckit.tasks`, `/speckit.implement`

**3 auxiliares:** `/speckit.clarify`, `/speckit.analyze`, `/speckit.checklist`

## `CLAUDE.md` vs `constitution.md`

- **`CLAUDE.md`** → descriptivo (cómo está estructurado el proyecto)
- **`constitution.md`** → prescriptivo (reglas no-negociables de toda feature)

## El flujo de 4 pasos (API Spring Boot 4.0.x / Java 25 / PostgreSQL 16+)

1. **Spec** — problema sin tecnología. Quality Checklist con **3 secciones**, reparación en **hasta 3 iteraciones**.
2. **Plan** — tecnología + decisiones. Prevención solapamientos: `EXCLUDE USING GIST` en PostgreSQL. Conflicto concurrente → **HTTP 409**. Goal: **95% ops < 2 segundos**.
3. **Tasks** — lista ordenada con `[P]` (paralizable) y rutas exactas de ficheros.
4. **Implement** — **tarea a tarea** con Plan Mode activado (Shift+Tab). Nunca arreglar en código lo que falta en la spec.

## Bug conocido

En `.specify/extensions.yml`: cambiar `optional: false` a `optional: true` antes de empezar.

## Conclusión

El agente maneja el tipeo; el desarrollador maneja el pensamiento. Los comandos `/speckit-*` son **agnósticos al agente** — si el tooling cambia, las specs viajan con él.
