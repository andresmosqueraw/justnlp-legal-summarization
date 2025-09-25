## carpeta papers-referencia contenido

He revisado los cuatro papers que me compartiste y aquí te explico qué hace cada uno:

---

### 1. **IL-TUR: Benchmark for Indian Legal Text Understanding and Reasoning (ACL 2024)**

Este paper **presenta un benchmark llamado IL-TUR** para procesamiento de lenguaje natural en el dominio legal de la India.

* Contiene **8 tareas** en inglés y 9 lenguas indias: reconocimiento de entidades legales (L-NER), predicción de roles retóricos, predicción de juicios con explicación, predicción de fianza, identificación de estatutos legales, recuperación de casos previos, resumen de documentos legales y traducción automática legal.
* Su objetivo es **estandarizar datasets y métricas**, ofrecer **baselines (incluyendo LLMs)** y un **leaderboard público**.
* La motivación principal: los documentos legales en India son largos, multilingües, poco estructurados, y su análisis manual contribuye al enorme atraso judicial (más de 40 millones de casos).
* Aporta un **recurso de referencia para comparar modelos legales-NLP**.

---

### 2. **A Comprehensive Analysis of Indian Legal Documents Summarization Techniques (SN Computer Science, 2023)**

Este trabajo es un **artículo de revisión y experimentación comparativa** sobre técnicas de resumen automático de documentos legales indios.

* Analiza **7 modelos de resumen**: BART, LexRank, TextRank, Luhn, LSA, Legal Pegasus y Longformer.
* Usan juicios obtenidos de **Indian Kanoon**.
* Concluyen que **Legal Pegasus** obtiene mejor rendimiento que los demás en la tarea de resumen legal.
* Explica la diferencia entre métodos **extractivos** (seleccionan oraciones originales) y **abstractive** (generan nuevo texto).
* Aporta un **estado del arte comparativo** que muestra fortalezas y debilidades de cada técnica en el contexto legal indio.

---

### 3. **Legal Case Document Summarization: Extractive and Abstractive Methods and their Evaluation (AACL 2022)**【10:2022.aacl-main.77.pdf】

Este paper **estudia sistemáticamente cómo rinden distintos métodos de resumen (extractivos vs. abstractive)** en documentos legales.

* Crean **tres datasets**:

  * **IN-Abs**: 7,130 juicios de la Corte Suprema India con resúmenes “headnotes”.
  * **IN-Ext**: 50 documentos con resúmenes extractivos segmentados por roles retóricos (hechos, argumentos, precedentes, etc.).
  * **UK-Abs**: 793 juicios del Reino Unido con resúmenes oficiales.
* Evalúan métodos extractivos (LexRank, SummaRunner, BERTSum), abstractive (BART, Pegasus, Longformer) y específicos para el dominio legal (CaseSummarizer, Gist).
* Hallazgos:

  * **Los métodos generales a veces superan a los específicos del dominio**.
  * Para textos largos, **estrategias de chunking** funcionan mejor que usar modelos abstractive directamente.
  * **Los abogados prefieren resúmenes extractivos** aunque los automáticos den mejores métricas ROUGE.
* Aporta datasets y un **análisis profundo de qué funciona mejor en documentos legales largos**.

---

### 4. **MILDSum: A Novel Benchmark Dataset for Multilingual Summarization of Indian Legal Case Judgments (EMNLP 2023)**【11:2023.emnlp-main.321.pdf】

Este paper introduce **MILDSum**, el primer dataset de resúmenes legales bilingües (inglés–hindi).

* Consta de **3,122 juicios de cortes indias** en inglés, con resúmenes en inglés e hindi elaborados por abogados.
* Fuente: artículos de **LiveLaw**, sitio legal que publica resúmenes profesionales en ambos idiomas.
* Proponen dos enfoques:

  1. **Summarize-then-translate** (resumir en inglés y luego traducir al hindi).
  2. **Cross-lingual summarization directa** (generar resumen en hindi a partir del documento en inglés).
* Resultados: la estrategia **resumir y luego traducir rinde mejor** que el resumen directo bilingüe, aunque introduce errores de traducción.
* El dataset también sirve para **entrenar/traducir en el dominio legal indio**.
* Contribución: **primer recurso multilingüe de resúmenes legales en India**, que busca mejorar el acceso a la justicia en lenguas locales.

---

📌 En resumen:

* **IL-TUR (2024)** → Benchmark amplio para múltiples tareas legales (no solo resumen).
* **Comprehensive Analysis (2023)** → Revisión y comparación de modelos de resumen en India.
* **Legal Case Summarization (2022)** → Estudio sistemático de extractivo vs. abstractive con nuevos datasets.
* **MILDSum (2023)** → Primer dataset bilingüe (inglés–hindi) para resúmenes legales indios.



## 🔹 1. Cómo se conectan los papers con el Shared Task

* **IL-TUR (ACL 2024)** → Define benchmarks legales amplios (incluyendo summarization). Inspira a crear datasets estandarizados como **InLSum** para este shared task.
* **Legal Case Summarization (AACL 2022)** → Demuestra qué métodos funcionan mejor en juicios indios largos (extractivo, chunking, supervisado vs. no supervisado). Esto orienta las **estrategias de los equipos** en la competencia.
* **Comprehensive Analysis (SN Computer Science 2023)** → Compara modelos (BART, Pegasus, Longformer) y concluye que **Legal Pegasus rinde mejor**. Esto sirve como **baseline esperado** para los participantes.
* **MILDSum (EMNLP 2023)** → Introduce el multilingüe (inglés–hindi). Aunque el shared task es solo en inglés, marca la **evolución futura hacia multilingüe**.

👉 En conjunto, los papers muestran la progresión natural: **de benchmarks → comparación de técnicas → evaluación sistemática → multilingüe**, y este shared task se ubica justo en esa línea, pero focalizado en **resumen abstractive de juicios indios en inglés**.

---

## 🔹 2. Flujo del Shared Task (L-SUMM)

1. **Tarea**

   * Generar resúmenes abstractive (~500 palabras) de juicios indios en inglés.

2. **Dataset (InLSum)**

   * 3 splits:

     * **Train**: 1200 documentos.
     * **Validation**: 200 documentos (para tuning y evaluación intermedia).
     * **Test**: 400 documentos (para evaluación final).
   * Formato: JSONL con archivos de juicios (`<split>_judg.jsonl`) y resúmenes de referencia (`<split>_ref_summary.jsonl`).

3. **Fases de la competencia**

   * **Fase 1: Training & Validation** → entrenar modelos en train y predecir summaries para validation.
   * **Fase 2: Testing** → predecir summaries en el test (oculto).

4. **Entrega**

   * Archivo ZIP con un JSONL (`answer.jsonl`) que tenga `ID` y `Summary`.
   * Ejemplo:

     ```json
     {"ID": "id_100", "Summary": "The Delhi High Court recently said that it hopes and ..."}
     ```

5. **Evaluación**

   * **Métricas**: ROUGE-2, ROUGE-L, BLEU (escaladas a 0–100).
   * **Ranking**: combinación de las tres.

---

## 🔹 3. Conexión con métricas (ejemplo intuitivo)

* **ROUGE** → hecho para **summarization**, mide **recall** (cuánto del contenido de referencia aparece en el resumen generado).
* **BLEU** → hecho para **translation**, mide **precision** (cuántos n-grams del resumen generado coinciden con la referencia).
* **ROUGE-L** → usa subsecuencia más larga común, combina precision + recall.

Ejemplo simplificado:

* Referencia: *“The way to make people trustworthy is to trust them.”*
* Hipótesis: *“To make people trustworthy, you need to trust them.”*
* Ambos miden solapamiento de palabras/frases, pero BLEU lo hace desde precisión y ROUGE desde recall.

---

## 🔹 4. Sugerencia de cómo presentar

* **Intro** → Explicar que el Shared Task sigue la línea de avances previos (los papers).
* **Dataset & Fases** → Mostrar el esquema InLSum (train/val/test).
* **Métricas** → Explicar BLEU vs ROUGE con un ejemplo sencillo.
* **Conexión** → Mostrar que los resultados esperados (por ejemplo, que Pegasus o BART rinden bien) ya tienen base en papers previos.