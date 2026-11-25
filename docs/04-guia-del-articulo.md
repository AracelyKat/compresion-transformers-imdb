# 04 · Guía del artículo científico

Extensión sugerida: **6–10 páginas**. Formato: el que indique el docente (IEEE o APA). El
borrador se guarda en `article/`.

## Estructura

### 1. Título y autor
- Título tentativo: *"Compresión de Transformers para análisis de sentimiento en dispositivos con
  recursos limitados: una comparación entre distilación y cuantización"*
- Autor, institución y asignatura.

### 2. Resumen / Abstract (150–250 palabras)
Un párrafo con: problema → objetivo → método (modelos, dataset) → resultados principales (con
números) → conclusión. Se escribe **al final**. Incluir versión en inglés y 4–5 palabras clave.

### 3. Introducción
- Contexto: el éxito de los Transformers y su costo computacional.
- Problema: dificultad para usarlos en dispositivos edge o móviles.
- Trabajos relacionados (breve): BERT, DistilBERT, cuantización, Green AI.
- Objetivo, pregunta de investigación e hipótesis (de `01-definicion-del-proyecto.md`).
- Aporte del trabajo.

### 4. Metodología
- Dataset y preprocesamiento (incluir la figura de longitud de reseñas).
- Modelos y técnicas de compresión, explicadas con claridad.
- Configuración de entrenamiento (tabla de hiperparámetros).
- Protocolo de evaluación y métricas (de `03-metodologia-experimental.md`).
- Entorno de hardware y software.

### 5. Resultados
- Tabla comparativa principal.
- Curvas de entrenamiento.
- Gráfico de trade-off precisión frente a eficiencia.
- Pruebas estadísticas.
- Contraste de H1, H2 y H3.

> En esta sección solo se **describe** lo observado; la interpretación va en Discusión.

### 6. Discusión
- ¿Por qué se obtuvieron estos resultados? Comparar con lo reportado en la literatura.
- Análisis de errores: qué tipo de reseñas falla el modelo comprimido.
- Mapas de atención: diferencias entre BERT y DistilBERT.
- Recomendación práctica: qué variante usar según el escenario.
- **Limitaciones** (amenazas a la validez).

### 7. Conclusiones y trabajo futuro
- Respuesta directa a la pregunta de investigación.
- 3–4 conclusiones concretas, con números.
- Trabajo futuro: poda de atención, cuantización INT4, prueba en un celular real, otros idiomas.

### 8. Referencias

## Referencias base sugeridas

1. Vaswani, A. et al. (2017). *Attention Is All You Need*. NeurIPS.
2. Devlin, J. et al. (2019). *BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding*. NAACL.
3. Sanh, V. et al. (2019). *DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter*. arXiv:1910.01108.
4. Hinton, G., Vinyals, O., Dean, J. (2015). *Distilling the Knowledge in a Neural Network*. arXiv:1503.02531.
5. Zafrir, O. et al. (2019). *Q8BERT: Quantized 8Bit BERT*. arXiv:1910.06188.
6. Jacob, B. et al. (2018). *Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference*. CVPR.
7. Maas, A. et al. (2011). *Learning Word Vectors for Sentiment Analysis*. ACL. (dataset IMDB)
8. Schwartz, R. et al. (2020). *Green AI*. Communications of the ACM.
9. Strubell, E. et al. (2019). *Energy and Policy Considerations for Deep Learning in NLP*. ACL.

> Verificar cada referencia (año, nombre exacto) antes de citarla.
