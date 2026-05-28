# Spec-driven development
### 🇪🇸 Desarrollo orientado a especificaciones

**Autor:** Liu Shangqi (Thoughtworks)
**Publicación:** Thoughtworks on Medium
**URL:** [enlace](https://thoughtworks.medium.com/spec-driven-development-d85995a81387)
**Fecha:** 2025-12-10
**Categoría:** spec-driven-design
**Fuente:** Medium
**Tags:** `spec-driven-development` `ai-coding` `context-engineering` `bdd` `claude-code` `cursor` `vibe-coding` `software-engineering`

---

## Contexto y tesis principal
El Spec-Driven Development (SDD) es uno de los paradigmas más importantes emergidos en 2025, aunque con menos visibilidad que el vibe coding. Consiste en usar especificaciones de software bien elaboradas como prompts para agentes de código IA que generan código ejecutable. El artículo, escrito por el Technology Director APAC de Thoughtworks, define qué es SDD, en qué se diferencia del vibe coding y del waterfall, y cómo practicarlo con herramientas actuales.

## Puntos clave

### 1. Qué es una especificación (y por qué no es un PRD)
Una spec no es un documento de requisitos de producto. Define el comportamiento *externo* del software: mappings input/output, precondiciones/postcondiciones, invariantes, tipos de interfaz, contratos de integración, lógica secuencial y máquinas de estados. Hoy puede escribirse en lenguaje natural gracias a los LLMs, pero sigue describiendo comportamiento, no solo intenciones de negocio. Las especificaciones en formato semi-estructurado o que fuercen outputs estructurados mejoran significativamente el razonamiento del modelo y reducen alucinaciones.

### 2. SDD vs. vibe coding vs. waterfall
- **Vibe coding**: demasiado rápido y haphazard → código fácil de generar como prototipo pero no mantenible.
- **Waterfall**: ciclos de feedback demasiado largos, desconexión entre diseño e implementación, "shadow architecture".
- **SDD**: aporta análisis de requisitos serio + diseño prudente + restricciones arquitectónicas + human-in-the-loop. Ciclos de feedback más cortos que waterfall, más rigor que vibe coding. No es un retorno al waterfall —el problema que resuelve es diferente.

### 3. Flujo práctico de SDD
1. **Fase de planificación**: el agente IA analiza requisitos y genera planes de diseño e implementación formalizados en ficheros `.md`.
2. **Revisión iterativa humana** de las specs (human-in-the-loop obligatorio).
3. **Fase de implementación**: el agente genera código basándose en las specs + reglas técnicas definidas en Cursor rules / AGENTS.md.

Herramientas con workflows predefinidos: Amazon Kiro, GitHub Spec-Kit. Herramientas metodológicamente neutras que requieren definir el propio workflow: Cursor, Claude Code.

### 4. SDD como context engineering
Las specs comprimen la información contextual necesaria para las fases de implementación. BDD spec-by-example es esencialmente few-shot prompting. La separación entre análisis de requisitos e implementación comprime el contexto en specs. MCP servers como Context7 aportan documentación en tiempo real; CodeConcise extrae estructura y dependencias de codebases legacy y las almacena en graph/vector databases para soporte en generación de código.

### 5. Qué hace buena a una spec
- Lenguaje ubicuo orientado al dominio (no a la implementación técnica).
- Estructura clara con escenarios Given/When/Then.
- Completa pero concisa (cubre el critical path sin enumerar todos los casos → ahorra tokens).
- Clara y determinista (reduce alucinaciones).
- Inputs semi-estructurados u outputs forzados a formato estructurado mejoran el razonamiento.
- Las specs machine-readable siguen siendo esenciales en la era LLM.

### 6. Riesgos y retos actuales
- No hay consenso sobre el workflow correcto ni sobre qué hace buena a una spec en el contexto de AI-assisted coding.
- La generación código-desde-spec no es determinista → requiere CI/CD robusto.
- Spec drift y alucinaciones son difíciles de evitar estructuralmente.
- Debate abierto: ¿es la spec o el código el artefacto definitivo del desarrollo de software? Las dos posiciones llevan a workflows y prácticas radicalmente diferentes.

## Datos relevantes

| Herramienta | Enfoque SDD |
|-------------|------------|
| Amazon Kiro | Workflow predefinido de SDD |
| GitHub Spec-Kit | Workflow predefinido de SDD |
| Cursor | Metodológicamente neutro (requiere workflow propio) |
| Claude Code | Metodológicamente neutro (requiere workflow propio) |

## Conceptos adicionales
- La separación de fases planificación/implementación que hacen los agentes actuales es la base técnica sobre la que se construye SDD.
- Experiencias de BDD siguen siendo válidas: lenguaje ubicuo, Given/When/Then, documentos vivos.
- "Prompt engineering optimiza la interacción humano-LLM; context engineering optimiza la interacción agente-LLM."

## Conclusión
SDD no es un retorno al waterfall. Es una respuesta al exceso de vibe coding: incorpora rigor de ingeniería —análisis de requisitos, diseño de arquitectura, restricciones técnicas, human-in-the-loop— sin sacrificar la velocidad que dan las herramientas actuales. Los retos son reales (no hay estándares, no determinismo, debate sobre el artefacto definitivo), pero la práctica está madurando rápidamente y 2026 traerá más cambios.

## Tags
`spec-driven-development` `ai-coding` `context-engineering` `bdd` `claude-code` `cursor` `vibe-coding` `software-engineering`
