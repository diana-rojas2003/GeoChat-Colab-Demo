# GeoChat-7B

GeoChat-7B es un modelo de lenguaje visual de gran escala (LVLM) multimodal especializado en teledetección (*Remote Sensing*), desarrollado originalmente por investigadores de MBZUAI (publicado en CVPR 2024) mediante la integración del codificador visual CLIP ViT-L/14, un proyector MLP y el modelo de lenguaje Vicuna-7B (arquitectura LLaVA-1.5) ajustado por instrucciones con el dataset GeoChat_Instruct (~318k pares de tareas geoespaciales). Este repositorio documenta la adaptación técnica y ejecución práctica en Google Colab utilizando una GPU Nvidia Tesla T4 mediante cuantización a 4 bits (`bitsandbytes`), la implementación de parches de compatibilidad para versiones modernas de la librería `transformers` (resolviendo incompatibilidades en `hf_prefixlm_converter.py`, `builder.py`, `clip_encoder.py` y `geochat_arch.py` ante `DynamicCache`), y su evaluación analítica sobre imágenes satelitales con generación de respuestas en español.

- [Ver ficha del modelo](./MODEL_CARD.md)
- [Ver ficha del conjunto de datos](./DATASET_CARD.md)
- [Ver cuaderno de demostración en Google Colab](./GeoChat_Colab_Demo.ipynb)

### Datos de la autora de la ficha
- **Nombre:** Diana Katherin Avendaño Rojas
- **Código estudiantil:** 20211007063
- **Correo electrónico:** dkavendanor@udistrital.edu.co
- **Usuario de GitHub:** [diana-rojas2003](https://github.com/diana-rojas2003)
- **Curso:** Inteligencia Artificial Aplicada al Territorio
- **Fecha:** 14/10/2026

*Basado en GeoChat de MBZUAI; las modificaciones y esta documentación son de Diana Katherin Avendaño Rojas.*
