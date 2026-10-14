---
language:
- en
- es  # [VERIFICAR: Modelo base preentrenado primordialmente en inglés; evaluado en español mediante prompting en el demo]
license: apache-2.0  # Declarada en el repositorio oficial de Hugging Face. Sujeta a licencias heredadas [VERIFICAR]
tags:
- remote-sensing
- vision-language
- multimodal
- teledeteccion
- vqa
- visual-grounding
- scene-classification
- earth-observation
- colab-demo
- 4-bit-quantization
datasets:
- MBZUAI/GeoChat_Instruct
base_model:
- lmsys/vicuna-7b-v1.5
pipeline_tag: image-text-to-text
---

# Ficha del Modelo (Model Card): GeoChat (Adaptación en Google Colab)

Esta ficha de modelo documenta **GeoChat**, el primer Modelo de Lenguaje Visual de Gran Escala (LVLM) adaptado y especializado en teledetección (*Remote Sensing*). Sigue la estructura estandarizada propuesta por **Mitchell et al. (2019)** (*Model Cards for Model Reporting*) y las directrices de la plantilla oficial de **Hugging Face Hub**.

---

## 1. Detalles del Modelo (Model Details)

### 1.1 Información Básica
- **Nombre del modelo:** GeoChat (identificador del repositorio en Hugging Face: [`MBZUAI/geochat-7B`](https://huggingface.co/MBZUAI/geochat-7B)).
- **Versión:** 1.0 (Publicado formalmente en **CVPR 2024**; pre-print en arXiv en noviembre de 2023).
- **Tipo de modelo:** Modelo de Lenguaje Visual Multimodal (LVLM / Large Vision-Language Model) ajustado por instrucciones para imágenes satelitales y aéreas.
- **Fecha de publicación original:** Noviembre de 2023 (arXiv:2311.15826) / Junio de 2024 (CVPR 2024).
- **Fecha de esta ficha y adaptación:** 14 de octubre de 2026.

### 1.2 ¿Qué puede hacer el modelo? (Capacidades Fundamentales)

Más allá de los detalles de arquitectura, el valor primordial de **GeoChat** radica en sus capacidades multifuncionales para el análisis e interpretación visual del territorio:

| Tarea | Descripción |
|---|---|
| 🏷️ Clasificación de escena | Identificar el tipo de zona (urbana, rural, industrial...) |
| 📝 Descripción de imagen | Generar una descripción detallada de la imagen satelital |
| ❓ VQA (Visual Question Answering) | Responder preguntas sobre el contenido de la imagen |
| 📍 Grounding visual | Localizar objetos mencionados con coordenadas |
| 🗣️ Conversación | Mantener un diálogo sobre la imagen |

### 1.3 Autoría y Créditos
- **Desarrollado por (modelo original):**  
  Kartik Kuckreja, Muhammad Sohail Danish, Muzammal Naseer, Abhijit Das, Salman Khan y Fahad Shahbaz Khan.  
  *Institución:* Mohamed bin Zayed University of Artificial Intelligence (MBZUAI), Abu Dabi, Emiratos Árabes Unidos.
- **Adaptado y documentado por:**  
  **Diana Katherin Avendaño Rojas** (Código estudiantil: 20211007063).  
  *Institución:* Universidad Distrital Francisco José de Caldas.  
  *Curso:* Inteligencia Artificial Aplicada al Territorio.  
  *Contacto:* `dkavendanor@udistrital.edu.co` | GitHub: [`diana-rojas2003`](https://github.com/diana-rojas2003).

### 1.4 Recursos y Enlaces Oficiales
- **Artículo científico (Paper):** [GeoChat: Grounded Large Vision-Language Model for Remote Sensing (CVPR 2024)](https://arxiv.org/abs/2311.15826)
- **Repositorio de código fuente original:** [mbzuai-oryx/GeoChat en GitHub](https://github.com/mbzuai-oryx/GeoChat)
- **Pesos preentrenados oficiales:** [MBZUAI/geochat-7B en Hugging Face](https://huggingface.co/MBZUAI/geochat-7B)
- **Dataset oficial de instrucciones:** [MBZUAI/GeoChat_Instruct en Hugging Face](https://huggingface.co/datasets/MBZUAI/GeoChat_Instruct)

### 1.5 Arquitectura Base
GeoChat se basa en la arquitectura de **LLaVA-1.5** adaptada para teledetección mediante:
1. **Encoder Visual:** CLIP ViT-L/14 (`openai/clip-vit-large-patch14-336`), adaptado con interpolación posicional para operar a alta resolución (504×504 píxeles con parches de 14×14).
2. **Proyector Multimodal:** Conector MLP de dos capas con funciones de activación no lineal GELU, encargado de alinear espacial y semánticamente los tokens visuales con el espacio latente del modelo de lenguaje.
3. **Modelo de Lenguaje Autorregresivo (LLM):** Vicuna-7B (versión v1.5), un modelo ajustado a partir de LLaMA-2 de Meta con 7 mil millones de parámetros.

### 1.6 Licenciamiento y Estado Legal 
- **Licencia declarada en Hugging Face:** Apache-2.0 por MBZUAI.
- **Licencias heredadas y restricciones :**  
  Aunque el repositorio declara Apache-2.0, el modelo hereda componentes que imponen restricciones legales estrictas:
  - **Vicuna-7B v1.5:** Se deriva de los pesos de **LLaMA-2** de Meta, sujeto al *LLaMA 2 Community License Agreement* (que impone políticas de uso aceptable y restricciones si se superan 700 millones de usuarios activos mensuales).
  - **Datos de ShareGPT:** Vicuna fue ajustado con conversaciones extraídas de ShareGPT generadas por modelos de OpenAI, lo que infringe los términos comerciales de OpenAI respecto al entrenamiento de modelos competidores.
  - **Datasets de teledetección fuente:** Múltiples conjuntos de datos utilizados para compilar `GeoChat_Instruct` (e.g., DIOR, NWPU-RESISC45) restringen su uso a fines exclusivamente académicos y no comerciales.
  > **Advertencia de cumplimiento:** Para cualquier uso comercial o redistribución externa, el licenciamiento debe marcarse como **`[VERIFICAR]`** para asegurar la compatibilidad con las licencias de LLaMA-2 y los datasets constitutivos.

---

## 2. Alcance de esta Ficha (Scope of this Card)

Esta ficha describe el modelo preentrenado **GeoChat** publicado por MBZUAI y su **ejecución técnica experimental en Google Colab** realizada por Diana Katherin Avendaño Rojas.

### Delimitación Clara del Trabajo:
1. **Sin reentrenamiento:** La autora de la adaptación **NO reentrenó** los pesos del modelo ni modificó los tensores base originales.
2. **Entorno de ejecución y optimización:** El trabajo consistió en el diseño de parches de ingeniería de software para resolver incompatibilidades con versiones modernas de librerías en Python/Colab, la cuantización a 4 bits para permitir su ejecución en una GPU de recursos limitados (Tesla T4 de 16 GB VRAM) y la validación funcional sobre una imagen satelital real con prompting bilingüe (inglés/español).
3. **Efecto de la cuantización (4 bits vs FP16 original):**
   - El modelo original fue evaluado por los autores en media precisión (FP16/BF16), requiriendo más de 14–16 GB de VRAM solo para los pesos del modelo.
   - En este demo, la cuantización a 4 bits (`bitsandbytes`, NF4) reduce el uso de VRAM a **~4.5 GB**, democratizando su uso en entornos gratuitos o de investigación estudiantil.
   - **Impacto en el desempeño:** La cuantización a 4 bits puede ocasionar degradaciones menores en la precisión métrica de las coordenadas de *grounding*, ligeras disminuciones en la exactitud de conteo (*counting accuracy*) y respuestas textualmente más sintéticas en comparación con las reportadas en el paper de CVPR 2024.

---

## 3. Modificaciones Realizadas (Modifications & Colab Implementation)

**Autora de las modificaciones:** Diana Katherin Avendaño Rojas.

Para lograr la ejecución exitosa de GeoChat en Google Colab en el año 2026 frente al ecosistema actual de Hugging Face y PyTorch, se aplicaron las siguientes modificaciones y parches de código:

### 3.1 Infraestructura y Cuantización
- **Hardware de ejecución:** Google Colab con acelerador gráfico Nvidia Tesla T4 (15.8 GB VRAM, arquitectura Turing, cómputo CUDA 7.5).
- **Cuantización en 4 bits:** Se forzó la carga mediante `bitsandbytes` (`load_4bit=True`, `load_8bit=False`), reduciendo la huella de memoria GPU de ~14 GB a **~4.5 GB VRAM**, dejando margen suficiente para la activación de tensores visuales e inferencia.

### 3.2 Parches de Compatibilidad con Versiones Modernas de Transformers
El código base de GeoChat fue desarrollado a finales de 2023 sobre versiones obsoletas de librerías (`transformers`, `accelerate`, `torch`). Se parchearon dinámicamente cuatro archivos clave:

1. **Parche en `hf_prefixlm_converter.py`:**
   - *Problema:* Las versiones modernas de `transformers` removieron las funciones privadas `_expand_mask` y `_make_causal_mask` del módulo `transformers.models.bloom.modeling_bloom`, generando un error crítico de importación (`ImportError`).
   - *Solución:* Se suprimieron mediante expresiones regulares (`re.sub`) todas las líneas asociadas a estas funciones obsoletas, desacoplando el convertidor MPT innecesario para la inferencia de Vicuna.
2. **Parche en `builder.py`:**
   - *Problema:* La función `load_pretrained_model` propagaba parámetros no admitidos (`load_in_4bit`, `load_in_8bit`) directamente a llamadas internas de constructores que lanzaban excepciones de tipo `unexpected keyword argument`.
   - *Solución:* Se filtraron los `kwargs` en tiempo de ejecución:
     ```python
     **{k: v for k, v in kwargs.items() if k not in ['load_in_4bit', 'load_in_8bit']}
     ```
3. **Parche en `clip_encoder.py`:**
   - *Problema 1 (AttributeError):* La estructura interna de `CLIPVisionModel` en versiones contemporáneas ya no aloja los embeddings bajo `.vision_model.embeddings`, sino directamente bajo `.embeddings`.
   - *Problema 2 (Carga diferida e interpolación):* Se presentaban conflictos de sangría y errores de dimensionamiento de embeddings de posición cuando `delay_load=False`.
   - *Solución:* Se corrigió el acceso al atributo de embeddings (`.replace('.vision_model.embeddings', '.embeddings')`), se condicionó la interpolación de posiciones visuales a `delay_load` con soporte para `image_size=504, patch_size=14`, y se reescribió el método `load_model` para inicializar y adaptar explícitamente `CLIPVisionModel` al dispositivo y tipo de dato correspondiente.
4. **Parche en `geochat_arch.py` (Error de `DynamicCache`):**
   - *Problema:* En versiones modernas de `transformers`, el parámetro `past_key_values` ya no es una tupla estática de tuplas sino un objeto estructurado `DynamicCache`. La instrucción original `past_key_values[-1][-1].shape[-2]` generaba fallos fatales de tipo `TypeError` o `IndexError`.
   - *Solución:* Se inyectó una función auxiliar dinámica `_get_cache_len(cache)` que detecta polimórficamente si el objeto cuenta con el método `.get_seq_length()`, atributos `.key_cache` o formato de tupla tradicional:
     ```python
     def _get_cache_len(cache):
         if isinstance(cache, tuple): return cache[-1][-1].shape[-2]
         if hasattr(cache, 'get_seq_length'): return cache.get_seq_length()
         if hasattr(cache, 'key_cache'): return cache.key_cache[0].shape[-2]
         if hasattr(cache, 'keys'): return cache.keys[0].shape[-2] if isinstance(cache.keys, list) else cache.keys.shape[-2]
         return 0
     ```

### 3.3 Función de Inferencia `preguntar_geochat`
Se diseñó y configuró la rutina de inferencia con los siguientes hiperparámetros y transformaciones:
- **Preprocesamiento visual:** Redimensionamiento y recorte forzado a **504×504 píxeles**:
  ```python
  image_tensor = image_processor.preprocess(
      imagen_pil,
      return_tensors='pt',
      size={"shortest_edge": 504},
      crop_size={"height": 504, "width": 504}
  )['pixel_values'].half().cuda()
  ```
- **Plantilla de conversación:** `conv_templates["llava_v1"]` con token delimitador `<image>\n`.
- **Estrategia de decodificación y muestreo:**
  - `do_sample = True`
  - `temperature = 0.2` (temperatura baja para minimizar alucinaciones y favorecer precisión objetiva).
  - `top_p = 0.9` (muestreo por núcleo/nucleus sampling).
  - `max_new_tokens = 512`
  - `use_cache = False` (desactivación explícita del caché autorregresivo durante `model.generate` para evitar inconsistencias de tensores en bucles de muestreo).

### 3.4 Caso de Prueba del Demo
- **Entrada:** Imagen satelital propia cargada desde Google Drive (`/content/drive/MyDrive/Geochat/satelite.png`), resolución nativa de **852 × 420 píxeles**.
- **Estrategia de prompting:** Instrucción redactada en inglés solicitando explícitamente la respuesta completa en idioma español:  
  `"Describe this satellite image in detail. Please output your response completely in Spanish."`
- **Respuesta real obtenida en el demo:**  
  > *"En la imagen satelital, se pueden observar varios edificios y una calle en un entorno urbanizado. Los edificios se encuentran en diferentes partes de la imagen, con algunos en el centro y otros en los lados. La calle se encuentra en la parte inferior de la imagen, con algunos edificios cercanos a ella. En general, la imagen presenta una vista panorámica de un área urbana con varios edificios y una calle principal."*

---

## 4. Uso Previsto (Intended Use)

### 4.1 Usos Previstos Directos (Primary Intended Uses)
- **Análisis conversacional multimodal del territorio:** Interrogación en lenguaje natural sobre escenas aéreas y satelitales (VQA) para identificar clases de suelo, infraestructuras, vegetación y redes viales.
- **Descripción detallada de escenas (Captioning):** Generación automática de reportes textuales a nivel de escena completa o regiones acotadas (*region captioning*).
- **Clasificación de escenas (Zero-shot Scene Classification):** Identificación automática del uso y cobertura del suelo (urbano, agrícola, forestal, industrial, cuerpos de agua).
- **Grounding visual y detección referida (Referring Expression Comprehension):** Localización espacial de entidades mencionadas en el diálogo, retornando coordenadas de cajas delimitadoras normalizadas (`[ymin, xmin, ymax, xmax]`).
- **Herramienta pedagógica y académica:** Apoyo a asignaturas de teledetección, geomática, ordenamiento territorial e inteligencia artificial aplicada al territorio.

### 4.2 Usuarios Previstos (Intended Users)
- Investigadores, docentes y estudiantes de ingeniería catastral, geodesia, geografía, ingeniería ambiental y ciencia de datos geoespaciales.
- Planificadores urbanos y territoriales que requieran asistencia para el filtrado rápido de información visual satelital.

### 4.3 Usos Fuera de Alcance y Usos Prohibidos (Out-of-Scope / Prohibited Uses)
- **Navegación y objetivos militares autónomos:** Guiado de municiones, aeronaves no tripuladas o sistemas de combate en tiempo real sin supervisión humana.
- **Atención de emergencias en tiempo real sin validación humana:** El modelo no debe ser la única fuente de decisión para despliegue de rescatistas o evacuaciones en desastres naturales, dada la presencia potencial de alucinaciones.
- **Vigilancia e identificación de personas:** El modelo no cuenta con resolución ni entrenamiento para reconocer individuos, placas de vehículos ni violar la privacidad ciudadana.
- **Decisiones catastrales legales automatizadas:** La asignación de límites prediales, avalúos o sanciones jurídicas requiere validación pericial de ingenieros y no puede delegarse ciegamente al modelo.

---

## 5. Factores (Factors)

El rendimiento de GeoChat está condicionado por múltiples factores intrínsecos al dominio geoespacial:

- **Resolución espacial (GSD - Ground Sample Distance):** El modelo exhibe mejor rendimiento en imágenes de muy alta resolución (VHR, 0.1 m a 1 m/px) que en sensores de media y baja resolución (e.g., Sentinel-2 a 10 m/px o Landsat a 30 m/px), donde los objetos menores a un píxel son invisibles.
- **Ángulo de toma y geometría de adquisición:** Optimizado primordialmente para tomas ortorrectificadas cenitales (nadir). Las tomas muy oblicuas o con deformaciones de perspectiva aérea pueden confundir el detector de relaciones espaciales.
- **Condiciones atmosféricas e iluminación:** Nubosidad, sombras pronunciadas por relieve o edificaciones altas, y reflectancia solar en cuerpos de agua pueden degradar la interpretación.
- **Resolución espectral:** El modelo opera exclusivamente en bandas visibles **RGB**. No procesa directamente bandas infrarrojas (NIR, SWIR) ni polarizaciones de radar (SAR).
- **Factores lingüísticos:** El modelo fue entrenado en inglés. Consultas formuladas en español o solicitudes de salida en idiomas distintos al inglés dependen de las habilidades de traducción latentes de Vicuna-7B, lo que puede introducir errores léxicos o sintácticos menores.

---

## 6. Métricas (Metrics)

Las métricas empleadas en la evaluación científica del modelo original (Kuckreja et al., CVPR 2024) varían según la tarea:

- **Clasificación de escenas:** Exactitud Top-1 (*Top-1 Accuracy, %*).
- **Visual Question Answering (VQA):** Exactitud de coincidencia exacta (*Exact Match / VQA Accuracy, %*), desglosada por tipología de pregunta:
  - *Presence:* Existencia o no de un elemento.
  - *Comparison:* Comparación cuantitativa o relacional entre dos entidades.
  - *Count:* Conteo de instancias visibles.
- **Descripción de imágenes (Image & Region Captioning):** Métricas de generación de lenguaje natural evaluadas contra referencias humanas:
  - **BLEU-4:** Precisión de n-gramas.
  - **METEOR:** Coincidencia armónica considerando sinónimos y lematización.
  - **ROUGE-L:** Coincidencia de la subsecuencia común más larga.
  - **CIDEr:** Consistencia de consenso informacional ponderado por TF-IDF.
- **Grounding Visual / Comprensión de Expresiones Referidas (REC):** Exactitud de intersección sobre unión (IoU) con umbral $\ge 0.5$ (**Acc@0.5**).

---

## 7. Datos de Evaluación (Evaluation Data)

### 7.1 Datasets de Referencia en el Paper Original
- **UCMerced Land Use:** 2,100 ortofotos aéreas (256×256 px, 0.3 m/px) distribuidas en 21 clases de uso de suelo.
- **AID (Aerial Image Dataset):** 10,000 imágenes aéreas en 30 clases temáticas con resoluciones espaciales de 0.5 a 8 metros.
- **RSVQA-HRBEN:** Benchmark de VQA de alta resolución basado en ortofotos aéreas de Países Bajos (resolución de 15 cm), con ~10.6 millones de preguntas en subconjuntos de presencia, comparación y conteo.
- **RSVQA-LRBEN:** Benchmark de VQA de baja resolución derivado de Sentinel-2 BigEarthNet (10 m/px), compuesto por 72,825 imágenes y miles de pares de preguntas/respuestas.
- **Sydney-Captions y UCM-Captions:** Datasets de imágenes con descripciones textuales redactadas por expertos en teledetección.
- **DIOR y DOTA:** Conjuntos de datos de detección y localización de objetos terrestres de alta densidad (puertos, aeropuertos, barcos, presas, vehículos).

### 7.2 Datos de Evaluación en el Demo Colab
- **Imagen satelital propia:** Archivo `satelite.png` (852 × 420 píxeles, RGB), correspondiente a una vista aérea urbana de edificaciones y vías, empleada para la validación cualitativa en el entorno Google Colab.

---

## 8. Datos de Entrenamiento (Training Data)

- **Dataset:** `MBZUAI/GeoChat_Instruct` ([Hugging Face](https://huggingface.co/datasets/MBZUAI/GeoChat_Instruct)).
- **Volumen:** Aproximadamente **318,000 pares de instrucciones y respuestas multimodales**.
- **Generación:** Construido mediante un pipeline automatizado propuesto por los autores de MBZUAI utilizando **Vicuna-v1.5** para transformar anotaciones tabulares y espaciales existentes en diálogos fluidos de teledetección.
- **Fuentes integradas:** Amalgama curada de datasets abiertos consolidados: UCMerced, AID, NWPU-RESISC45, PatternNet, RSVQA (HR/LR), RSIVQA, Sydney-Captions, UCM-Captions y DIOR.
*(Para un desglose pormenorizado, consúltese la [Ficha del Conjunto de Datos (DATASET_CARD.md)](./DATASET_CARD.md)).*

---

## 9. Análisis Cuantitativos (Quantitative Analyses)

A continuación se resumen los resultados cuantitativos reportados en el artículo de **CVPR 2024** (*Kuckreja et al.*), verificados directamente contra las tablas publicadas del paper:

### 9.1 Clasificación de Escenas Zero-Shot (Tabla 5 del Paper)
Comparación de exactitud (%) frente a modelos fundacionales generalistas y especializados:

| Modelo | UCMerced (%) | AID (%) |
| :--- | :---: | :---: |
| Qwen-VL | 62.90 | 52.60 |
| MiniGPT-v2 | 4.76 | 12.90 |
| LLaVA-1.5 (7B) | 68.00 | 51.00 |
| **GeoChat (Ours)** | **84.43** | **72.03** |

> **Hallazgo:** GeoChat supera a su modelo base LLaVA-1.5 por más de **+16.4 %** en UCMerced y más de **+21.0 %** en AID, demostrando que el ajuste por instrucciones especializado en teledetección es indispensable frente a los VLMs de propósito general.

### 9.2 Visual Question Answering Zero-Shot en RSVQA-HRBEN (Tabla 8 del Paper)
Exactitud de respuesta desglosada por categoría temática:

| Modelo | Presencia (*Presence*) | Comparación (*Comparison*) | Promedio General |
| :--- | :---: | :---: | :---: |
| Qwen-VL | 66.44 % | 60.41 % | 63.06 % |
| MiniGPT-v2 | 40.79 % | 50.91 % | 46.46 % |
| LLaVA-1.5 (7B) | **69.83 %** | 67.29 % | 68.40 % |
| **GeoChat (Ours)** | 58.45 % | **83.19 %** | **72.30 %** |

> **Hallazgo:** GeoChat aventaja ampliamente a los modelos genéricos en preguntas de comparación territorial (+15.9 % sobre LLaVA-1.5), consolidando un promedio general superior del **72.30 %**.

### 9.3 Desempeño en RSVQA-LRBEN y Grounding
- En **RSVQA-LRBEN**, GeoChat compite directamente con arquitecturas entrenadas exclusivamente bajo supervisión cerrada para dicha tarea (como RSGPT), superando con holgura a cualquier modelo zero-shot no adaptado a teledetección.
- En tareas de **grounding visual (REC)** sobre el dataset DIOR, GeoChat logra predecir con precisión cajas delimitadoras en coordenadas `[ymin, xmin, ymax, xmax]`, permitiendo la localización simultánea de entidades dentro del flujo conversacional.

---

## 10. Consideraciones Éticas (Ethical Considerations)

- **Vigilancia e invasión de la privacidad:** El uso de modelos avanzados de visión sobre sensores de muy alta resolución plantea inquietudes sobre la monitorización invasiva de predios privados, movimientos cotidianos y actividades comunitarias.
- **Geopolítica y asimetría de datos:** La mayoría de los datasets de entrenamiento provienen de sensores y plataformas gubernamentales del hemisferio norte. Esto genera una asimetría estructural: el modelo tiene menor comprensión de dinámicas de ocupación informal, tipologías constructivas autóctonas o coberturas de países en desarrollo (como Colombia o la región andina).
- **Riesgo de uso militar indebido:** La teledetección automatizada con reconocimiento de infraestructura (pistas, silos, puentes, puertos) conlleva riesgos intrínsecos de doble uso militar si se acopla a cadenas de ataque no autorizadas.

---

## 11. Limitaciones y Recomendaciones (Limitations & Recommendations)

### 11.1 Inconsistencia Técnica Detectada entre la Documentación del Notebook y el Código Real
> [!WARNING]
> **Inconsistencia Crítica Identificada:**
> - **En el texto explicativo del notebook (Sección conceptual / Resumen):** Se señala textualmente que el encoder visual CLIP ViT-L/14 *"Redimensiona a 336x336 y extrae 576 tokens visuales"*.
> - **En la ejecución real del código (Celdas de parche e inferencia):** El código implementado en `clip_encoder.py` y `preguntar_geochat` **fuerza la resolución a 504×504 píxeles**:
>   ```python
>   self.clip_interpolate_embeddings(image_size=504, patch_size=14)
>   size = {"shortest_edge": 504}
>   crop_size = {"height": 504, "width": 504}
>   ```
> - **Explicación matemática:** Con parches de 14×14 píxeles, una imagen de 336×336 produce $(336/14) \times (336/14) = 24 \times 24 = \mathbf{576}$ tokens visuales. Sin embargo, al forzar la resolución a 504×504, la matriz resultante es de $(504/14) \times (504/14) = 36 \times 36 = \mathbf{1296}$ tokens visuales.
> - **Documentación de la realidad del código:** Para efectos de reproducibilidad, se documenta que **el sistema en ejecución real procesa 1,296 tokens visuales a una resolución de 504×504 píxeles**, desmintiendo el texto conceptual preliminar del notebook.

### 11.2 Limitaciones de la Cuantización a 4 bits
- La compresión de pesos con `bitsandbytes` (NF4) induce pérdida de precisión en los pesos del proyector MLP y las capas de atención del LLM. Aunque la semántica de alto nivel se conserva intacta, la precisión en regresión numérica de coordenadas espaciales (*bounding boxes*) y el conteo de elementos densos (e.g. número exacto de autos o casas) pueden presentar un margen de error mayor que el reportado en FP16.

### 11.3 Alucinaciones y Conteo
- Como todo modelo derivado de LLMs autorregresivos, GeoChat puede sufrir de alucinación si se le induce con preguntas capciosas (e.g., preguntar *"¿Cuántos aviones hay?"* en una imagen donde no hay aeronaves, pudiendo inducir una respuesta afirmativa errónea). Se recomienda verificar visualmente siempre las respuestas críticas.

### 11.4 Recomendaciones de Uso
- Emplear siempre una temperatura baja ($\le 0.2$) para tareas analíticas territoriales.
- Mantener la relación de aspecto de las imágenes o evitar deformaciones extremas antes de alimentar el recorte de 504×504.
- Para aplicaciones en idiomas distintos al inglés, formular las directrices con claridad mediante prompts estructurados (como se demostró en el demo: prompt en inglés pidiendo la salida en español).

---

## 12. Sesgos y Riesgos (Biases & Risks)

- **Sesgo de Dominio Geográfico:** El dataset `GeoChat_Instruct` fue sintetizado a partir de conjuntos de datos estandarizados mayormente occidentales y asiáticos. Estructuras territoriales específicas de Latinoamérica (e.g., minifundios irregulares, asentamientos informales de ladera, cubiertas de zinc/teja rústica) pueden clasificarse genéricamente como *"urban"* o *"rural"* sin suficiente granularidad semántica.
- **Sesgo de Idioma:** Dado que el ajuste por instrucciones se hizo al 100% en inglés, el razonamiento profundo en español no está garantizado para conceptos cartográficos especializados (e.g. "desenglobe", "servidumbre", "páramo").

---

## 13. Cómo Empezar (How to Get Started)

A continuación se presenta el código mínimo reproducible, derivado de las modificaciones funcionales realizadas en Google Colab:

```python
import os
import pathlib
import re
import sys
import torch
from PIL import Image

# 1. Clonar repositorio base
if not os.path.exists('/content/GeoChat'):
    !git clone https://github.com/mbzuai-oryx/GeoChat.git /content/GeoChat

# 2. Parchear DynamicCache en geochat_arch.py
arch_path = pathlib.Path('/content/GeoChat/geochat/model/geochat_arch.py')
if arch_path.exists():
    arch_code = arch_path.read_text()
    helper = """
def _get_cache_len(cache):
    if isinstance(cache, tuple): return cache[-1][-1].shape[-2]
    if hasattr(cache, 'get_seq_length'): return cache.get_seq_length()
    if hasattr(cache, 'key_cache'): return cache.key_cache[0].shape[-2]
    if hasattr(cache, 'keys'): return cache.keys[0].shape[-2] if isinstance(cache.keys, list) else cache.keys.shape[-2]
    return 0
"""
    if "_get_cache_len" not in arch_code:
        arch_code = helper + "\n" + arch_code
    arch_code = re.sub(
        r'attention_mask = torch\.ones\(\(attention_mask\.shape\[0\],\s*.*?\s*\+\s*1\)',
        r'attention_mask = torch.ones((attention_mask.shape[0], _get_cache_len(past_key_values) + 1)',
        arch_code
    )
    arch_path.write_text(arch_code)

# 3. Parchear builder.py y clip_encoder.py
builder_path = pathlib.Path('/content/GeoChat/geochat/model/builder.py')
if builder_path.exists():
    b_code = builder_path.read_text().replace(
        "**kwargs)",
        "**{k: v for k, v in kwargs.items() if k not in ['load_in_4bit', 'load_in_8bit']})"
    )
    builder_path.write_text(b_code)

clip_path = pathlib.Path('/content/GeoChat/geochat/model/multimodal_encoder/clip_encoder.py')
if clip_path.exists():
    c_code = clip_path.read_text().replace(".vision_model.embeddings", ".embeddings")
    clip_path.write_text(c_code)

# 4. Importar y cargar el modelo en 4 bits
sys.path.append('/content/GeoChat')
from geochat.model.builder import load_pretrained_model
from geochat.mm_utils import get_model_name_from_path, tokenizer_image_token, KeywordsStoppingCriteria
from geochat.conversation import conv_templates, SeparatorStyle
from geochat.constants import IMAGE_TOKEN_INDEX, DEFAULT_IMAGE_TOKEN

model_path = 'MBZUAI/geochat-7B'  # Identificador oficial en Hugging Face
model_name = get_model_name_from_path(model_path)
tokenizer, model, image_processor, context_len = load_pretrained_model(
    model_path, None, model_name, load_8bit=False, load_4bit=True, device='cuda'
)
model = model.eval()

# 5. Función de inferencia con resolución forzada a 504x504
def preguntar_geochat(imagen_pil, pregunta, max_tokens=512):
    image_tensor = image_processor.preprocess(
        imagen_pil,
        return_tensors='pt',
        size={"shortest_edge": 504},
        crop_size={"height": 504, "width": 504}
    )['pixel_values'].half().cuda()

    qs = DEFAULT_IMAGE_TOKEN + '\n' + pregunta if DEFAULT_IMAGE_TOKEN not in pregunta else pregunta
    conv = conv_templates["llava_v1"].copy()
    conv.append_message(conv.roles[0], qs)
    conv.append_message(conv.roles[1], None)
    prompt = conv.get_prompt()

    input_ids = tokenizer_image_token(prompt, tokenizer, IMAGE_TOKEN_INDEX, return_tensors='pt').unsqueeze(0).cuda()
    stop_str = conv.sep if conv.sep_style != SeparatorStyle.TWO else conv.sep2
    stopping_criteria = KeywordsStoppingCriteria([stop_str], tokenizer, input_ids)

    with torch.inference_mode():
        output_ids = model.generate(
            input_ids,
            images=image_tensor,
            max_new_tokens=max_tokens,
            do_sample=True,
            temperature=0.2,
            top_p=0.9,
            use_cache=False,
            stopping_criteria=[stopping_criteria]
        )
    return tokenizer.batch_decode(output_ids[:, input_ids.shape[1]:], skip_special_tokens=True)[0].strip()

# 6. Ejecución sobre imagen
# img = Image.open('tu_imagen_satelital.png').convert('RGB')
# respuesta = preguntar_geochat(img, 'Describe this satellite image in detail. Please output your response completely in Spanish.')
# print(respuesta)
```

---

## 14. Citación y Créditos (Citation & Credits)

### 14.1 Citación del Paper Original (BibTeX)
Si utilizas el modelo GeoChat en tu investigación, cita el trabajo original de los autores:

```bibtex
@inproceedings{kuckreja2024geochat,
  title={GeoChat: Grounded Large Vision-Language Model for Remote Sensing},
  author={Kuckreja, Kartik and Danish, Muhammad Sohail and Naseer, Muzammal and Das, Abhijit and Khan, Salman and Khan, Fahad Shahbaz},
  booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
  pages={27831--27840},
  year={2024}
}
```

### 14.2 Citación de la Adaptación y Ficha Técnica
Para referenciar esta implementación, parches de compatibilidad en Google Colab y documentación en español:

```bibtex
@misc{avendano2026geochatcolab,
  title={Adaptación, Optimización en 4-Bits y Ficha Técnica de GeoChat para Teledetección en Google Colab},
  author={Avenda{\~n}o Rojas, Diana Katherin},
  year={2026},
  howpublished={Universidad Distrital Francisco Jos{\'e} de Caldas, Curso de Inteligencia Artificial Aplicada al Territorio},
  note={Repositorio de demostración en Google Colab con parches de compatibilidad para Transformers y evaluación bilingüe}
}
```

### 14.3 Datos de la Autora de la Ficha
- **Nombre:** Diana Katherin Avendaño Rojas
- **Código Estudiantil:** 20211007063
- **Correo Electrónico Institucional:** `dkavendanor@udistrital.edu.co`
- **Usuario de GitHub:** [`diana-rojas2003`](https://github.com/diana-rojas2003)
- **Asignatura:** Inteligencia Artificial Aplicada al Territorio
- **Fecha de Elaboración:** 14 de octubre de 2026
