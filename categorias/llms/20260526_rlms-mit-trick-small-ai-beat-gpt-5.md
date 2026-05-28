# RLMs: The MIT Trick That Makes a Small AI Beat GPT-5
### 🇪🇸 RLMs: El truco del MIT que hace que una IA pequeña supere a GPT-5

**Autor:** Sumit Pandey
**Publicación:** Towards Deep Learning
**URL:** [enlace](https://medium.com/towards-deep-learning/rlms-the-mit-trick-that-makes-a-small-ai-beat-gpt-5-668c7744cda7)
**Fecha:** 2026-05-27
**Categoría:** llms
**Fuente:** Medium
**Tags:** `rlms` `recursive-language-models` `mit` `context-window` `context-rot` `python-repl` `benchmarks` `research` `inference`

---

## Contexto y tesis principal

Los Recursive Language Models (RLMs) son una técnica de inferencia desarrollada por investigadores del MIT (Zhang, Kraska y Khattab, arXiv:2512.24601) que resuelve uno de los problemas más persistentes de los LLMs: el deterioro del rendimiento con contextos muy largos, conocido como "context rot". La idea central es radicalmente simple: en lugar de introducir el contexto gigante en el prompt del modelo, se almacena como variable Python en un entorno REPL y se deja al modelo escribir código para explorarlo.

> 📊 Resultado principal: **GPT-5-mini con RLM supera a GPT-5 puro en un 114%** en los benchmarks de contexto largo más exigentes, a coste comparable.

## Puntos clave

### 1. El problema que los RLMs resuelven
La carrera por ventanas de contexto más grandes (de 4K a 1M+ tokens) no ha eliminado el "context rot": pasado cierto punto, el modelo se vuelve más torpe, olvida información, alucina y da respuestas superficiales sobre cosas que respondía bien antes. El efecto "lost in the middle" (Liu et al., 2024) demuestra que los modelos pierden sistemáticamente información enterrada en el centro del contexto, incluso cuando técnicamente caben. Las dos soluciones habituales —compactación (resumen con pérdida de información) y RAG (retrieval que solo funciona bien para búsquedas semánticas simples)— son parches que no resuelven el problema de fondo.

### 2. Cómo funciona un RLM paso a paso
1. El contexto extenso se carga en un REPL Python como variable (no en el prompt del modelo)
2. El modelo raíz (depth 0) solo ve la query y tiene acceso al REPL
3. Para explorar los datos, escribe código: puede hacer slicing, regex, conteos, filtros
4. El REPL ejecuta el código y devuelve el output (truncado a un presupuesto de **~8.192 caracteres**), lo que obliga al modelo a ser estratégico
5. Cuando una porción del contexto necesita razonamiento —no procesamiento mecánico—, el modelo raíz puede invocar una sub-instancia sobre ese fragmento concreto
6. El resultado de la sub-llamada llega como variable Python al modelo raíz, **no como muro de texto en su contexto**

Esto mantiene el contexto del modelo raíz limpio aunque el contexto total sea masivo.

### 3. Por qué un modelo pequeño gana al grande
El truco es que la dificultad de estas tareas no reside en la inteligencia bruta, sino en no ahogarse. Un modelo modesto con subproblemas limpios y acceso a Python (que no cuenta mal 5.000 filas, a diferencia de un LLM estimando a ojo) supera a un modelo más potente intentando retener todo en su cabeza. Las llamadas son cortas (1-2 iteraciones vs. largas cadenas ReAct) porque el modelo escribe un programa completo en lugar de hacer una acción por turno, lo que también reduce el crecimiento de contexto por interacción.

### 4. Resultados empíricos y reproducciones independientes
> 📊 En el benchmark OOLONG (textos de **132K tokens**), RLM sobre GPT-5-mini **duplica los aciertos** de GPT-5 puro. En tareas de deep research sobre ~100K documentos, los RLMs no degradan rendimiento incluso **más allá de 10M tokens** de input. Prime Intellect reprodujo ganancias similares de eficiencia en tokens. minRLM reporta procesar contexto "ilimitado" con **~3,6 veces menos tokens** en más de **6.600 evaluaciones**.

El patrón es robusto entre distintas reimplementaciones independientes.

### 5. Advertencias importantes
- La recursión profunda puede volverse contraproducente: depth=2 o superior puede disparar el tiempo de ejecución de **~3,6 segundos a ~344 segundos** en algunas configuraciones
- Ejecutar código generado por un LLM en un REPL es una **superficie de ataque real** que requiere sandbox aislado
- RAG sigue siendo mejor para búsqueda semántica simple sobre corpus fijo con restricciones de latencia
- El ecosistema es joven (del blog de octubre 2025 a las primeras reimplementaciones en 2026)

## Datos relevantes

| Métrica | Valor |
|---------|-------|
| Mejora de GPT-5-mini con RLM vs. GPT-5 puro | +114% |
| Tamaño del benchmark OOLONG | 132K tokens |
| Presupuesto de output del REPL por iteración | ~8.192 caracteres |
| Reducción de tokens (minRLM) | ~3,6x |
| Evaluaciones de minRLM | 6.600+ |
| Latencia depth=1 | ~3,6 segundos |
| Latencia depth=2+ | ~344 segundos |
| Contexto máximo testado sin degradación | >10M tokens |

## Conceptos adicionales

- La diferencia filosófica con los tool-calling APIs clásicos: en RLMs, las herramientas son funciones Python en el namespace del REPL, sin JSON schemas, sin menú fijo. El modelo las compone libremente.
- Implementaciones open source disponibles: `alexzhang13/rlm`, `rlm-minimal`, `minRLM`, `rlm-code`.

## Conclusión

Los RLMs representan un cambio genuino en cómo los modelos interactúan con contextos masivos: de lectores pasivos a programadores activos. Para cualquier tarea agentica donde el modelo necesita razonar a través de un cuerpo grande e interconectado de información —un codebase completo, un historial de diffs, miles de registros que hay que computar—, dar al modelo un REPL y dejarle explorar produce resultados superiores a alimentarle de chunks. Comenzar con depth=1 y resistir la tentación de aumentar la recursión es el consejo práctico más importante para quienes quieran experimentar hoy.

## Tags
`rlms` `recursive-language-models` `mit` `context-window` `context-rot` `python-repl` `benchmarks` `research` `inference`
