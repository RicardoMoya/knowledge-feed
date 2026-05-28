# 📚 Knowledge Feed

Base de conocimiento personal con resúmenes en español de artículos, posts y contenido de cualquier fuente, generados automáticamente con ayuda de IA y organizados por temática.

---

## ¿Qué es este repositorio?

Este repositorio actúa como una **base de conocimiento personal** que crece día a día. Se nutre de dos formas: accediendo automáticamente al feed de recomendaciones de Medium para descubrir novedades, o resumiendo bajo demanda cualquier URL que el usuario proporcione (Medium, LinkedIn, blogs, o cualquier otra fuente).

El objetivo es transformar el hábito de leer en un archivo consultable, estructurado y permanente: en lugar de leer un artículo y olvidarlo, cada lectura queda destilada en un resumen de ~2 minutos organizado por temática y etiquetado con tags para facilitar la búsqueda.

---

## ¿Cómo funciona?

El proceso está automatizado mediante la skill **knowledge-feed** de Claude AI, que sigue este flujo:

1. **Accede al feed de Medium** y recoge los artículos recomendados
2. **Filtra** los artículos ya resumidos en los últimos 15 días consultando `history.md`
3. **Genera el resumen detallado** de cada artículo nuevo directamente (máx. 10 por sesión)
4. **Presenta todos los resúmenes** al usuario para que decida cuáles subir al repositorio
5. **Clasifica automáticamente** cada resumen seleccionado en la temática correspondiente
6. **Sube los ficheros** al repositorio y actualiza el histórico con un commit

Además, es posible pasar una o varias URLs de cualquier fuente (Medium, LinkedIn, blogs...) y la skill las resumirá y añadirá al repositorio siguiendo las mismas reglas.

---

## Estructura del repositorio

```
📁 knowledge-feed/
│
├── README.md           → Este fichero
├── history.md          → Histórico de todos los artículos resumidos (título, URL, fecha)
├── topics.md           → Lista de categorías/temáticas activas
│
└── summarize-post/     → Resúmenes organizados por temática
    ├── {categoria-1}/
    │   └── YYYYMMDD_nombre-del-articulo.md
    ├── {categoria-2}/
    │   └── ...
    └── {categoria-N}/
```

### Ficheros clave

| Fichero | Descripción |
|--------|-------------|
| `history.md` | Registro de todos los artículos resumidos: título, URL y fecha. Evita duplicados en sesiones futuras. |
| `topics.md` | Lista de categorías activas. Modifica este fichero para añadir, eliminar o renombrar temáticas. |
| `summarize-post/` | Carpeta raíz con una subcarpeta por cada categoría definida en `topics.md`. |

---

## Formato de los resúmenes

Cada resumen es un fichero Markdown con la siguiente estructura:

```markdown
# Título del artículo en su idioma original
### 🇪🇸 Título traducido al español

**Autor:** Nombre
**Publicación:** Nombre de la publicación
**URL:** enlace al artículo original
**Fecha:** YYYY-MM-DD
**Categoría:** nombre-categoria
**Fuente:** Medium / LinkedIn / Blog / ...
**Tags:** `tag-1` `tag-2` `tag-3` `tag-4` `tag-5`

---

## Contexto y tesis principal
...

## Puntos clave
### 1. Primer punto
...
### 2. Segundo punto
...

## Conceptos adicionales destacados
...

## Conclusión
...

## Tags
`tag-1` `tag-2` `tag-3` `tag-4` `tag-5`
```

---

## Nomenclatura de ficheros

Los ficheros siguen el patrón `YYYYMMDD_slug-del-titulo.md`, por ejemplo:

```
20260527_the-4-lines-every-claude-md-needs.md
```

Esto permite ordenarlos cronológicamente y localizar resúmenes por fecha de forma natural.

---

## Histórico (`history.md`)

El fichero `history.md` registra todos los artículos resumidos:

| Título | URL | Fecha |
|--------|-----|-------|
| Ejemplo de artículo | [https://medium.com/...](https://medium.com/@richardmoya/top-papers-that-made-the-llm-era-possible-jarroba-com-7727170de13f) | 2026-05-27 |

La skill consulta este fichero para evitar resumir el mismo artículo dos veces en un período de 15 días. Las URLs añadidas manualmente nunca se vuelven a resumir automáticamente salvo petición explícita.

---

## Gestión de categorías

Las categorías se definen en `topics.md`. Si se modifican (añadir, eliminar o renombrar), la skill ofrece la opción de **reposicionar automáticamente** los artículos existentes, actualizando tanto la ubicación de los ficheros como los metadatos internos de cada resumen.

---

## Convenciones de commits

| Tipo | Mensaje |
|------|---------|
| Sesión diaria | `YYYYMMDD: subidos X artículos resumidos` |
| URL externa (uno) | `YYYYMMDD: añadido artículo externo – Título` |
| URL externa (varios) | `YYYYMMDD: añadidos X artículos externos` |
| Reorganización de categorías | `YYYYMMDD: reorganización de categorías – X artículos reposicionados` |
| Actualización de categorías | `YYYYMMDD: categorías actualizadas en topics.md` |
| Inicialización | `init: estructura del repositorio knowledge-feed creada` |

---

## Tecnología

Este repositorio se genera y mantiene con la skill **knowledge-feed**, construida sobre [Claude AI](https://claude.ai) de Anthropic, usando:

- **Claude in Chrome** — para navegar y leer el contenido de los artículos de cualquier fuente
- **GitHub MCP** — para crear ficheros, hacer commits y gestionar el repositorio
