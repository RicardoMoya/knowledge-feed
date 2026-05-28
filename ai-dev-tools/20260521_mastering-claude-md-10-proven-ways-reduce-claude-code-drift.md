# Mastering CLAUDE.md: 10 Proven Ways to Reduce Claude Code Drift

**Autor:** Mouez Yazidi
**Publicación:** Towards AI
**URL:** [enlace](https://medium.com/towards-artificial-intelligence/mastering-claude-md-10-proven-ways-to-reduce-claude-code-drift-9ec878786246)
**Fecha de resumen:** 2026-05-27
**Categoría:** ai-dev-tools

---

## Contexto y tesis principal

Cuando los equipos tratan `CLAUDE.md` como un README, el modelo recurre a sus valores predeterminados de entrenamiento. El autor propone tratarlo como una **API de comportamiento**: un contrato determinista. Ilustrado con SynapseRAG — plataforma RAG empresarial con latencia p95 < **800 ms**, salidas JSON deterministas y orquestación multiagente.

## Los 10 patrones clave

1. **Máximo 200 líneas** — la atención es memoria de trabajo, no almacenamiento. Eliminar ~30% de reglas mensualmente.
2. **Primeras 30 líneas** establecen el marco cognitivo para todo lo demás.
3. **Separar reglas estrictas** (`never`/`always`/`must`) de preferencias (`prefer`/`avoid`) — nunca mezclarlas.
4. **Antipatrones > instrucciones positivas** — suprimen activamente los valores de entrenamiento por defecto.
5. **Criterios de éxito testeables**: confianza < **0,75** → `insufficient_context`; **cero** citas falso-positivas en CI.
6. **Divulgación progresiva** — enrutamiento a ficheros especializados solo cuando la tarea lo requiere.
7. **CLAUDE.md por subdirectorio** en monorepos — el fichero más cercano gana.
8. **Protocolo de fallback**: similitud top-k < **0,65** → reformulación; nunca fabricar IDs de documentos.
9. **Máximo 3-4 roles** de ingeniería vinculados a tareas concretas.
10. **Versionado como API** + sección `@deprecated` con check de CI.

## Conclusión

Dejar de escribir prompts y empezar a diseñar contratos. Un fichero bien construido transforma a Claude de un sistema que adivina en uno que ejecuta con previsibilidad.
