# I Built Karpathy's LLM Wiki Twice — Once as Code, Once as a .md
### 🇪🇸 Construí la LLM Wiki de Karpathy dos veces: una en código, otra en Markdown

**Autor:** Leandro Bernardo
**Publicación:** Towards AI
**URL:** [enlace](https://medium.com/towards-artificial-intelligence/i-built-karpathys-llm-wiki-twice-once-as-code-once-as-a-md-heres-what-each-one-gives-up-08b31170999a)
**Fecha:** 2026-05-27
**Categoría:** ai-dev-tools
**Fuente:** Medium
**Tags:** `karpathy` `rag` `knowledge-management` `agents-md` `claude-code` `python` `wiki` `llm-patterns`

---

## Contexto y tesis principal

En abril de 2026, Andrej Karpathy publicó un gist de GitHub llamado `llm-wiki.md`: no código, no un producto, sino un "idea file", un patrón en prosa diseñado para pegarlo en un agente LLM y que ese agente construya una wiki estructurada a partir de documentos del usuario. La premisa es tratar el conocimiento como los compiladores tratan el código fuente: pre-procesar una vez en una wiki interconectada y consultarla después, en lugar de releer fuentes en bruto en cada query (RAG). El autor implementó el mismo concepto dos veces —como paquete Python y como fichero AGENTS.md— y este artículo compara qué ofrece y qué sacrifica cada enfoque.

## Puntos clave

### 1. El concepto original de Karpathy y por qué importa
El problema con RAG es que el modelo actúa como lector pasivo: el retriever decide qué páginas mostrarle y él responde. Karpathy propone un giro: pre-compilar los documentos en una wiki estructurada con páginas interconectadas mediante `[[wikilinks]]`, y que el agente sea el compilador. El beneficio: las queries posteriores son más rápidas, más coherentes y no dependen de que el retriever acierte con los chunks relevantes. Es especialmente útil para corpora de cientos o miles de documentos donde el coste de inferencia por cada consulta es real.

### 2. La implementación programática: `wiki-llm`
El autor construyó primero un pipeline Python de **8 etapas**: Scan → Plan → Generate → Evaluate → Edit → Lint → Repair → Consolidate. Las decisiones de ingeniería más relevantes:
- Los contratos entre etapas usan **Pydantic v2** para garantizar tipos, no esperanzas
- Los IDs de página son **deterministas por SHA-256** del contenido: misma página, mismo ID, independientemente del nombre de fichero o frontmatter
- El backend LLM es intercambiable via `instructor` (OpenRouter, OpenAI, Bedrock, Ollama)
- El repair agent usa **LangGraph** solo donde la recursión fan-out lo justifica
- El chat usa **BM25** (retrieval léxico sin vector store) por ser suficientemente rápido y transparente
- Todo cabe en un Docker image y corre en un CronJob de Kubernetes

### 3. El problema de Markdown que la mayoría ignora
El artículo identifica un fallo silencioso en casi todas las wikis generadas por LLMs: el Markdown producido es *plausible* pero estructuralmente defectuoso. Dos H1 en el mismo fichero, tablas que se parsean mal, YAML en frontmatter que rompe cuando hay dos puntos en un valor sin entrecomillar, enlaces que parecen correctos pero resuelven a ningún sitio. Para resolverlo, el autor desarrolló **`markdown-hero`**: una librería de procesamiento Markdown con type checking, section awareness y un chunker cuya ventana de overlap se mantiene dentro del mismo heading.

### 4. La implementación agentica: `AGENTS.md`
Semanas después, el autor destila los mismos 8 stages en un único fichero de instrucciones que funciona con cualquier agente que lea ficheros de instrucciones en la raíz del proyecto: Claude Code, Codex, Cursor, VS Code Agent Mode. Desaparecen Pydantic, Docker, el repair agent de LangGraph y los UUIDs deterministas. Se mantienen: los 8 stages en el mismo orden, el loop Writer→Evaluator→Editor, las convenciones de wikilinks, las reglas de lint y un bloque CONFIG que el agente rellena en una primera ejecución de configuración.

### 5. Cuándo elegir cada enfoque

| Criterio | `wiki-llm` (Python) | `AGENTS.md` |
|---|---|---|
| Tamaño del corpus | Grande (>200 docs) | Pequeño (<200 docs) |
| Reproducibilidad | Determinista (SHA-256) | Variable por sesión |
| Pipeline downstream | Soportado | No soportado |
| Despliegue | Docker + Kubernetes | Solo agente existente |
| Iteración de prompts | Requiere redespliegue | Inmediata |

## Conceptos adicionales

- La calidad del output es comparable en ambas versiones: el patrón Writer→Evaluator→Editor es suficientemente robusto tanto en Python como en instrucciones para agente.
- `pip install markdown-hero` disponible para quien quiera la capa de procesamiento Markdown en sus propios proyectos.

## Conclusión

Este artículo es una demostración práctica de la tensión fundamental en el desarrollo agentico: ¿cuánta implementación debe estar congelada en código y cuánta debe negociarse con el agente en tiempo de ejecución? La respuesta de Karpathy —compartir el patrón, no el código— resultó correcta: el patrón es suficientemente robusto como para que ambas implementaciones lleguen al mismo destino por caminos distintos. La elección entre ellas no es de calidad sino de constraints: tamaño del corpus, necesidades de reproducibilidad y si el pipeline alimenta automatización downstream.

## Tags
`karpathy` `rag` `knowledge-management` `agents-md` `claude-code` `python` `wiki` `llm-patterns`
