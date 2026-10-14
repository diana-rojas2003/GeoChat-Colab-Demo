---
dataset_info:
  features:
  - name: id
    dtype: string
  - name: image
    dtype: image
  - name: conversations
    list:
    - name: from
      dtype: string
    - name: value
      dtype: string
  splits:
  - name: train
    num_bytes: 
    num_examples: 318000
  download_size: 
  dataset_size: 
configs:
- config_name: default
  data_files:
  - split: train
    path: data/*
task_categories:
- visual-question-answering
- image-to-text
- object-detection
task_ids:
- visual-reasoning
- remote-sensing-captioning
- scene-classification
language:
- en
tags:
- remote-sensing
- geospatial
- earth-observation
- visual-grounding
- instruction-tuning
- aerial-imagery
- satellite-imagery
license: apache-2.0  # Declarada en Hugging Face. Sujeta a licencias heredadas de datasets base [VERIFICAR]
---

# Ficha del Conjunto de Datos (Dataset Card): GeoChat_Instruct

Esta ficha describe el conjunto de datos **GeoChat_Instruct**, el primer dataset masivo de ajuste por instrucciones (*instruction-tuning*) multimodal diseñado específicamente para el dominio de la teledetección (*Remote Sensing*). Sigue el estándar de documentación de la plantilla oficial de **Hugging Face Hub** y buenas prácticas en ciencia de datos territoriales.

---

## 1. Descripción del Conjunto de Datos (Dataset Description)

- **Nombre del conjunto de datos:** `GeoChat_Instruct`
- **Repositorio oficial en Hugging Face:** [MBZUAI/GeoChat_Instruct](https://huggingface.co/datasets/MBZUAI/GeoChat_Instruct)
- **Paper de referencia:** [GeoChat: Grounded Large Vision-Language Model for Remote Sensing (CVPR 2024)](https://arxiv.org/abs/2311.15826)
- **Institución creadora:** Mohamed bin Zayed University of Artificial Intelligence (MBZUAI), Abu Dabi, EAU.
- **Autores:** Kartik Kuckreja, Muhammad Sohail Danish, Muzammal Naseer, Abhijit Das, Salman Khan, Fahad Shahbaz Khan.
- **Documentado por:** Diana Katherin Avendaño Rojas (código 20212025089), Universidad Distrital Francisco José de Caldas.

### Resumen
**GeoChat_Instruct** contiene aproximadamente **318,000 pares de instrucciones, preguntas y respuestas multimodales** orientadas a tareas analíticas de observación de la Tierra. Fue creado para dotar a los modelos de visión-lenguaje de capacidades que trascienden los modelos generalistas: conversación interactiva sobre imágenes aéreas y satelitales, descripción pormenorizada de coberturas (*captioning*), respuesta a preguntas visuales complejas (VQA) y localización espacial precisa de objetos referenciados (*visual grounding*) mediante coordenadas normalizadas.

---

## 2. Estructura del Conjunto de Datos (Dataset Structure)

### 2.1 Formato de Almacenamiento
El conjunto de datos sigue el formato conversacional estandarizado de **LLaVA**:
1. **Colección de Imágenes:** Imágenes en formatos comprimidos estándar (`.jpg`, `.png`, `.tif`), con resoluciones espaciales heterogéneas provenientes de diversos sensores aéreos y satelitales.
2. **Metadatos y Anotaciones (JSON):** Un archivo estructurado con una lista de diccionarios que definen cada turno conversacional y su anclaje a una imagen.

### 2.2 Esquema de Datos (Data Schema)
Cada registro contiene los siguientes campos:

```json
{
  "id": "geochat_sample_0001234",
  "image": "images/ucmerced/agricultural01.jpg",
  "conversations": [
    {
      "from": "human",
      "value": "<image>\nWhat type of scene is shown in this aerial image and what are its prominent features?"
    },
    {
      "from": "gpt",
      "value": "This aerial photograph illustrates an agricultural area. Broad, rectangular fields with alternating green and cultivated crops are visible, bordered by narrow unpaved farm access roads."
    },
    {
      "from": "human",
      "value": "Can you localize the tractor operating in the upper field?"
    },
    {
      "from": "gpt",
      "value": "Certainly. The tractor is located at [124, 650, 180, 712]."
    }
  ]
}
```

### 2.3 Formato de Coordenadas de Grounding
Las coordenadas espaciales se presentan en formato de caja delimitadora normalizada en una cuadrícula virtual de 0 a 1000:
$$\text{caja} = [y_{\min}, x_{\min}, y_{\max}, x_{\max}]$$
donde $y_{\min}$ y $x_{\min}$ representan la esquina superior izquierda, y $y_{\max}$ y $x_{\max}$ representan la esquina inferior derecha.

---

## 3. Proceso de Creación del Dataset (Dataset Creation Process)

### 3.1 Pipeline de Generación Automatizada
A diferencia de los dominios visuales cotidianos (donde se cuenta con miles de anotaciones de usuarios en redes sociales), el dominio satelital requiere conocimiento pericial especializado de difícil escalamiento manual. Los autores de MBZUAI implementaron un **pipeline automatizado de síntesis de diálogos**:

```
[Datasets RS Existentes] ──▶ [Extracción de Metadatos y Cajas] ──▶ [Prompt a Vicuna-v1.5] ──▶ [Filtrado y Limpieza] ──▶ [GeoChat_Instruct (~318k)]
 (UCMerced, AID, DIOR...)      (Clases, Coordenadas, Atributos)       (Generación de Diálogos)       (Reglas heurísticas)
```

1. **Extracción de Anotaciones Preexistentes:** Se recolectaron millones de etiquetas semánticas, cajas de detección y atributos categóricos de múltiples bases de datos públicas de teledetección.
2. **Generación con Vicuna-v1.5:** Se estructuraron *prompts* con contexto denso introduciendo la clase de la imagen, los objetos presentes, sus posiciones relativas y conteos. El modelo LLM **Vicuna-v1.5** generó interacciones fluidas simulando preguntas de usuarios y respuestas analíticas completas.
3. **Verificación y Filtrado Heurístico:** Se descartaron interacciones con contradicciones espaciales, coordenadas invertidas ($x_{\min} > x_{\max}$) o referencias fuera de los límites de la imagen.

---

## 4. Conjuntos de Datos Fuente (Source Datasets)

`GeoChat_Instruct` no recopila imágenes satelitales primarias desde plataformas de órbita, sino que consolida y recombina imágenes provenientes de los benchmarks más reconocidos de la comunidad de teledetección:

| Tarea de Origen | Datasets Integrados | Características de las Imágenes | Tipos de Instrucciones Generadas |
| :--- | :--- | :--- | :--- |
| **Clasificación de Escenas** | **UCMerced**, **AID**, **NWPU-RESISC45**, **PatternNet** | Ortofotos aéreas RGB, resoluciones métricas y submétricas (0.1 a 8 m/px). | Identificación de uso y cobertura del suelo (urbano, residencial, industrial, bosque, agrícola, agua). |
| **Visual Question Answering** | **RSVQA-HRBEN**, **RSVQA-LRBEN**, **RSIVQA** | Imágenes de Países Bajos (aéreas, 15 cm) y Sentinel-2 (satelitales, 10 m). | Preguntas sobre presencia de entidades, conteo y comparaciones relacionales. |
| **Descripción de Imágenes** | **Sydney-Captions**, **UCM-Captions**, **RSICD** | Vistas cenitales aéreas con múltiples oraciones descriptivas por imagen. | Generación de párrafos estructurados describiendo patrones, texturas y elementos espaciales. |
| **Detección y Grounding** | **DIOR**, **DOTA-v1.5** | Colecciones de gran escala con millones de instancias etiquetadas. | Instrucciones de localización espacial (*Referring Expression Comprehension*) y descripción de regiones acotadas. |

---

## 5. Anotaciones y Calidad de los Datos (Annotations & Data Quality)

- **Anotadores:** La autoría semántica de los diálogos es sintética, atribuida al modelo **Vicuna-v1.5**, anclada en etiquetas humanas curadas por los creadores de los datasets fuente.
- **Validación de Calidad:**
  - Garantía de precisión espacial: Los cuadros delimitadores derivan de anotaciones periciales de DIOR/DOTA, manteniendo coherencia con las entidades geográficas reales.
  - Alucinación en texto sintético: Existe la posibilidad residual de que el modelo de lenguaje haya adornado descripciones con adjetivos o detalles inferidos no estrictamente observables en la imagen (e.g. asumir actividad en un edificio cerrado).

---

## 6. Consideraciones Sociales y Sesgos (Social Considerations & Biases)

- **Sesgo Geográfico y Territorial:**
  - La inmensa mayoría de las imágenes aéreas de alta resolución provienen de regiones de Norteamérica, Europa Occidental y áreas desarrolladas del este de Asia.
  - El dataset carece de representatividad adecuada para paisajes del **Sur Global**, como morfologías urbanas irregulares o asentamientos de ladera característicos de Latinoamérica (e.g., Bogotá, Medellín, favelas, comunas), así como ecosistemas tropicales o de páramo.
- **Sesgo de Resolución Temporal y Estacional:**
  - Predominan capturas en días despejados durante estaciones favorables (primavera/verano). Escenas con bruma espesa, nubosidad densa o sombras orográficas profundas están subrepresentadas.
- **Sesgo Espectral:**
  - Exclusividad en canales visibles RGB. No se incluyen canales no visibles fundamentales en teledetección científica (infrarrojo cercano NIR para índices de vegetación NDVI, bandas térmicas o datos radar SAR para humedad del suelo).

---

## 7. Limitaciones del Conjunto de Datos (Limitations)

1. **Restricción Monolingüe (Inglés):** Todas las instrucciones y respuestas sintéticas de `GeoChat_Instruct` están redactadas en inglés. No contiene pares de datos de entrenamiento en español ni en otros idiomas, lo que obliga al usuario a recurrir a técnicas de prompting cruzado en inferencia.
2. **Dependencia de Datos Sintéticos:** Al haber sido generado mediante un LLM (Vicuna), el dataset puede exhibir patrones repetitivos de fraseo y estilos estilísticos particulares del modelo generador.
3. **Escala Espacial Fija en Entrenamiento:** El ajuste del modelo GeoChat se optimizó forzando las imágenes a 504×504 píxeles. Esto implica que patrones geográficos que dependan de contextos panorámicos extremadamente amplios sufren pérdidas de resolución por compresión.

---

## 8. Licencia y Consideraciones Legales

- **Licencia declarada:** Apache-2.0 en el repositorio de Hugging Face de MBZUAI.
- **Situación de Licencias Heredadas :**  
  > [!CAUTION]
  > El usuario debe verificar la legalidad de uso comercial antes de cualquier despliegue operativo:
  > 1. Datasets como **DIOR** y **NWPU-RESISC45** fueron publicados bajo licencias de investigación académica no comercial.
  > 2. Los datos generados mediante **Vicuna** están sujetos a las restricciones de la licencia de **LLaMA-2** (Meta) y las condiciones de servicio de **OpenAI** (por el uso indirecto de ShareGPT).
  > 3. Por ende, la etiqueta `apache-2.0` no exime del cumplimiento de las restricciones no comerciales impuestas por los datasets constitutivos.

---

## 9. Nota sobre los Datos del Demo (Imagen Satelital Propia de Prueba)

Durante el demo experimental desarrollado en Google Colab por **Diana Katherin Avendaño Rojas**, se evaluó el comportamiento del modelo utilizando una imagen propia ajena al conjunto de entrenamiento original de GeoChat:

- **Archivo evaluado:** `/content/drive/MyDrive/Geochat/satelite.png`
- **Resolución nativa:** 852 × 420 píxeles, 3 canales (RGB).
- **Contenido visual analizado:** Vista cenital de un sector urbano consolidado con múltiples bloques de edificaciones rectangulares y una vía arterial de tránsito.
- **Ficha de la muestra propia :**
  - **Fuente del dato:** `[COMPLETAR: e.g., Captura satelital Google Earth / Ortofoto IDECA Bogotá / PlanetScope / Sentinel-2]`
  - **Resolución espacial (GSD):** `[COMPLETAR: e.g., ~0.5 metros por píxel / 1.0 m / 0.3 m]`
  - **Fecha aproximada de captura:** `[COMPLETAR: e.g., 2024 / 2025]`
  - **Licencia de la imagen:** `[COMPLETAR: e.g., Fines educativos y de investigación / CC BY-NC 4.0 / Propietaria con fines académicos]`

---

## 10. Citación y Autora

### 10.1 Citación del Dataset Original (BibTeX)
```bibtex
@inproceedings{kuckreja2024geochat,
  title={GeoChat: Grounded Large Vision-Language Model for Remote Sensing},
  author={Kuckreja, Kartik and Danish, Muhammad Sohail and Naseer, Muzammal and Das, Abhijit and Khan, Salman and Khan, Fahad Shahbaz},
  booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
  pages={27831--27840},
  year={2024}
}
```

### 10.2 Datos de la Autora de la Ficha
- **Nombre:** Diana Katherin Avendaño Rojas
- **Código Estudiantil:** 20212025089
- **Correo Institucional:** `dkavendanor@udistrital.edu.co`
- **Usuario de GitHub:** [`diana-rojas2003`](https://github.com/diana-rojas2003)
- **Curso:** Inteligencia Artificial Aplicada al Territorio
- **Universidad:** Universidad Distrital Francisco José de Caldas
- **Fecha de Elaboración:** 14 de octubre de 2026
