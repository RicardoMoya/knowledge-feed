# CLAUDE.md Best Practices: The Complete Guide for 2026

**Autor:** IAKH Studio
**Publicación:** Medium (IAKH Studio)
**URL:** [enlace](https://medium.com/@ikh4ever/claude-md-best-practices-the-complete-guide-for-2026-e35e1408cef2)
**Fecha de resumen:** 2026-05-27
**Categoría:** ai-dev-tools

---

## Contexto y tesis principal

CLAUDE.md es el fichero que Claude Code carga automáticamente al inicio de cada sesión: un sistema de prompt persistente específico del proyecto. A diferencia de la documentación ordinaria, está diseñado para que la IA razone sobre él activamente. Este artículo sostiene que CLAUDE.md es la palanca más poderosa disponible para cualquier usuario de Claude Code, y que la mayoría la desaprovecha por escribirla de forma demasiado larga o mal estructurada.

## Puntos clave

### 1. La estructura de tres capas
El artículo propone organizar CLAUDE.md en tres niveles bien diferenciados. El primero es el **What**: el stack tecnológico, la estructura del proyecto y las dependencias clave. El segundo es el **Why**: el propósito de los componentes principales y las decisiones arquitectónicas. El tercero —y más valioso— es el **How**: las reglas explícitas sobre cómo debe trabajar Claude. Documentar el *por qué* detrás de las decisiones vale diez veces más que dar una instrucción sin contexto.

### 2. La regla de oro: menos es más
El error más común es escribir un CLAUDE.md demasiado largo. Boris Cherny, ingeniero de Anthropic y creador de Claude Code, mantiene el suyo en menos de 100 líneas. El filtro: "¿quitarla haría que Claude cometiera errores?". El consenso recomienda no superar las 300 líneas.

### 3. Qué incluir y qué excluir
Pertenece: comandos bash (build, test, lint), guías de estilo, ficheros que nunca deben modificarse, patrones arquitectónicos clave y "gotchas" no evidentes. No pertenece: prácticas autoevidentess, conocimiento específico de un workflow ocasional, ni instrucciones que Claude ya sigue correctamente.

### 4. Uso de `/init` y sub-CLAUDE.md para monorepos
El comando `/init` genera un primer borrador sólido. Para monorepos, la mejor práctica es crear ficheros CLAUDE.md anidados en subdirectorios, manteniendo cada fichero corto y enfocado.

### 5. CLAUDE.md vs Skills
CLAUDE.md se carga en cada sesión automáticamente para reglas universales. Las Skills (SKILL.md) se cargan bajo demanda para workflows especializados y ocasionales.

## Conclusión

CLAUDE.md no es documentación para humanos: es un sistema de prompt de proyecto. Mantenerlo corto, específico y vivo es lo que diferencia un uso amateur de un uso experto de Claude Code.
