# Using Spec-Driven Development with Claude Code

**Autor:** Heeki Park
**Publicación:** Medium (blog personal)
**URL:** [enlace](https://heeki.medium.com/using-spec-driven-development-with-claude-code-4a1ebe5d9f29)
**Fecha de resumen:** 2026-05-27
**Categoría:** spec-driven-design

---

## Contexto y tesis principal

Field report de un solutions architect de AWS que aplicó SDD construyendo un prototipo de integración con **AgentCore Gateway**. El vibe coding acumula deuda técnica; SDD actúa como freno sin sacrificar las ganancias de productividad.

## Los 3 niveles de SDD (Birgitta Böckeler)

| Nivel | Descripción |
|---|---|
| **Spec-first** | Spec se escribe primero, luego guía el desarrollo |
| **Spec-anchored** | Spec se mantiene activa para evolución y mantenimiento |
| **Spec-as-source** | Solo el humano edita la spec; nunca toca el código |

Trampa del **"spec-once development"**: empezar bien y olvidar la spec.

## Estructura del proyecto: 2 sub-proyectos, 4 fases

- Stack 1: Interceptor Lambda (testeable con SAM de forma aislada)
- Stack 2: MCP server en AgentCore Runtime
- Stack 3: AgentCore Gateway + OAuth2
- Stack 3 update: Interceptor integrado + logging

## Datos técnicos sobre Claude Code

- **Plan Pro:** **200.000 tokens** de contexto
- **Max 20x:** **1.000.000 tokens**
- Compactación automática: **3-5 min** (ligera) / **10-12 min** (pesada)
- **Opus 4.6:** límites en **45-60 min** de uso intensivo
- **Sonnet 4.6:** sin límites tras **varias horas**
- Por cada acción: **3 opciones** (sí / sí y no volver a preguntar / no)

## Flujo de trabajo (3 fases antes de generar código)

1. **Due diligence:** leer docs + dirigir a Claude Code a ingerir el mismo contexto
2. **Planificación:** descomponer en sub-proyectos y fases antes del primer prompt
3. **Spec viva:** actualizar en cada corrección de rumbo

**Consejo:** preguntas de aclaración con **opciones seleccionables**, no texto libre.

## Lecciones aprendidas

1. Tiempo en planificación → dividendos en implementación
2. Construir escalonado en módulos pequeños y testeables
3. Flexibilidad y workarounds son frecuentes (AgentCore CLI en preview → fallback CloudFormation)
4. Seguridad desde el principio (OAuth2 al final → redeploy completo del stack)
5. Revisar y actualizar la spec en cada corrección de rumbo

## Curiosidad

Claude Code intentó fetchear el propio blog del autor. **Medium bloqueó el acceso al agente**.

## Conclusión

SDD es menos un framework que una disciplina. Fricciones conocidas: **200k tokens** en Pro, compactaciones de **3-12 min**, límites Opus en **< 1 hora**. Próximo paso: **agent teams** para proyectos más grandes.
