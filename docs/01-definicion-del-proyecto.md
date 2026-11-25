# 01 · Definición del proyecto

## 1. Problema

Los Transformers (BERT, GPT) son el estado del arte en procesamiento de lenguaje natural, pero:

- Ocupan **mucho espacio** (BERT-base pesa más de 400 MB).
- Son **lentos sin GPU**, que es la situación de un celular o un dispositivo edge.
- **Consumen energía**, lo que afecta a la batería y al impacto ambiental.

Esto dificulta usarlos fuera de servidores potentes. Las técnicas de compresión buscan modelos
más livianos que conserven la mayor parte de la precisión.

## 2. Objetivos

### Objetivo general
Evaluar experimentalmente el efecto de la **distilación** y la **cuantización** sobre la eficiencia
y la precisión de modelos Transformer en la tarea de análisis de sentimiento.

### Objetivos específicos
1. Ajustar (fine-tuning) BERT-base y DistilBERT para clasificar reseñas de IMDB.
2. Aplicar cuantización dinámica INT8 a ambos modelos.
3. Medir y comparar las 4 variantes en precisión (accuracy, F1), tamaño, latencia y energía.
4. Determinar si las diferencias de precisión son estadísticamente significativas.
5. Identificar qué variante ofrece el mejor equilibrio entre eficiencia y precisión.

## 3. Pregunta de investigación e hipótesis

**Pregunta:** ¿Cuánto se puede reducir el tamaño, el tiempo de inferencia y el consumo energético
de BERT mediante distilación y cuantización, y cuál es el costo en precisión?

| Hipótesis | Enunciado |
|---|---|
| **H1** | DistilBERT reduce el tamaño ~40% y la latencia ~2× respecto de BERT, perdiendo menos de 2 puntos de accuracy. |
| **H2** | La cuantización INT8 reduce el tamaño a menos de la mitad y acelera la inferencia en CPU, perdiendo menos de 1 punto de accuracy. |
| **H3** | La combinación DistilBERT + INT8 ofrece el mejor equilibrio eficiencia/precisión. |

> Las hipótesis se aceptan o rechazan con los resultados de la Fase 4. Rechazar una hipótesis
> **también es un resultado válido** y debe discutirse.

## 4. Alcance

### Incluye
- Dataset IMDB (50.000 reseñas en inglés, clases balanceadas).
- 2 modelos preentrenados de Hugging Face: `bert-base-uncased` y `distilbert-base-uncased`.
- Cuantización dinámica post-entrenamiento con PyTorch.
- Medición de latencia y energía en **CPU** (simulando un dispositivo sin GPU).

### No incluye
- Entrenar modelos desde cero ni crear técnicas de compresión nuevas.
- Pruebas en un celular físico.
- Poda (pruning) de atención. Puede mencionarse como trabajo futuro.
- Textos en otros idiomas.

## 5. Aporte y originalidad

- **Medición de energía** con `codecarbon`, que muchos estudios comparativos omiten.
- **Análisis combinado** de distilación y cuantización (por separado y juntas).
- **Pruebas estadísticas** (McNemar e intervalos de confianza) en lugar de comparar solo números.
- **Gráfico de trade-off** (precisión frente a tamaño y latencia) para guiar decisiones prácticas.

## 6. Relevancia

Permite responder una pregunta práctica: *"¿qué modelo uso si quiero análisis de sentimiento en
una app móvil?"*. Se relaciona con IA eficiente (*Green AI*), privacidad (procesar en el
dispositivo sin enviar datos a un servidor) y acceso a la IA en equipos de bajo costo.
