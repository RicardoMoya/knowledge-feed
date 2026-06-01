# 5 Claude Code Agents You Can Build in less than 10 Minutes

**Autor:** Shashwat  
**Fuente:** [Medium – AI in Plain English](https://medium.com/ai-in-plain-english/5-claude-code-agents-you-can-build-in-less-than-10-minutes-27c808ea4f6f)  
**Fecha original:** 2026-05-18  
**Fecha de resumen:** 2026-06-01  
**Categoría:** ai-dev-tools

---

## Idea central

Claude Code permite definir **subagentes especializados** mediante archivos YAML en `.claude/agents/`. Cada agente tiene un contexto acotado, lo que elimina el ruido de conversaciones largas y reduce el desperdicio de tokens en torno al **90%**. El modelo recomendado para todos ellos es `claude-sonnet-4-5`, aproximadamente **5× más barato** que Opus con rendimiento suficiente para tareas de desarrollo rutinarias.

---

## Los 5 agentes

### 1. PR Summarizer
Genera automáticamente la descripción de un Pull Request: qué cambia, por qué y qué hay que revisar. Se activa sobre el diff del PR y produce un resumen estructurado listo para pegar en GitHub.

### 2. Test Coverage Engine
Analiza el código sin tests y genera casos de prueba unitarios. Prioriza las rutas críticas y los bordes más propensos a fallos, reduciendo el tiempo dedicado a escribir tests desde cero.

### 3. Dead Code Sweeper
Escanea el repositorio en busca de funciones, variables e imports que ya no se usan. Propone eliminaciones seguras con justificación, ayudando a mantener la base de código limpia sin revisiones manuales exhaustivas.

### 4. Database Migration Generator
A partir de cambios en el esquema de datos (nuevas columnas, renombrados, relaciones), genera los scripts de migración correspondientes. Incluye la migración de rollback para mayor seguridad.

### 5. Error Log Analyzer
Ingiere logs de errores en producción y proporciona un diagnóstico: causa probable, archivos implicados y pasos de corrección sugeridos. Útil para triaje rápido sin tener que leer stacks completos manualmente.

---

## Estructura YAML de un agente

Cada agente se define en un fichero `.claude/agents/<nombre>.md` con un frontmatter YAML:

```yaml
---
name: pr-summarizer
model: claude-sonnet-4-5
description: Generates structured PR descriptions from diffs
tools:
  - read_file
  - run_command
---
# Instrucciones del agente
...
```

El campo `description` es clave: Claude Code lo usa para decidir qué agente invocar de forma automática.

---

## Por qué importa la aislación de contexto

Cuando un agente solo ve el contexto relevante para su tarea (el diff, los logs, el esquema), el modelo no procesa miles de líneas de historia de conversación irrelevante. Esto se traduce en:

- **Menor coste** por llamada
- **Mayor precisión** al no haber distracción por contexto ajeno
- **Paralelización**: varios agentes pueden correr simultáneamente sobre distintas partes del código

---

## Conclusión

Definir subagentes en Claude Code es una forma práctica de convertir tareas repetitivas de desarrollo (tests, migraciones, análisis de logs) en flujos automatizados de bajo coste. La inversión inicial —escribir el YAML y las instrucciones— se amortiza desde la primera ejecución.
