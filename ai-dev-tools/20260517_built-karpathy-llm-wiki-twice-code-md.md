# I Built Karpathy's LLM Wiki Twice — Once as Code, Once as a .md

**Autor:** Leandro Bernardo
**Publicación:** Towards AI
**URL:** [enlace](https://medium.com/towards-artificial-intelligence/i-built-karpathys-llm-wiki-twice-once-as-code-once-as-a-md-heres-what-each-one-gives-up-08b31170999a)
**Fecha de resumen:** 2026-05-27
**Categoría:** ai-dev-tools

---

## Contexto y tesis principal

En abril de 2026, Andrej Karpathy publicó `llm-wiki.md`: un patrón en prosa diseñado para pegarlo en un agente LLM y que construya una wiki estructurada a partir de documentos del usuario. La premisa: tratar el conocimiento como los compiladores tratan el código fuente — pre-procesar una vez en una wiki interconectada y consultarla después, en lugar de releer fuentes en bruto en cada query (RAG). El autor implementó el mismo concepto dos veces —como paquete Python y como AGENTS.md— y compara qué ofrece y qué sacrifica cada enfoque.

## Puntos clave

### 1. El concepto original de Karpathy
RAG actúa como lector pasivo: el retriever decide qué páginas mostrarle. Karpathy propone un giro: pre-compilar documentos en una wiki con `[[wikilinks]]`, siendo el agente el compilador. Más rápido, más coherente, no depende del retriever.

### 2. La implementación programática: `wiki-llm`
Pipeline Python de 8 etapas (Scan → Plan → Generate → Evaluate → Edit → Lint → Repair → Consolidate). Contratos con Pydantic v2, IDs deterministas por SHA-256, LangGraph para el repair agent, BM25 para el chat retriever. Corre en Docker + CronJob de Kubernetes.

### 3. El problema de Markdown que la mayoría ignora
El Markdown generado por LLMs es plausible pero estructuralmente defectuoso. El autor desarrolló `markdown-hero`: librería con type checking, section awareness y chunker que mantiene el overlap dentro del mismo heading.

### 4. La implementación agéntica: `AGENTS.md`
Los mismos 8 stages en un único fichero de instrucciones. Sin Pydantic, Docker, LangGraph ni UUIDs. Funciona con Claude Code, Codex, Cursor y VS Code Agent Mode.

### 5. Cuándo elegir cada enfoque
Python: corpus grandes, equipos, pipelines de automatización downstream, trazabilidad. AGENTS.md: wikis personales (<200 docs), cuando la forma aún está por descubrir, cuando ya se usa Claude Code o Cursor a diario.

## Conclusión

La elección no es de calidad sino de constraints: tamaño del corpus, necesidades de reproducibilidad y si el pipeline alimenta automatización downstream.
