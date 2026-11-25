# 03 · Metodología experimental

Este documento fija **cómo** se hacen los experimentos, para que sean justos y reproducibles.
Corresponde al criterio de *rigor metodológico* (20%).

## 1. Dataset

| Partición | Tamaño | Origen |
|---|---|---|
| Entrenamiento | 22.500 | 90% de `imdb/train` (estratificado) |
| Validación | 2.500 | 10% de `imdb/train` (estratificado) |
| Prueba | 25.000 | `imdb/test` completo |

- Clases balanceadas (50% positivas, 50% negativas).
- Preprocesamiento: eliminar etiquetas HTML y tokenizar (`max_length = 256`, truncado al final).
- El conjunto de prueba **no se usa** hasta la Fase 4.

## 2. Modelos

| Modelo | Checkpoint Hugging Face | Capas | Parámetros aprox. |
|---|---|---|---|
| BERT-base | `bert-base-uncased` | 12 | 110M |
| DistilBERT | `distilbert-base-uncased` | 6 | 66M |

**Distilación:** DistilBERT fue creado por Sanh et al. (2019) entrenando un modelo pequeño
("estudiante") para imitar a BERT ("profesor"). Aquí se usa el modelo ya distilado y se ajusta a
la tarea.

**Cuantización dinámica INT8:** los pesos de las capas lineales se guardan en 8 bits en vez de
32 bits, y las activaciones se cuantizan durante la inferencia. No requiere reentrenar.

## 3. Hiperparámetros de entrenamiento (iguales para ambos modelos)

| Parámetro | Valor |
|---|---|
| Épocas | 2 |
| Tasa de aprendizaje | 2e-5 |
| Batch size | 16 |
| Optimizador | AdamW (weight decay 0,01) |
| Warmup | 10% de los pasos |
| Precisión de entrenamiento | FP16 mixta (GPU) |
| Semilla | 42 |
| Selección de modelo | Mejor F1 en validación |

## 4. Métricas

| Dimensión | Métrica | Cómo se mide |
|---|---|---|
| Calidad | Accuracy, Precision, Recall, F1 | `sklearn.metrics` sobre las 25.000 reseñas de prueba |
| Tamaño | MB en disco, nº de parámetros | `os.path.getsize` del `state_dict` guardado |
| Velocidad | Latencia (ms/reseña) y rendimiento (reseñas/s) | `time.perf_counter()` en CPU |
| Energía | kWh por cada 1.000 reseñas | `codecarbon.EmissionsTracker` |

## 5. Protocolo de medición de latencia y energía

Para que la comparación sea justa:

1. **Todo en CPU**, con el mismo número de hilos (`torch.set_num_threads(N)`, con N fijo y registrado).
2. Las mismas **1.000 reseñas** de prueba (fijadas con la semilla) para todas las variantes.
3. **Calentamiento:** 20 inferencias previas que no se cuentan.
4. **Latencia:** batch = 1, se repite 3 veces y se reporta la media ± desviación estándar.
5. **Rendimiento:** batch = 32.
6. Se mide con `torch.inference_mode()` y el modelo en modo `eval()`.
7. Se registra el hardware (modelo de CPU, núcleos, RAM) en el notebook.

> ⚠️ La energía de `codecarbon` es una **estimación** basada en el hardware y no una medición
> física. Debe indicarse como limitación.

## 6. Análisis estadístico

- **Intervalo de confianza al 95%** de la accuracy de cada variante (bootstrap, 1.000 remuestreos).
- **Prueba de McNemar** (`statsmodels`) entre BERT-base FP32 y cada variante comprimida:
  ¿la diferencia en aciertos es estadísticamente significativa (p < 0,05)?
- Las diferencias se reportan en **puntos porcentuales** y como **% de cambio relativo**.

## 7. Tabla de resultados esperada

| Variante | Acc. | F1 | IC 95% | p (McNemar) | MB | ms/reseña | reseñas/s | kWh/1000 |
|---|---|---|---|---|---|---|---|---|
| BERT FP32 | | | | — | | | | |
| BERT INT8 | | | | | | | | |
| DistilBERT FP32 | | | | | | | | |
| DistilBERT INT8 | | | | | | | | |

## 8. Amenazas a la validez (para la Discusión)

- Una sola semilla: no se mide la variación entre entrenamientos (limitado por el tiempo de GPU).
- Latencia medida en la CPU de Colab, que no es un celular real.
- La energía es estimada.
- `max_length = 256` trunca algunas reseñas largas.
- Solo un dataset y un idioma (inglés).
