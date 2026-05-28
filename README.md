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
├── .gitignore          → Archivos ignorados por git
│
└── categorias/
    └── {categoria}/
        └── YYYYMMDD_nombre-del-articulo.md
```

---

## Categorías activas

- ai-dev-tools
- spec-driven-design
- llms
- green-ai

---

## Tecnología

Este repositorio se genera y mantiene con la skill **knowledge-feed**, construida sobre [Claude AI](https://claude.ai) de Anthropic.
