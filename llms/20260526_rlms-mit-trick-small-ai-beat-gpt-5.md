# RLMs: The MIT Trick That Makes a Small AI Beat GPT-5

**Autor:** Sumit Pandey
**Publicación:** Towards Deep Learning
**URL:** [enlace](https://medium.com/towards-deep-learning/rlms-the-mit-trick-that-makes-a-small-ai-beat-gpt-5-668c7744cda7)
**Fecha de resumen:** 2026-05-27
**Categoría:** llms

---

## Contexto y tesis principal

Los Recursive Language Models (RLMs) son una técnica del MIT (Zhang, Kraska y Khattab, arXiv:2512.24601) que resuelve el "context rot": en lugar de introducir el contexto gigante en el prompt, se almacena como variable Python en un REPL y se deja al modelo escribir código para explorarlo. GPT-5-mini con RLM supera a GPT-5 puro en un **114%** en benchmarks de contexto largo.

## Puntos clave

### 1. El problema que resuelven
La carrera por contextos más grandes (4K → 1M+ tokens) no elimina el "context rot". El efecto "lost in the middle" demuestra que los modelos pierden información enterrada en el centro. RAG y compactación son parches, no soluciones.

### 2. Cómo funciona un RLM
Contexto extenso como variable Python en un REPL. El modelo escribe código para explorarlo (slicing, regex, filtros). Output truncado a ~**8.192 caracteres**, obligando al modelo a ser estratégico. Sub-instancias para razonamiento sobre fragmentos concretos.

### 3. Por qué un modelo pequeño gana al grande
La dificultad no es inteligencia bruta sino no ahogarse. Un modelo modesto con subproblemas limpios y acceso a Python supera a uno más potente reteniéndolo todo. Llamadas cortas (1-2 iteraciones vs. largas cadenas ReAct).

### 4. Resultados empíricos
En OOLONG (**132K tokens**), RLM sobre GPT-5-mini **duplica** los aciertos de GPT-5 puro. minRLM: contexto "ilimitado" con **~3,6x menos tokens** en **6.600+** evaluaciones.

### 5. Advertencias
depth=2+ puede disparar el tiempo de ~**3,6 segundos** a ~**344 segundos**. REPL requiere sandbox aislado. RAG sigue siendo mejor para búsqueda semántica simple con restricciones de latencia.

## Conclusión

RLMs: de lectores pasivos a programadores activos. Comenzar con depth=1 y resistir la tentación de aumentar la recursión.
