# Build a Personal Knowledge Base With Claude Code
### 🇪🇸 Construye una base de conocimiento personal con Claude Code

**Autor:** Ketan Damle  
**Publicación:** Medium  
**URL:** [enlace](https://medium.com/@koriigami/build-a-personal-knowledge-base-with-claude-code-25d215b61822)  
**Fecha:** 2026-05-23  
**Categoría:** ai-dev-tools  
**Fuente:** Medium  
**Tags:** `knowledge-management` `llm-wiki` `rag-alternative` `karpathy` `claude-code` `obsidian` `personal-knowledge-base` `markdown`

---

## Contexto y tesis principal

En abril de 2026, Andrej Karpathy (cofundador de OpenAI, ex-director de IA en Tesla) publicó un Gist llamado "LLM Wiki" que acumuló más de 5.000 stars en cuestión de días. La idea es deceptivamente simple: en lugar de construir un pipeline RAG con embeddings, búsqueda vectorial e infraestructura de recuperación, se deja que Claude Code mantenga una colección de ficheros markdown estructurados que acumulan conocimiento con cada nueva fuente ingerida. Karpathy lo resumió con honestidad: "Pensaba que necesitaba RAG sofisticado, pero el LLM ha sido bastante bueno manteniendo los ficheros de índice automáticamente."

## Puntos clave

### 1. Por qué RAG falla para notas personales

RAG funciona bien para preguntas directas pero falla en síntesis. Si la respuesta requiere conectar cinco documentos, el sistema debe encontrar los cinco fragmentos relevantes, esperar que el retrieval funcione, e integrarlos al vuelo. No hay acumulación de conexiones previas. Además, los fallos de recuperación son silenciosos: el sistema genera una respuesta incompleta sin avisar. Para el conocimiento personal, donde las preguntas suelen ser del tipo "¿cómo se relaciona X con los patrones que he observado en los últimos meses?", esto no es aceptable.

### 2. Las 3 capas del LLM Wiki

El sistema se organiza en tres capas con roles bien definidos:

```
wiki/
├── raw/          ← fuentes originales INMUTABLES (PDFs, artículos, notas)
│   ├── articles/
│   ├── papers/
│   └── notes/
├── wiki/         ← páginas markdown mantenidas por Claude (conceptos, entidades, síntesis)
├── index.md      ← índice mantenido automáticamente por Claude
└── CLAUDE.md     ← el schema: estructura, convenciones, comandos
```

- **`raw/`**: Claude solo lee aquí. Las fuentes son inmutables: son el registro de lo que existe.
- **`wiki/`**: La capa de Claude. Páginas de conceptos, entidades, comparativas y síntesis con `[[wikilinks]]` entre ellas. Las conexiones se construyen en el momento de la ingesta, no de la consulta.
- **`CLAUDE.md`**: Define las reglas del sistema: estructura de carpetas, convenciones de nombres, cómo manejar contradicciones, comandos de ingest/query/lint.

### 3. Las 3 operaciones cotidianas

**Ingest** — cuando encuentras algo que vale la pena guardar:
```
ingest articles/ese-paper-que-guarde.pdf
```
Claude lee la fuente, crea o actualiza páginas wiki (resumen de la fuente, páginas de conceptos nuevos, páginas de entidades mencionadas) y actualiza `index.md`. La fuente queda marcada como procesada.

**Query** — cuando quieres consultar tu base de conocimiento:
```
¿Qué he capturado sobre [tema]? Resume los patrones clave y enlaza a las páginas wiki relevantes.
```
Claude busca en la wiki (partiendo del índice), recupera las páginas relevantes y responde con citas a páginas wiki —no a fuentes raw—. Como las páginas ya contienen síntesis de ingestas anteriores, la respuesta puede cruzar múltiples fuentes sin repetir el trabajo de síntesis.

**Lint** — mantenimiento semanal (10 minutos):
Claude comprueba enlaces rotos, páginas huérfanas, contradicciones entre páginas y conocimiento desactualizado. Sin lint periódico, la calidad de las conexiones degrada con el tiempo.

### 4. Setup en 30 minutos con Claude Code + Obsidian (opcional)

1. Crear la estructura de carpetas (`wiki/raw/`, `wiki/wiki/`, `wiki/index.md`).
2. Escribir el `CLAUDE.md` **antes** de ingerir nada. Las convenciones deben estar definidas desde el inicio; si no, Claude las inventa de forma inconsistente.
3. Abrir la carpeta en Obsidian como nuevo vault (opcional, pero añade navegación visual por los wikilinks).
4. Ingerir 5 fuentes de un dominio concreto que estés trabajando activamente.
5. Hacer la primera query y ajustar el schema si hace falta.
6. Programar un lint semanal.

### 5. Reglas de oro

- **Las fuentes raw son inmutables.** En el momento en que Claude edita una fuente original, pierdes la trazabilidad de qué era original y qué fue generado.
- **No indexar todo.** Al contrario que RAG, la calidad del índice importa más que el tamaño. Si una fuente no vale la pena releer, no vale la pena ingerir.
- **Empezar pequeño.** 50 páginas bien conectadas son más útiles que 500 vagamente indexadas. La ventaja competitiva del sistema llega con el tiempo, no con el volumen inicial.

## Datos relevantes

| Concepto | Dato |
|---|---|
| Stars del Gist de Karpathy | 5.000+ en pocos días |
| Límite de escala recomendado | ~50.000–100.000 tokens de wiki (~500–1.000 notas) |
| Setup inicial | ~30 minutos |
| Lint de mantenimiento | ~10 minutos/semana |
| Repos de referencia | nvk/llm-wiki, Ar9av/obsidian-wiki |

## Conclusión

El patrón LLM Wiki de Karpathy invierte la lógica de RAG: en lugar de buscar en el momento de la consulta, Claude construye y mantiene las conexiones en el momento de la ingesta. El resultado es un sistema de conocimiento personal que se comporta como un cuaderno que se escribe solo, con la capacidad de responder preguntas sintéticas que RAG no puede responder bien. Para bases de conocimiento personales de tamaño razonable (<100K tokens), no necesita infraestructura: solo markdown, Claude Code y un CLAUDE.md bien escrito.

## Tags
`knowledge-management` `llm-wiki` `rag-alternative` `karpathy` `claude-code` `obsidian` `personal-knowledge-base` `markdown`
