# Notebooks

**Entregable 1:** `compresion_transformers_imdb.ipynb`

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AracelyKat/compresion-transformers-imdb/blob/main/notebooks/compresion_transformers_imdb.ipynb)

Repositorio privado: la primera vez, Colab pedirá autorizar el acceso a GitHub.

## Secciones planificadas

| Sección | Contenido | Fase |
|---|---|---|
| 0 | Introducción, instalación, semillas, hardware | 0 |
| 1 | Carga de IMDB, EDA y tokenización | 1 |
| 2 | Fine-tuning de BERT-base y DistilBERT + curvas | 2 |
| 3 | Cuantización dinámica INT8 | 3 |
| 4 | Evaluación: calidad, tamaño, latencia, energía, estadística | 4 |
| 5 | Visualizaciones, mapas de atención, análisis de errores | 5 |
| 6 | Conclusiones del notebook | 5 |

## Flujo de trabajo por fase

1. Abrir el notebook en Colab con el botón de arriba (GPU T4 activada).
2. Ejecutar y completar la fase.
3. *Archivo → Descargar → Descargar .ipynb* y reemplazar este archivo.
4. Copiar figuras y tablas desde Drive a `../results/`.
5. Un solo commit: `Fase X: ...`.

## Convenciones

- Cada bloque de código va precedido de una celda Markdown que explica **qué hace y por qué**.
- Las figuras se guardan en `../results/figures/` y las tablas en `../results/tables/`.
- Los modelos se guardan en Google Drive, no en el repositorio.
- Antes de entregar: *Reiniciar y ejecutar todo* sin errores.
