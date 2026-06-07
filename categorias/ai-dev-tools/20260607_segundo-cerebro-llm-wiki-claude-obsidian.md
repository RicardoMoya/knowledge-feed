# Build an AI Second Brain (LLM Wiki Pattern) With Claude Code and Obsidian

**Autor:** Tahir  
**Fuente:** [Medium](https://medium.com/@tahirbalarabe2/build-an-ai-second-brain-llm-wiki-pattern-with-claude-code-and-obsidian-fc41cc213d50)  
**Fecha original:** 2026-06-01  
**Fecha de resumen:** 2026-06-07  
**Categoría:** ai-dev-tools  
**GitHub:** [Build-an-AI-Second-Brain-LLM-Wiki-Pattern](https://github.com/balarabetahir/Build-an-AI-Second-Brain-LLM-Wiki-Pattern-With-Claude-Code-and-Obsidian)

---

## La distinción fundamental

Un segundo cerebro de verdad necesita dos partes:

1. **La parte que recoge** — ingesta, organiza, almacena
2. **La parte que piensa/conecta** — vincula conceptos, encuentra relaciones, razona sobre el conjunto

La mayoría de sistemas (Notion, Obsidian básico, Roam) solo tienen la primera. Son archivadores glorificados: metes cosas, las buscas después. Eso no es un segundo cerebro, es un disco duro con opiniones.

El **patrón LLM Wiki** añade la segunda parte mediante Claude Code: slash commands que ingieren, enlazan y consultan el vault de Obsidian de forma autónoma.

---

## Los 3 comandos clave

| Comando | Función |
|---------|---------|
| `/ingest` | Procesa ficheros crudos (PDFs, notas sueltas, scraped data) y los convierte en notas estructuradas del vault |
| `/link` | Analiza las notas existentes y detecta conexiones semánticas, creando `[[wiki-links]]` entre conceptos relacionados |
| `/query` | Busca en el wiki y devuelve respuestas con citas verificables (fichero, línea, fuente original) |

### Cómo se ve una página del wiki generada

Las páginas son deliberadamente **escasas**. Una nota sobre "mobility" puede mostrar solo:

```
[[threat-modelling]]
[[maestro-framework]]
[[ai-agent-governance]]
[[securing-ai-agents-data-governance]]
[[risen-framework]]
[[ics-security]]
```

Sin explicaciones, sin resúmenes, sin juicios. Solo nombres y enlaces. La intención es clara: el wiki no reemplaza el pensamiento, lo facilita mostrando qué ya tienes y cómo conecta.

---

## La prueba de un buen segundo cerebro

El autor propone una prueba concreta: no es si el sistema *recuerda cosas*, sino si puede **probar de dónde viene cada respuesta**.

Cuando se ejecuta `/query`, Claude Code devuelve siempre:
- Qué fichero raw contiene la información
- En qué línea
- Cuál era la fuente original

Sin trazabilidad, el sistema es una caja negra que puede confabular. Con trazabilidad, cada respuesta es verificable y el conocimiento acumulado es confiable.

---

## División de responsabilidades

| Responsabilidad de Claude Code | Responsabilidad del humano |
|-------------------------------|---------------------------|
| Ingesta y normalización de datos | Decidir qué vale la pena ingestar |
| Detección de duplicados | Juzgar qué conexiones tienen sentido |
| Creación de wiki-links semánticos | Determinar qué descartar |
| Detección y rotura de links muertos | Establecer criterios de "relevante" |
| Consultas con citas | Interpretar los resultados |

Claude Code maneja el trabajo mecánico y repetitivo. El juicio sobre *qué importa* sigue siendo del humano. Esto es la distinción que el autor enfatiza: no se trata de delegar el pensamiento, sino de eliminar la fricción que impide pensar.

---

## Contexto de uso real

El autor lo desarrolló trabajando desde un workstation remoto en Moalboal, manejando grandes volúmenes de datos de mercado no estructurados que "se volvieron físicamente imposibles de mantener en la RAM humana". El patrón surge de una necesidad real de escalar la capacidad de procesar y conectar información sin perder la capacidad de razonar sobre ella.

---

## Instalación

```bash
# Prerequisito: Claude Code instalado y vault de Obsidian existente
# Clonar el repositorio
git clone https://github.com/balarabetahir/Build-an-AI-Second-Brain-LLM-Wiki-Pattern-With-Claude-Code-and-Obsidian

# Seleccionar el agente sdlc.orchestrator en el IDE
# Apuntar al vault de Obsidian como directorio de trabajo
# Ejecutar /ingest sobre los primeros ficheros
```

---

## Relación con otros patrones

Este artículo implementa el **LLM Wiki Pattern** descrito por Karpathy, que propone usar un wiki de Markdown como memoria persistente y estructurada para LLMs en lugar de RAG o conversaciones largas. La diferencia clave: el wiki se construye y mantiene de forma incremental a lo largo del tiempo, no se genera de una vez.

---

## Conclusión

El patrón LLM Wiki sobre Obsidian con Claude Code es una de las implementaciones más pragmáticas de memoria persistente para trabajo del conocimiento. La clave no está en la complejidad técnica (los slash commands son simples), sino en la disciplina de ingestar consistentemente y en que `/query` siempre cite sus fuentes. Un segundo cerebro que no puede probar lo que sabe no es mejor que uno que no sabe nada.
