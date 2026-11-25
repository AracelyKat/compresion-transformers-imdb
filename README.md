# Compresión de Transformers para Análisis de Sentimiento en Dispositivos con Recursos Limitados

**Asignatura:** Investigación Aplicada en Deep Learning
**Temática:** Transformers y Modelos de Autoatención (NLP) → *Optimización de Transformers para Recursos Limitados*
**Modalidad:** Trabajo individual

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AracelyKat/compresion-transformers-imdb/blob/main/notebooks/compresion_transformers_imdb.ipynb)

---

## Resumen del proyecto

Los modelos Transformer como BERT logran muy buenos resultados en tareas de lenguaje, pero son
pesados (cientos de MB) y lentos en dispositivos sin GPU (celulares, equipos edge). Este proyecto
compara dos técnicas de compresión, **distilación** y **cuantización**, aplicadas a un clasificador
de sentimiento entrenado con reseñas de películas de **IMDB**, y mide qué se gana (tamaño, velocidad,
energía) y qué se pierde (precisión).

**Pregunta de investigación:**
> ¿Cuánto se puede reducir el tamaño, el tiempo de inferencia y el consumo energético de BERT
> mediante distilación y cuantización, y cuál es el costo en precisión?

## Modelos comparados

| # | Modelo | Técnica |
|---|---|---|
| 1 | BERT-base FP32 | Referencia (sin comprimir) |
| 2 | BERT-base INT8 | Cuantización dinámica |
| 3 | DistilBERT FP32 | Distilación |
| 4 | DistilBERT INT8 | Distilación + cuantización |

## Estructura del repositorio

```
deep-learning/
├── README.md                         ← Este archivo (visión general)
├── .gitignore
├── requirements.txt                  ← Librerías necesarias
├── docs/                             ← Planificación y documentación
│   ├── 01-definicion-del-proyecto.md ← Objetivos, hipótesis, alcance
│   ├── 02-plan-por-fases.md          ← Fases detalladas con tareas y checklist
│   ├── 03-metodologia-experimental.md← Configuración, métricas y protocolo de medición
│   ├── 04-guia-del-articulo.md       ← Estructura y contenido del artículo científico
│   └── 05-bitacora-y-autoevaluacion.md ← Registro de avance y reflexión crítica
├── notebooks/                        ← Notebook de implementación (entregable 1)
├── article/                          ← Artículo científico (entregable 2)
├── results/
│   ├── figures/                      ← Gráficos generados por el notebook
│   └── tables/                       ← Tablas de resultados (CSV)
└── models/                           ← Modelos entrenados (no se versionan, son pesados)
```

## Fases (resumen)

| Fase | Nombre | Entregable |
|---|---|---|
| 0 | Preparación del entorno | Notebook base funcionando en Colab |
| 1 | Datos y preprocesamiento | EDA + datos tokenizados |
| 2 | Entrenamiento de modelos base | BERT y DistilBERT ajustados |
| 3 | Compresión (cuantización) | 4 variantes de modelo |
| 4 | Evaluación experimental | Tabla comparativa de métricas |
| 5 | Visualización y análisis | Figuras para el artículo |
| 6 | Redacción del artículo | Artículo científico |
| 7 | Reflexión, revisión y entrega | Entrega final |

Detalle completo en [`docs/02-plan-por-fases.md`](docs/02-plan-por-fases.md).

## Entregables y criterios de evaluación

| Criterio | Peso | Dónde se cubre |
|---|---|---|
| Originalidad y relevancia | 15% | Medición de energía + análisis de trade-offs (docs/01) |
| Calidad del notebook | 25% | `notebooks/` |
| Rigor metodológico | 20% | Protocolo de medición y pruebas estadísticas (docs/03) |
| Artículo científico | 20% | `article/` |
| Autoevaluación | 10% | `docs/05` (adaptado a trabajo individual) |
| Reflexión y análisis crítico | 10% | `docs/05` + sección de discusión del artículo |

## Tecnologías

Python 3 · PyTorch · Hugging Face `transformers` y `datasets` · scikit-learn · `codecarbon` ·
matplotlib / seaborn · Google Colab (GPU T4)
