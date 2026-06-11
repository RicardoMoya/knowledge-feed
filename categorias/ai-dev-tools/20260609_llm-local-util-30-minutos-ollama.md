# Run a Useful Local LLM in 30 Minutes (Coding, RAG, Voice)
### 🇪🇸 Ejecuta un LLM local útil en 30 minutos (Código, RAG, Voz)

**Autor:** Anubhav
**Publicación:** Data Science Collective
**URL:** [enlace](https://medium.com/data-science-collective/run-a-useful-local-llm-in-30-minutes-coding-rag-voice-pick-one-9f628082e0d0)
**Fecha:** 2026-06-09
**Categoría:** ai-dev-tools
**Fuente:** Medium
**Tags:** `ollama` `local-llm` `qwen3` `rag` `coding-assistant` `tutorial` `offline-ai`

---

## Contexto y tesis principal

Montar un LLM local útil ya no es un proyecto de entusiasta: con **Ollama + Qwen3:8b** (~5GB de RAM) tienes un asistente de código, un buscador semántico sobre tus notas o un asistente de voz operativo en 8 minutos, a coste cero y sin conexión a internet.

## Puntos clave

### 1. Stack base
```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama run qwen3:8b   # ~5GB, listo para codificar
```
El servidor local corre en `localhost:11434` con API compatible OpenAI — cualquier plugin de VS Code que acepte un endpoint personalizable funciona sin cambios.

### 2. Tres casos de uso concretos

| Caso | Herramientas | Coste |
|---|---|---|
| Asistente de código | Ollama + Qwen3:8b + Continue.dev | 0€ |
| RAG sobre notas | Ollama + nomic-embed-text + ChromaDB | 0€ |
| Asistente de voz | Whisper + Qwen3:8b + TTS local | 0€ |

### 3. Lo que ha cambiado en 18 meses
> 📊 Qwen3:8b ocupa ~5GB y supera a GPT-4o en benchmarks de código. Hace 2 años, modelos equivalentes requerían 32GB+.

## Conclusión

El artículo no se queda en "instala Ollama" — elige un caso de uso y lo construye hasta que es útil. El punto de inflexión ya ocurrió: la línea entre hobby y herramienta de producción se ha movido.

## Tags
`ollama` `local-llm` `qwen3` `rag` `coding-assistant` `tutorial` `offline-ai` `chromadb`
