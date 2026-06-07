# How to Build Claude Skills 2.0 Better than 99% of People

**Autor:** Gao Dalie (高達烈)  
**Fuente:** [Medium – Data Science Collective](https://medium.com/data-science-collective/how-to-build-claude-skills-2-0-better-than-99-of-people-af4927dd5335)  
**Fecha original:** 2026-03-14  
**Fecha de resumen:** 2026-06-07  
**Categoría:** ai-dev-tools

---

## El problema que resuelve

Tres quejas recurrentes en equipos que usan IA:

- *"Es un fastidio dar las mismas instrucciones al modelo cada vez"*
- *"El AI nunca recuerda las reglas y formatos de la empresa"*
- *"En el equipo cada uno usa la IA a su manera, así que solo se benefician los que saben usarla bien"*

**Claude Skills** es la funcionalidad de Anthropic diseñada específicamente para resolver estos tres problemas. Permite enseñar a Claude procesos de negocio y conocimiento especializado de forma **persistente**: una vez definida una skill, Claude la carga automáticamente cuando la necesita, sin que el usuario tenga que repetir nada.

---

## Qué es una Skill

Una Skill es esencialmente un **plugin de conocimiento especializado**. Se define en un fichero `SKILL.md` ubicado en `.claude/skills/` y contiene dos partes:

1. **Metadata YAML** (frontmatter): le dice a Claude cuándo existe la skill y cuándo usarla
2. **Cuerpo Markdown**: instrucciones detalladas y ejemplos de uso

```yaml
---
name: Your Skill Name
description: Brief description of what this Skill does and when to use it
---
# Your Skill Name

## Instructions
Provide clear, step-by-step guidance for Claude.

## Examples
Show concrete examples of using this Skill.
```

### El campo `description` es crítico

Claude lee los metadatos de todas las skills en el arranque y los incorpora al system prompt. La `description` es lo único que Claude ve en un primer momento para decidir si invocar o no la skill. Una descripción pobre = skill que nunca se activa. Una descripción precisa = skill que se dispara en el momento exacto.

**Regla práctica:** escribe la description respondiendo a *"¿cuándo debería Claude usar esto?"* con ejemplos de frases que el usuario diría.

---

## Estructura de ficheros

```
proyecto/
└── .claude/
    └── skills/
        ├── redaccion-informes/
        │   └── SKILL.md
        ├── revisor-codigo/
        │   └── SKILL.md
        └── formato-empresa/
            └── SKILL.md
```

Cada skill vive en su propio subdirectorio dentro de `.claude/skills/`. Claude carga solo las que necesita en cada momento (lazy loading), lo que evita sobrecargar el contexto.

---

## Cómo instalar Skills desde el marketplace

Anthropic mantiene un repositorio oficial de skills. Hay dos formas de instalarlas:

### Opción 1: Desde VS Code con la extensión de Claude Code
1. Instalar la extensión **Claude Code** en VS Code (verificar el símbolo de verificación oficial)
2. Hacer clic en el logo de Claude Code en la barra superior
3. Navegar al marketplace de plugins

### Opción 2: Mediante comandos `/plugin`

Añadir el repositorio oficial de Anthropic como fuente:
```
/plugin add https://github.com/anthropics/skills
```

Instalar skills concretas:
```bash
/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
```

Claude detectará las nuevas skills en el siguiente arranque de sesión y las incorporará a su sistema.

---

## Anatomía de una Skill efectiva

### Sección `## Instructions`
- Instrucciones paso a paso, concretas y sin ambigüedad
- Incluir condiciones de activación y desactivación
- Especificar el formato de salida esperado
- Anticipar casos edge

### Sección `## Examples`
- Al menos 2–3 ejemplos completos de input → output
- Los ejemplos son la parte más determinante para la calidad de la skill
- Cuantos más ejemplos representen variación real, más robusta es la skill

---

## Casos de uso más potentes

| Caso de uso | Qué hace la Skill |
|-------------|-------------------|
| Formato corporativo | Aplica plantillas, estilos y tonos de comunicación de la empresa automáticamente |
| Revisor de código | Evalúa PRs según las convenciones internas del equipo |
| Generador de tests | Crea tests siguiendo el framework y estilo que usa el proyecto |
| Documentación técnica | Genera docs en el formato estándar de la organización |
| Análisis de datos | Sigue el pipeline de análisis propio de cada equipo |

---

## Por qué importa para equipos

Sin Skills, el uso de la IA en un equipo es **asimétrico**: los ingenieros más experimentados saben cómo construir prompts efectivos y obtienen resultados mucho mejores que el resto. Las Skills **democratizan el acceso** al nivel avanzado: cualquier miembro del equipo que use Claude obtiene el mismo nivel de calidad porque las instrucciones y el contexto están codificados en la skill, no en la cabeza de un individuo.

Esto convierte las Skills en un **activo de conocimiento organizacional** versionable en git, auditable y mejorable de forma colaborativa.

---

## Limitaciones actuales

- El artículo es de marzo 2026; algunas instrucciones de instalación pueden haber cambiado con actualizaciones posteriores del marketplace
- Las skills de pago o privadas del marketplace requieren autenticación adicional
- El lazy loading depende de que la `description` esté bien escrita; una mala descripción puede hacer que la skill nunca se cargue aunque sea relevante

---

## Conclusión

Claude Skills es la respuesta de Anthropic al problema de la memoria y el contexto persistente en equipos. La clave técnica está en el fichero `SKILL.md`: estructura sencilla (YAML + Markdown), pero cuya calidad determina completamente el comportamiento de la skill. La inversión en escribir buenas instrucciones y ejemplos se amortiza en cada sesión donde Claude no necesita que se le explique nada de nuevo.
