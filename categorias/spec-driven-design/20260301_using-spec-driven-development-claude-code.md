# Using Spec-Driven Development with Claude Code
### 🇪🇸 Usando Desarrollo Dirigido por Especificaciones con Claude Code

**Autor:** Heeki Park
**Publicación:** Medium (blog personal)
**URL:** [enlace](https://heeki.medium.com/using-spec-driven-development-with-claude-code-4a1ebe5d9f29)
**Fecha:** 2026-05-27
**Categoría:** spec-driven-design
**Fuente:** Medium
**Tags:** `spec-driven-development` `claude-code` `aws` `agentcore` `field-report` `best-practices` `serverless` `cloudformation`

---

## Contexto y tesis principal

El autor, solutions architect (no software engineer de profesión), comparte su experiencia práctica usando Spec-Driven Development (SDD) con Claude Code para construir un prototipo de integración con **AgentCore Gateway** de AWS. El artículo es un field report honesto: no teoría, sino lecciones aprendidas de un proyecto real. Su tesis es que el "vibe coding" acumula deuda técnica y que SDD actúa como freno para reforzar buenas prácticas de ingeniería sin sacrificar las ganancias de productividad de la IA generativa.

## Puntos clave

### 1. Los 3 niveles de Spec-Driven Development
Referenciando a Birgitta Böckeler, el autor describe una escala de madurez:

| Nivel | Descripción |
|---|---|
| **Spec-first** | Spec bien pensada se escribe primero, luego se usa en el flujo de desarrollo |
| **Spec-anchored** | Spec se mantiene activa después de la tarea, para evolución y mantenimiento de la feature |
| **Spec-as-source** | La spec es el fichero fuente principal; solo el humano edita la spec, nunca toca el código |

El autor reconoce que tiende hacia spec-first pero advierte de la trampa: es muy fácil empezar bien y luego olvidar la spec a medida que el proyecto avanza — lo que llama **"spec-once development"**.

### 2. Estructura del proyecto: 2 sub-proyectos, 4 fases
Para el prototipo con AgentCore Gateway, el autor dividió el trabajo en **2 sub-proyectos** y **4 fases** de despliegue en CloudFormation/SAM:
- **Stack 1:** Interceptor Lambda como función serverless independiente (testeable con SAM de forma aislada)
- **Stack 2:** MCP server desplegado en AgentCore Runtime con tests local + endpoint
- **Stack 3:** AgentCore Gateway con credential provider OAuth2 para conectar con el MCP server
- **Stack 3 (update):** Integración del interceptor en el Gateway, añadiendo logging de headers

La separación en stacks modulares fue deliberada: permite testear y corregir por fases antes de componer el sistema completo.

### 3. Datos técnicos clave sobre Claude Code

> 📊 **Ventana de contexto:** Plan Pro (por defecto) = **200.000 tokens**; Max 20x = **1.000.000 tokens**. Modelos disponibles en Bedrock con contexto de **1M tokens** (beta): Claude Opus 4.6, Sonnet 4.6, Sonnet 4.5 y Sonnet 4.

> 📊 **Compactación automática:** cuando se alcanza el límite de 200k tokens, Claude Code compacta automáticamente el historial. Tiempos observados: caso ligero **3-5 minutos**; caso pesado **10-12 minutos**.

> 📊 **Límites de uso por modelo:** Claude Opus 4.6 alcanza límites en **45 minutos a 1 hora** de uso intensivo con Plan Pro. Claude Sonnet 4.6 sin límites observados incluso después de **varias horas** de uso intensivo.

### 4. Flujo de trabajo del autor: 3 fases antes de generar código
1. **Due diligence:** leer documentación y fuentes relevantes; dirigir a Claude Code a ingerir la misma documentación para operar con el mismo contexto
2. **Planificación de recursos AWS:** pensar en dependencias entre recursos, testing modular y flujo general; descomponer en sub-proyectos y fases antes del primer prompt
3. **Spec como documento vivo:** escribir todos los planes como especificación, revisarla antes de ejecutar, y actualizarla cada vez que se necesite corregir el rumbo

**Consejo clave:** instruir a Claude Code para que haga preguntas de aclaración con **opciones seleccionables** (no texto libre). Al final presenta un resumen de las preguntas hechas y las respuestas elegidas — hace el feedback mucho más rápido.

### 5. Lecciones aprendidas del proyecto real
Las 5 lecciones más importantes del field report:

1. **El tiempo en planificación genera dividendos en implementación.** En proyectos anteriores con prompts cortos, el autor corregía constantemente el rumbo y a veces se planteaba empezar desde cero. En este proyecto, la mayoría de interacciones de seguimiento fueron ajustes menores, no cambios radicales.

2. **Construir de forma escalonada en módulos pequeños y fácilmente testeables.** La planificación previa facilita testear y corregir funcionalidad. Equivalente al patrón de *stacked pull requests* en ingeniería tradicional.

3. **Flexibilidad y workarounds son frecuentes.** El nuevo AgentCore CLI (en public preview) no soportaba la funcionalidad necesaria → fallback a CloudFormation. Los credential providers no se podían crear con CloudFormation → fallback a boto3.

4. **Seguridad debe integrarse desde el principio.** El autor añadió OAuth2 al final del proyecto, lo que requirió redesplegar el stack completo. La propiedad `AuthorizerType` del Gateway no se puede cambiar en lugar (requiere reemplazar el recurso).

5. **Revisar y actualizar la spec en cada corrección de rumbo.** Cada vez que se necesitó ajustar el diseño, se actualizó la spec. Esto mantiene al agente anclado en la intención original y evita que el "spec-once" silenciosamente se apodere del proyecto.

## Datos relevantes

| Métrica | Valor |
|---------|-------|
| Ventana de contexto Plan Pro | 200.000 tokens |
| Ventana de contexto Max 20x | 1.000.000 tokens |
| Compactación automática (caso ligero) | 3-5 min |
| Compactación automática (caso pesado) | 10-12 min |
| Límite uso Opus 4.6 (uso intensivo) | 45 min – 1 hora |
| Límite uso Sonnet 4.6 (uso intensivo) | Sin límites observados |
| Opciones de permiso por acción | 3 (sí / sí siempre / no) |

## Conceptos adicionales

- Durante la investigación, Claude Code intentó hacer web search y encontró el propio blog del autor publicado con AWS in Plain English. **Medium bloqueó al agente** impidiéndole acceder al contenido de la plataforma.
- Herramientas usadas: AgentCore CLI (AWS, public preview), AgentCore Gateway + Runtime, SAM para testing local, tmux/iTerm2 para sesiones paralelas de Claude Code.
- Al final del proyecto, el autor creó su primera skill personalizada (SKILL.md), que anticipa como acelerador significativo para proyectos futuros.

## Conclusión

SDD es menos un framework rígido que una disciplina: obliga al desarrollador a pensar profundamente en los requisitos, documentar decisiones arquitectónicas y revisar continuamente el diseño. El valor real no está en la primera versión del spec, sino en mantenerla viva como fuente de verdad mientras el proyecto evoluciona. Claude Code resulta práctico para este flujo, con fricciones conocidas (límites de contexto de 200k tokens en Pro, compactaciones de 3-12 minutos, límites de uso con Opus en menos de 1 hora). El autor anticipa el siguiente paso: **agent teams** para proyectos más grandes, asignando issues a agentes como si fueran miembros del equipo.

## Tags
`spec-driven-development` `claude-code` `aws` `agentcore` `field-report` `best-practices` `serverless` `cloudformation`
