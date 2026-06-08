# Claude Code's 5-Layer Agent Development Kit: The Architecture Most Engineers Are Missing

**Autor:** Youssef Hosni  
**Fuente:** [Medium – Level Up Coding](https://medium.com/gitconnected/claude-codes-5-layer-agent-development-kit-the-architecture-most-engineers-are-missing-2e670e5f85ec)  
**Fecha original:** 2026-05-06  
**Fecha de resumen:** 2026-06-08  
**Categoría:** ai-dev-tools

---

## La tesis central

La mayoría de ingenieros usa Claude Code como si fuera un asistente de chat avanzado. En realidad, están sentados encima de un runtime de agentes completo que casi nunca inspeccionan. Claude Code ya expone una arquitectura de 5 capas para memoria, expertise, guardarraíles, delegación y acceso a herramientas externas. Sin entenderla, los workflows siguen siendo frágiles aunque el modelo sea excelente.

---

## Las 5 capas

### Capa 1 — CLAUDE.md: Memoria y política persistente

CLAUDE.md es el fichero que Claude lee al inicio de cada sesión antes de hacer nada. No es un prompt. Es la política operacional del proyecto: convenciones de código, qué nunca tocar, patrones de arquitectura aprobados, reglas del equipo.

La distinción clave es que CLAUDE.md *siempre se carga*, independientemente de la tarea. Es la línea de base de comportamiento que evita que el equipo repita las mismas restricciones en cada sesión. Funciona en macOS, Linux, WSL y Windows, lo que refuerza su rol como capa de política duradera, no de preferencias personales.

### Capa 2 — Skills: Conocimiento modular bajo demanda

Si CLAUDE.md es la memoria permanente, las Skills son el expertise especializado que se carga *solo cuando hace falta*. Se definen en un fichero `SKILL.md` y Claude puede invocarlas explícitamente con un slash command o activarlas automáticamente cuando detecta que la tarea es relevante.

La diferencia arquitectónica con CLAUDE.md es importante: las Skills no viven en la memoria permanente. Son módulos cargados bajo demanda, lo que evita sobrecargar el contexto con instrucciones irrelevantes para la tarea actual.

### Capa 3 — Hooks: Guardarraíles deterministas

Los Hooks son el mecanismo de control que el artículo distingue explícitamente de las instrucciones *advisory* de CLAUDE.md. Mientras que CLAUDE.md *recomienda* comportamientos, un Hook los *impone* de forma determinista, sin excepciones.

Se ejecutan en puntos de control específicos — antes o después de que Claude use una herramienta, escriba un fichero o ejecute un comando. La propia documentación de Anthropic establece la regla: *"Los hooks son la herramienta correcta cuando algo debe ocurrir siempre, con cero excepciones."* Ejemplos típicos: validar que ningún secreto se incluye en un commit, ejecutar el linter automáticamente antes de guardar, o bloquear escrituras fuera de directorios permitidos.

### Capa 4 — Subagents: Delegación con contexto aislado

Los Subagents resuelven el problema de degradación de contexto. Cuando un agente hace todo en un solo hilo — búsquedas, exploración, implementación, logs — el contexto se contamina con información que nunca se reutilizará y que diluye la atención del modelo.

La solución: el agente principal delega tareas secundarias a workers especializados que operan en su propia ventana de contexto y devuelven únicamente el resultado.

| Beneficio | Descripción |
|-----------|-------------|
| Preservación de contexto | La exploración y los logs no contaminan el hilo principal |
| Aplicación de restricciones | Cada subagente tiene acceso limitado a herramientas |
| Especialización | Prompts focalizados para tareas concretas |
| Reducción de coste | Las tareas simples se pueden enrutar a modelos más baratos |
| Reutilización | Las configuraciones de subagentes son portables entre proyectos |

### Capa 5 — Plugins + MCP: Distribución y conexión externa

Los Plugins empaquetan las capas anteriores (CLAUDE.md, Skills, Hooks) en unidades distribuibles que un equipo puede instalar en un comando. MCP (Model Context Protocol) es la capa de conexión con sistemas externos: bases de datos, APIs, servicios de terceros. Juntos resuelven el problema de portabilidad — cómo llevar un workflow funcional de un proyecto a otro, o de un ingeniero a todo un equipo.

---

## El mapa completo

| Capa | Problema que resuelve | Cuándo se activa |
|------|-----------------------|------------------|
| CLAUDE.md | ¿Qué recordar siempre? | Cada sesión, sin excepción |
| Skills | ¿Cómo especializarse? | Bajo demanda, por tarea |
| Hooks | ¿Qué imponer sin excepciones? | En puntos de control deterministas |
| Subagents | ¿Qué delegar? | Tareas secundarias con contexto propio |
| Plugins + MCP | ¿Cómo distribuir y conectar? | Instalación y llamadas a sistemas externos |

---

## Conclusión

El artículo propone releer Claude Code no como un asistente de terminal sino como un sistema operacional de agentes. El modelo puede ser excelente, pero sin las capas que lo rodean el workflow sigue siendo frágil: el contexto se degrada, las reglas se olvidan, el comportamiento no es reproducible entre proyectos ni entre miembros del equipo. Las 5 capas no son opcionales — son la diferencia entre usar Claude Code y *controlarlo*.

---

## Glosario rápido

| Concepto | Qué es | Analogía |
|----------|--------|----------|
| **CLAUDE.md** | Fichero de memoria y política que Claude lee siempre al arrancar | El reglamento interno que lee un empleado nuevo antes de su primer día |
| **Skills** | Módulos de expertise especializado cargados bajo demanda desde `SKILL.md` | Un especialista al que derivas cuando el caso lo requiere, no uno que está siempre en la sala |
| **Hooks** | Scripts que se ejecutan antes o después de acciones de Claude, de forma determinista e irrenunciable | El vigilante de seguridad que inspecciona todo lo que entra y sale del edificio, sin excepciones |
| **Subagents** | Agentes secundarios con su propio contexto aislado a los que el agente principal delega tareas | Equipos de obra independientes (fontanería, electricidad, pintura) que trabajan en paralelo y reportan al director |
| **Plugins + MCP** | Paquetes instalables que distribuyen capas de configuración + protocolo de conexión con sistemas externos | Las apps del móvil (amplían capacidades) + el cable USB (conectan con el mundo exterior) |
