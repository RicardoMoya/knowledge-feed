# Karpathy's 4 CLAUDE.md Rules Cut Mistakes by 30%. I Added 4 More to Further Cut It Down to Less Than 5%.
### 🇪🇸 Las 4 reglas CLAUDE.md de Karpathy reducen errores un 30%. Añadí 4 más para bajarlos al 5%.

**Autor:** Shashwat  
**Publicación:** Tech and AI Guild  
**URL:** [enlace](https://medium.com/tech-and-ai-guild/karpathys-4-claude-md-rules-cut-mistakes-by-30-i-added-4-more-to-further-cut-it-down-to-5-3f2f03cbc969)  
**Fecha:** 2026-05-13  
**Categoría:** ai-dev-tools  
**Fuente:** Medium  
**Tags:** `claude-md` `claude-code` `best-practices` `agentic-engineering` `token-budget` `multi-step-agents` `guardrails`

---

## Contexto y tesis principal

En enero de 2026, Andrej Karpathy disecó públicamente por qué Claude Code fallaba en codebases de producción: suposiciones silenciosas, sobre-ingeniería y daños colaterales. Un desarrollador empaqueó esas quejas en un CLAUDE.md de 4 reglas que se convirtió en el repo de GitHub de más rápido crecimiento del año. El problema: esas 4 reglas son el suelo, no el techo. En entornos de agentes autónomos multi-paso, el agente completa 3 pasos perfectamente, alucina en el 4.º y sobreescribe silenciosamente código que funcionaba. El artículo propone 4 reglas adicionales específicamente diseñadas para pipelines agénticos modernos, llevando la tasa de errores por debajo del 5%.

---

## Puntos clave

### 1. Las 4 reglas originales de Karpathy (el suelo)

Cubren ~40% de los modos de fallo en prompts simples de un solo turno:

1. **Think Before Coding** — Explicitar suposiciones. Pedir aclaración si hay incertidumbre. Proponer alternativas más simples.
2. **Simplicity First** — El mínimo código que resuelve el problema. Sin features especulativas ni abstracciones para uso único.
3. **Surgical Changes** — Tocar solo lo estrictamente necesario. No "mejorar" código adyacente, comentarios ni formato.
4. **Goal-Driven Execution** — Definir criterios de éxito e iterar hasta verificarlos. No seguir pasos ciegamente.

> ⚠️ **Por qué fallan hoy:** Estas reglas son completamente silenciosas sobre pipelines multi-paso. No dan al agente un presupuesto de tokens, no fuerzan checkpoints y asumen que el agente ya entiende el codebase circundante.

### 2. Las 4 nuevas reglas para orquestación agéntica (el techo)

**Regla 5 — Token budgets (no son opcionales)**  
Sin presupuesto, un agente en bucle puede consumir un dump de 50.000 tokens depurando el mismo error hasta perder el hilo. La regla establece: 4.000 tokens por tarea, 30.000 por sesión. Si se acerca al límite, el agente debe resumir y reiniciar. Hacer visible la brecha siempre es mejor que superar silenciosamente los límites de la API.

**Regla 6 — Leer antes de escribir**  
Las reglas de Karpathy dicen "no toques código adyacente" pero no dicen "entiende el código adyacente". Esto lleva al agente a escribir funciones duplicadas que ya existen 30 líneas más abajo. La regla: antes de añadir código a un fichero, el agente debe leer sus exports, llamadores inmediatos y utilidades compartidas.

**Regla 7 — Checkpoint tras cada paso significativo**  
Si un refactor de 6 pasos falla en el paso 4, el agente ejecutará felizmente los pasos 5 y 6 sobre el estado roto, arruinando toda la rama. La regla: tras cada paso significativo, el agente debe resumir qué fue verificado y qué queda. No puede continuar desde un estado que no es capaz de describir de vuelta al usuario.

**Regla 8 — Fail loud**  
Los fallos más caros se parecen exactamente a los éxitos. "Migración completada" es mentira si 30 registros de base de datos fueron saltados silenciosamente por violaciones de constraint. La regla: por defecto, el agente debe hacer visible la incertidumbre. Si algo fue omitido, o si un test pasó por razones incorrectas, debe alertar inmediatamente.

### 3. Por qué exactamente 8 reglas

A partir de cierta longitud, Claude deja de leer las reglas y simplemente pattern-matchea el hecho de que "existen reglas". Con exactamente 8 reglas de alto impacto en formato imperativo, la tasa de compliance se mantiene por encima del **75%** y la tasa de errores cae cerca del **5%**.

---

## El CLAUDE.md completo (listo para copiar)

```markdown
# CLAUDE.md — 8-Rule Architecture
Bias: caution over speed on non-trivial work.

## Rule 1 — Think Before Coding
State assumptions explicitly. Push back when simpler approach exists.

## Rule 2 — Simplicity First
Minimum code. No speculative features. No abstractions for single-use code.

## Rule 3 — Surgical Changes
Touch only what you must. Don't "improve" adjacent code or formatting.

## Rule 4 — Goal-Driven Execution
Define success criteria. Loop until verified.

## Rule 5 — Token budgets are not advisory
Per-task: 4,000 tokens. Per-session: 30,000 tokens.
If approaching budget, summarize and start fresh.

## Rule 6 — Read before you write
Before adding code, read exports, immediate callers, shared utilities.

## Rule 7 — Checkpoint after every significant step
Summarize what was done, what's verified, what's left.

## Rule 8 — Fail loud
"Completed" is wrong if anything was skipped silently.
Default to surfacing uncertainty, not hiding it.
```

---

## Datos relevantes

| Métrica | Valor |
|---|---|
| Reducción de errores con las 4 reglas originales | ~30% (cubren ~40% de modos de fallo) |
| Tasa de error con las 8 reglas | <5% |
| Tasa de compliance con ≤8 reglas | >75% |
| Token budget por tarea (recomendado) | 4.000 tokens |
| Token budget por sesión (recomendado) | 30.000 tokens |
| Longitud máxima recomendada de CLAUDE.md | <200 líneas |

---

## Conclusión

El artículo es una extensión práctica y directamente accionable del trabajo de Karpathy. Las 4 reglas originales funcionan para sesiones simples de un turno, pero los workflows agénticos multi-paso exponen sus límites: no hay presupuesto de tokens, no hay checkpoints y el agente no está obligado a entender el contexto antes de actuar. Las 4 reglas nuevas (presupuesto, read-before-write, checkpoint y fail-loud) cierran exactamente esos huecos. El resultado final —un CLAUDE.md de 8 reglas, ~50 líneas— es probablemente el artefacto de configuración con mejor ratio impacto/coste en Claude Code hoy.

## Tags
`claude-md` `claude-code` `best-practices` `agentic-engineering` `token-budget` `multi-step-agents` `guardrails`
