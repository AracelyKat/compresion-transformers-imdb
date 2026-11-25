# 02 · Plan por fases

Trabajo individual. Cada fase tiene un objetivo, tareas concretas, un entregable y una lista de
verificación. Las duraciones son estimadas en **sesiones de trabajo (~2–3 h)**.

```
Fase 0 → Fase 1 → Fase 2 → Fase 3 → Fase 4 → Fase 5 → Fase 6 → Fase 7
Entorno   Datos    Entreno  Compres. Evaluac. Visualiz. Artículo  Cierre
                    └──── (mientras entrena: redactar Intro y Metodología) ────┘
```

| Fase | Nombre | Sesiones estimadas |
|---|---|---|
| 0 | Preparación del entorno | 1 |
| 1 | Datos y preprocesamiento | 1–2 |
| 2 | Entrenamiento de modelos base | 2 |
| 3 | Compresión | 1 |
| 4 | Evaluación experimental | 2 |
| 5 | Visualización y análisis | 1–2 |
| 6 | Redacción del artículo | 3 |
| 7 | Reflexión, revisión y entrega | 1 |
| | **Total** | **~12–14 sesiones** |

---

## Fase 0 · Preparación del entorno

**Objetivo:** tener un notebook que funcione en Google Colab y se pueda reproducir.

**Tareas**
- [ ] Crear el notebook `notebooks/compresion_transformers_imdb.ipynb` en Colab.
- [ ] Activar la GPU (Entorno de ejecución → Cambiar tipo → T4 GPU).
- [ ] Instalar las librerías: `transformers`, `datasets`, `evaluate`, `codecarbon`, `scikit-learn`, `statsmodels`.
- [ ] Montar Google Drive para guardar modelos y resultados (Colab borra los archivos al cerrarse).
- [ ] Fijar una semilla global (`seed = 42`) para `random`, `numpy` y `torch`.
- [ ] Imprimir las versiones de las librerías y el hardware (GPU, CPU, número de núcleos).
- [ ] Escribir una celda Markdown de introducción con el título, el objetivo y la estructura del notebook.

**Entregable:** notebook base que se ejecuta sin errores.

---

## Fase 1 · Datos y preprocesamiento

**Objetivo:** conocer el dataset y dejarlo listo para los modelos.

**Tareas**
- [ ] Cargar IMDB con `datasets.load_dataset("imdb")`.
- [ ] Dividir los datos: train 22.500 / validación 2.500 (estratificado, desde train) / test 25.000.
- [ ] **Análisis exploratorio (EDA):**
  - [ ] Distribución de clases (gráfico de barras).
  - [ ] Distribución del largo de las reseñas en palabras y en tokens (histograma).
  - [ ] Porcentaje de reseñas que superan 256 y 512 tokens.
  - [ ] Mostrar 2–3 ejemplos de reseñas positivas y negativas.
- [ ] Limpieza mínima: quitar etiquetas HTML (`<br />`). No se quitan stopwords, porque BERT necesita el texto natural.
- [ ] Tokenizar con el tokenizador de cada modelo (`max_length = 256`, truncamiento y padding).
- [ ] Justificar en Markdown la elección de `max_length = 256` usando el histograma.

**Entregable:** sección 1 del notebook con el EDA y los datasets tokenizados.
**Figuras:** `results/figures/eda_clases.png`, `results/figures/eda_longitud.png`

---

## Fase 2 · Entrenamiento de modelos base

**Objetivo:** obtener BERT-base y DistilBERT ajustados a la tarea de sentimiento.

**Tareas**
- [ ] Cargar `bert-base-uncased` y `distilbert-base-uncased` con `AutoModelForSequenceClassification` (2 clases).
- [ ] Configurar `Trainer` con los mismos hiperparámetros para ambos (ver `03-metodologia-experimental.md`).
- [ ] Entrenar 2 épocas, evaluando en validación al final de cada época.
- [ ] Guardar el mejor checkpoint de cada modelo en Drive.
- [ ] Registrar el tiempo de entrenamiento y la energía de entrenamiento (`codecarbon`).
- [ ] Graficar las curvas de pérdida (train y validación) y la accuracy de validación por época.
- [ ] Comentar si hay sobreajuste.

**Entregable:** 2 modelos FP32 entrenados con sus curvas de aprendizaje.
**Figuras:** `results/figures/curvas_bert.png`, `results/figures/curvas_distilbert.png`

> 💡 Mientras entrena (~20–40 min por modelo), avanza con la Introducción del artículo (Fase 6).

---

## Fase 3 · Compresión

**Objetivo:** generar las versiones cuantizadas.

**Tareas**
- [ ] Mover ambos modelos a CPU (la cuantización dinámica de PyTorch funciona en CPU).
- [ ] Aplicar `torch.quantization.quantize_dynamic(model, {torch.nn.Linear}, dtype=torch.qint8)`.
- [ ] Verificar que los modelos cuantizados predicen correctamente con 5 reseñas de prueba.
- [ ] Guardar las 4 variantes y medir su tamaño en disco (MB).
- [ ] Explicar en Markdown, con palabras simples, qué es la cuantización y qué es la distilación.

**Entregable:** 4 variantes de modelo listas para evaluar.

---

## Fase 4 · Evaluación experimental

**Objetivo:** medir las 4 variantes con el mismo protocolo.

**Tareas**
- [ ] **Calidad:** accuracy, precision, recall y F1 sobre el test completo (25.000 reseñas).
- [ ] **Matriz de confusión** de cada variante.
- [ ] **Tamaño:** MB en disco y número de parámetros.
- [ ] **Latencia en CPU:** ms por reseña (batch = 1) y reseñas por segundo (batch = 32).
- [ ] **Energía:** kWh por cada 1.000 reseñas con `codecarbon`.
- [ ] **Estadística:**
  - [ ] Intervalo de confianza al 95% de la accuracy (bootstrap).
  - [ ] Prueba de McNemar: cada variante comprimida frente a BERT-base FP32.
- [ ] Guardar todo en `results/tables/resultados.csv`.
- [ ] Contrastar H1, H2 y H3 (aceptar o rechazar, con argumentos).

**Entregable:** tabla comparativa final y contraste de hipótesis.

---

## Fase 5 · Visualización y análisis

**Objetivo:** presentar los resultados de forma clara.

**Figuras**
- [ ] Barras comparativas: tamaño, latencia, energía y accuracy de las 4 variantes.
- [ ] **Gráfico de trade-off:** accuracy (eje Y) frente a latencia (eje X); el tamaño del punto indica los MB.
- [ ] Matrices de confusión en una grilla 2×2.
- [ ] **Mapa de atención** de BERT y DistilBERT para una misma reseña (`output_attentions=True`).
- [ ] **Análisis de errores:** 5 reseñas que BERT clasifica bien y la versión comprimida mal. ¿Qué tienen en común (sarcasmo, negaciones, longitud)?

**Entregable:** figuras en `results/figures/` y conclusiones parciales en el notebook.

---

## Fase 6 · Redacción del artículo científico

**Objetivo:** escribir el artículo con la estructura pedida.

**Tareas**
- [ ] Revisar 6–10 referencias clave (ver `04-guia-del-articulo.md`).
- [ ] Redactar en este orden: Metodología → Resultados → Discusión → Introducción → Conclusiones → Resumen → Título.
- [ ] Insertar las figuras y tablas de la Fase 5.
- [ ] Formatear las referencias (APA o IEEE, según pida el docente).

**Entregable:** `article/articulo.pdf` (o `.docx`).

---

## Fase 7 · Reflexión, revisión y entrega

**Objetivo:** cerrar con calidad y cubrir los criterios de autoevaluación y reflexión.

**Tareas**
- [ ] Reiniciar Colab y ejecutar el notebook completo de principio a fin ("Ejecutar todo").
- [ ] Revisar que cada bloque de código tenga su explicación en Markdown.
- [ ] Completar la autoevaluación y la reflexión en `05-bitacora-y-autoevaluacion.md`.
- [ ] Revisar la ortografía y la coherencia entre el notebook y el artículo (mismos números).
- [ ] Empaquetar y entregar.

**Entregable:** notebook y artículo finales.
