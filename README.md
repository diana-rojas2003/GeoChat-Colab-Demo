# GeoChat

**GeoChat** es un Modelo de Lenguaje Visual de Gran Escala (LVLM) multimodal diseñado y especializado para la interpretación de imágenes de teledetección (*Remote Sensing*), desarrollado originalmente por investigadores de MBZUAI (publicado en CVPR 2024). A diferencia de los modelos de visión generales, GeoChat está optimizado para analizar vistas aéreas y satelitales, permitiendo interrogar escenas territoriales en lenguaje natural, reconocer coberturas del suelo, resolver preguntas visuales complejas y ubicar elementos en el espacio geográfico. Este repositorio documenta su adaptación técnica y ejecución práctica en Google Colab con una GPU Nvidia Tesla T4 mediante cuantización a 4 bits (`bitsandbytes`), parches de compatibilidad para versiones modernas de `transformers` y evaluación cualitativa con generación de respuestas en español.

### ¿Qué puede hacer?

| Tarea | Descripción |
|---|---|
| 🏷️ Clasificación de escena | Identificar el tipo de zona (urbana, rural, industrial...) |
| 📝 Descripción de imagen | Generar una descripción detallada de la imagen satelital |
| ❓ VQA (Visual Question Answering) | Responder preguntas sobre el contenido de la imagen |
| 📍 Grounding visual | Localizar objetos mencionados con coordenadas |
| 🗣️ Conversación | Mantener un diálogo sobre la imagen |

---

- [Ver ficha del modelo](./MODEL_CARD.md)
- [Ver ficha del conjunto de datos](./DATASET_CARD.md)
- [Ver cuaderno de demostración en Google Colab](./GeoChat_Colab_Demo.ipynb)

### Datos de la autora de la ficha
- **Nombre:** Diana Katherin Avendaño Rojas
- **Código estudiantil:** 20212025089
- **Correo electrónico:** dkavendanor@udistrital.edu.co
- **Usuario de GitHub:** [diana-rojas2003](https://github.com/diana-rojas2003)
- **Curso:** Inteligencia Artificial Aplicada al Territorio
- **Fecha:** 14/10/2026

*Basado en GeoChat de MBZUAI; las modificaciones y esta documentación son de Diana Katherin Avendaño Rojas.*
