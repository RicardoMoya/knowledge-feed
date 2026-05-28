# LLMs Predict Words. LCMs Predict Ideas.
### 🇪🇸 Los LLMs predicen palabras. Los LCMs predicen ideas.

**Autor:** Rohan Mistry
**Publicación:** Towards AI
**URL:** [enlace](https://pub.towardsai.net/llms-predict-words-lcms-predict-ideas-400705d191d4)
**Fecha:** 2026-05-19
**Categoría:** llms
**Fuente:** Medium
**Tags:** `lcm` `large-concept-models` `meta-fair` `sonar` `embeddings` `multimodal` `research` `architecture`

---

## Contexto y tesis principal
En diciembre 2024, Meta FAIR publicó "Large Concept Models: Language Modeling in a Sentence Representation Space". La pregunta central: ¿es la predicción del siguiente token la abstracción incorrecta para el razonamiento? Los LCMs predicen la próxima *idea* (vector de embedding de una frase completa) en lugar del próximo token (fragmento sub-palabra). Los humanos planifican en conceptos — la presentación, los datos, la solución — y las palabras vienen después. Los LCMs intentan modelar ese mismo nivel de abstracción.

## Puntos clave

### 1. Qué es un Large Concept Model
En lugar de predecir tokens, el LCM predice vectores de concepto. El encoder SONAR mapea frases completas a vectores de alta dimensión agnósticos al idioma. El mismo vector representa "The meeting starts at 9" en inglés y en hindi. El LCM opera exclusivamente en ese espacio de conceptos; la decodificación a lenguaje natural es un paso posterior independiente.

Pipeline completo:
1. Texto/voz (cualquier idioma) → SONAR Encoder → vector de concepto
2. LCM Transformer predice el siguiente vector de concepto
3. SONAR Decoder convierte el vector → texto/voz en cualquier idioma destino

### 2. La variante de difusión (Two-Tower LCM)
MSE loss tiene un problema fundamental: asume una única respuesta correcta. Tras "The CEO walked into the room" hay cientos de continuaciones plausibles; MSE fuerza al modelo hacia su promedio semántico. La variante de difusión (idéntica idea a la generación de imágenes) arranca de un embedding ruidoso y lo va refinando en ~10-100 pasos iterativos. El Two-Tower LCM —un tower para codificar contexto, otro para desrumorizar la predicción— es la variante más potente y la arquitectura a seguir.

### 3. Resultados de benchmarks
> 📊 Two-Tower LCM 7B: ROUGE-L competitivo con modelos de summarización fine-tuneados específicamente; resúmenes más abstractivos (menos copia literal); menor repetición; zero-shot cross-lingual robusto.

> 📊 Un documento de 1.000 palabras ≈ 50 conceptos. La atención cuadrática sobre 50 posiciones es ~400× más barata que sobre 1.000 tokens, reduciendo drásticamente el coste de contextos largos.

### 4. Cinco implicaciones prácticas

1. **Contexto largo más barato**: reducción ~20× en longitud de secuencia → ventanas de millón de conceptos como objetivo realista de ingeniería, no como moonshot.
2. **Planificación jerárquica para agentes**: el espacio de conceptos permite razonamiento en múltiples niveles (objetivo → subobjetivos → pasos individuales) de forma más natural que el prompting actual.
3. **Multilingüe nativo sin sesgo**: SONAR soporta 200 idiomas en texto y 76 en voz. Los idiomas no-ingleses tokenizan igual de eficientemente porque el razonamiento ocurre en espacio de conceptos, no de tokens.
4. **Sistemas voice-native**: si el decoder soporta audio, el mismo modelo razona igual sobre texto y voz —la modalidad es solo la superficie, el concepto es el mismo.
5. **Representación como habilidad central**: si el razonamiento ocurre en espacio de embeddings semánticos, las habilidades de representation learning y vector retrieval se vuelven más centrales, no menos.

### 5. Limitaciones honestas
- La granularidad del concepto es arbitraria: ¿por qué una frase y no una cláusula o un párrafo? No hay fundamento teórico.
- SONAR está congelado durante el entrenamiento del LCM. Entrenamiento conjunto podría mejorar sustancialmente el rendimiento pero no se ha intentado aún.
- No hay entrenamiento a escala frontier (el modelo es 7B como prueba de concepto).
- La inferencia por difusión añade latencia: múltiples pasos de denoising por concepto.
- Las métricas de fluency están por debajo de LLMs —incluso texto humano puntúa más bajo que LLMs en fluency automática, lo que dice más sobre qué han memorizado los LLMs que sobre calidad real.

## Datos relevantes

| Aspecto | LLM (tokens) | LCM (conceptos) |
|---------|-------------|----------------|
| Unidad de predicción | Sub-palabra (~4 chars) | Frase completa |
| Secuencia para 1.000 palabras | ~1.000 tokens | ~50 conceptos |
| Coste de atención | O(n²) sobre ~1.000 | O(n²) sobre ~50 |
| Sesgo de idioma | Alto (inglés dominante) | Ninguno (SONAR agnóstico) |
| Soporte de idiomas | Variable | 200 texto / 76 voz |

## Conclusión
Los LCMs no reemplazarán a los LLMs el año que viene —la brecha de escala de entrenamiento es enorme— pero la dirección de investigación es clara: Meta apuesta consistentemente por predicción en espacio de representación abstracta (I-JEPA, V-JEPA, LCMs). Los transformers siguen siendo el motor interno. Lo que cambia es la unidad de representación: de sub-palabra a concepto. Los cambios más grandes en IA suelen empezar como papers ignorados antes de que el campo se dé cuenta de que la abstracción misma ha cambiado.

## Tags
`lcm` `large-concept-models` `meta-fair` `sonar` `embeddings` `multimodal` `research` `architecture`
