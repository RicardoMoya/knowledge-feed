# Mastering CLAUDE.md: 10 Proven Ways to Reduce Claude Code Drift
### 🇪🇸 Dominando CLAUDE.md: 10 formas probadas de reducir el drift de Claude Code

**Autor:** Mouez Yazidi
**Publicación:** Towards AI
**URL:** [enlace](https://medium.com/towards-artificial-intelligence/mastering-claude-md-10-proven-ways-to-reduce-claude-code-drift-9ec878786246)
**Fecha:** 2026-05-27
**Categoría:** ai-dev-tools
**Fuente:** Medium
**Tags:** `claude-code` `claude-md` `drift-prevention` `context-management` `best-practices` `production` `rag` `ai-dev-tools`

---

## Contexto y tesis principal

El problema más común con Claude Code no es el modelo, es el contexto. Cuando los equipos tratan `CLAUDE.md` como un archivo README o un vertedero de notas, el modelo queda sin señales claras y recurre a sus valores predeterminados de entrenamiento: código genérico, patrones comunes y comportamiento impredecible. El autor, con **más de un año** implementando agentes de IA en producción con Claude Code, propone tratar `CLAUDE.md` no como documentación sino como una **API de comportamiento**: un contrato determinista que define exactamente cómo debe razonar y ejecutar el modelo en cada sesión.

El artículo se ilustra con un sistema real llamado **SynapseRAG** — una plataforma RAG empresarial con requisitos concretos: latencia p95 inferior a **800 ms**, salidas JSON deterministas, cero citas inventadas y orquestación multiagente con LangGraph, Python 3.11, FastAPI, Pydantic v2, Weaviate y Redis.

## Puntos clave

### 1. Máximo 200 líneas — la atención no es infinita
La ventana de contexto de un Transformer no es almacenamiento: es **memoria de trabajo**. Cuando un fichero supera las **200 líneas**, las restricciones críticas quedan enterradas.

> 📊 Caso real documentado: un equipo perdió horas depurando comportamiento aleatorio porque el modelo seguía una nota obsoleta sobre datos simulados oculta en la **línea 187**. Regla de mantenimiento: **revisar mensualmente y eliminar ~el 30% de las reglas**. Si una regla no se ha activado en semanas, se mueve a un fichero de referencia o se elimina.

### 2. Las primeras 30 líneas establecen el marco
Los modelos Transformer asignan **peso posicional**: los tokens iniciales crean el filtro cognitivo para todo lo que sigue. Si el fichero comienza con la estructura de carpetas, Claude optimizará organización. Si comienza con restricciones de seguridad, optimizará seguridad.

> 📊 Las primeras **30 líneas** deben contener exclusivamente: identidad del sistema, principios innegociables y prioridades. El resto se interpreta a través de ese filtro.

### 3. Reglas estrictas vs. preferencias — no mezclar señales
Cuando restricciones y preferencias coexisten en el mismo bloque, el modelo las convierte en una distribución de probabilidades. El resultado: comportamiento inconsistente e irreproducible. Separación recomendada:
- **Reglas estrictas:** usan lenguaje absoluto — `never`, `always`, `must`
- **Preferencias:** usan lenguaje orientativo — `prefer`, `avoid`, `lean toward`

Nunca usar `should` o `try to` en rutas críticas.

### 4. Los antipatrones son más poderosos que las instrucciones positivas
Los datos de entrenamiento del modelo están repletos de código genérico y patrones por defecto. Para sobrescribirlos, hay que documentar explícitamente **qué no hacer**. Cada antipatrón que aparece en el fichero suprime activamente un comportamiento que el modelo generaría por defecto.

Ejemplo: `Never use naive text splitting for code documentation; respect semantic boundaries` — sin esta línea, el modelo usará fragmentación naïve porque es el patrón más frecuente en su entrenamiento.

### 5. Definir el éxito, no solo el cumplimiento
Los LLMs son motores orientados a objetivos. Sin criterios de éxito explícitos, seguirán las restricciones por el camino de menor resistencia. Los criterios de éxito deben ser **observables y testeables**. Ejemplos de SynapseRAG:
- Retornar **100%** de citas verificables vinculadas a IDs de documentos fuente
- Degradar limpiamente a `{"status": "insufficient_context"}` cuando la confianza de recuperación sea **< 0,75**
- Mantener estructura JSON determinista en todos los chunks de streaming
- Pasar checks de alucinación en CI con **cero** citas falso-positivos

> 📊 Si un criterio no puede expresarse como una aserción concreta, no es lo suficientemente preciso.

### 6. Divulgación progresiva — no obligarle a memorizarlo todo
`CLAUDE.md` debe funcionar como **capa de enrutamiento**, no como enciclopedia. Se mantienen las reglas globales y se apunta a ficheros especializados solo cuando la tarea lo requiere. Esto reduce el consumo de tokens y crea aislamiento cognitivo: cuando Claude trabaja en recuperación, no debe estar parseando reglas de rate-limit de la API.

Patrón de enrutamiento de contexto:
```
When building agent nodes → @agents/node-patterns.md
For prompt injection defense → @security/prompt-sanitization-rules.md
```

### 7. CLAUDE.md por subdirectorio — que el filesystem haga el trabajo
En monorepos, `CLAUDE.md` respeta la **proximidad**: un fichero en `/agents/` se superpone al de la raíz. El más cercano gana. Ejemplo de estructura en SynapseRAG:
- `/CLAUDE.md` → Stack global, reglas de seguridad, criterios de éxito
- `/agents/CLAUDE.md` → Gestión de estado LangGraph, enrutamiento de nodos
- `/retrieval/CLAUDE.md` → Estrategias de chunking, configuraciones de embeddings
- `/api/CLAUDE.md` → Validación FastAPI, formatos de streaming, rate limits

**Regla de conflicto:** cualquier contradicción entre fichero raíz y subfichero provoca comportamiento inconsistente y debe resolverse antes de hacer commit.

### 8. Enseñarle a fallar — el protocolo de fallback
La mayoría de alucinaciones no son maliciosas: son confianza sin límites. Cuando la recuperación devuelve chunks de baja similitud, el modelo fabrica citas para satisfacer el prompt. Umbrales de SynapseRAG:

> 📊 Similitud top-k **< 0,65** → activar agente de reformulación antes de síntesis. Confianza ambigua → retornar `{"status": "insufficient_context", "missing": [...]}`. Nunca fabricar IDs de documentos o hashes de citas.

### 9. Cambio de rol — activar el camino de razonamiento correcto
Los LLMs son una colección de capacidades latentes. El nombre de un rol activa una ruta de razonamiento específica. Recomendación: **máximo 3-4 roles** vinculados a disciplinas de ingeniería reales. Roles de SynapseRAG:
- Lógica de recuperación → **Search Engineer**: optimizar mezcla BM25/vector
- Grafos de agentes → **Systems Architect**: minimizar state bloat
- Tests de evaluación → **QA Auditor**: detectar alucinaciones y verificar latencia
- Endpoints API → **Backend Engineer**: validación de esquemas y rate limits

### 10. Versionarlo como una API — el context rot es real
Las reglas obsoletas no solo desperdician tokens: **desorientan activamente al modelo**.

> 📊 Caso real: pipelines rotos porque Claude seguía una estrategia de chunking deprecada eliminada **3 sprints antes**. Práctica recomendada: changelog explícito + sección `@deprecated` con migraciones señaladas. Implementar en CI: check que falle cuando aparecen en el código generado patrones marcados como `@deprecated`.

## Datos relevantes

| Patrón | Límite / Referencia |
|--------|---------------------|
| Longitud máxima recomendada | 200 líneas |
| Líneas para marco inicial | 30 líneas |
| Reglas a eliminar mensualmente | ~30% de las reglas inactivas |
| Umbral fallback SynapseRAG | similitud top-k < 0,65 |
| Umbral insufficient_context | confianza < 0,75 |
| Latencia p95 objetivo (SynapseRAG) | < 800 ms |
| Roles máximos recomendados | 3-4 |

## Conclusión

El cambio conceptual central del artículo: dejar de escribir prompts y empezar a diseñar contratos. `CLAUDE.md` es la aproximación más cercana a una API basada en comportamiento disponible en Claude Code. Un fichero bien construido —corto, con lenguaje absoluto en rutas críticas, criterios de éxito medibles, fallbacks explícitos y versionado— transforma a Claude de un sistema que adivina en uno que ejecuta con previsibilidad. La inversión en este fichero se amortiza en cada sesión posterior.

## Tags
`claude-code` `claude-md` `drift-prevention` `context-management` `best-practices` `production` `rag` `ai-dev-tools`
