# Unidad-10-Tipos-de-an-lisis-de-sentimiento
Unidad 10 · Tipos de análisis de sentimiento


# 🌍 Multilingual Aspect-Based Sentiment Analysis (ABSA) Pipeline

Este repositorio contiene un pipeline de producción para el **Análisis de Sentimiento Basado en Aspectos (ABSA)** diseñado para procesar flujos de datos multiculturales e internacionales (español e inglés) de manera síncrona, sin requerir capas intermedias de traducción automática.

A diferencia de los enfoques tradicionales de análisis global, este sistema deconstruye las opiniones complejas para aislar la carga emocional dirigida a atributos específicos de negocio (cámara, precio, soporte técnico, envío, etc.), permitiendo extraer insights granulares listos para la toma de decisiones estratégicas.

---

## 🛠️ Desafíos de Ingeniería Resueltos

La implementación base tradicional suele presentar dos limitaciones críticas que fueron subsanadas en este desarrollo:

1. **El Problema del Sesgo por Vecindad (Efecto Espejo):** Si un cliente escribe *"La cámara es excelente, pero la batería dura muy poco"*, los clasificadores estándar evalúan la oración completa y marcan **ambos aspectos como negativos** debido al impacto de las palabras finales.
   * **La Solución:** Se diseñó un **algoritmo de segmentación por cláusulas gramaticales**. Al fragmentar el texto ante la presencia de conectores coordinados adversativos (*"pero"*, *"aunque"*, *"but"*), el pipeline aísla los contextos. Así, la cámara se evalúa de forma independiente como `POSITIVE` y la batería como `NEGATIVE`.
2. **El Conflicto Idiomático:** El uso de clasificadores entrenados en un solo idioma destruye la precisión al enfrentarse a un corpus multilingüe. 
   * **La Solución:** Se unificó el procesamiento bajo un **Transformer nativamente multilingüe (`XLM-RoBERTa`)** que opera sobre embeddings compartidos para inglés y español, eliminando la latencia y la pérdida de matices propias de la traducción previa.

---

## 🔄 Arquitectura y Modularización del Pipeline

El script está construido bajo estándares de código limpio y desacoplado, dividido en cuatro módulos funcionales:

* **Módulo 1: Ingesta del Corpus:** Configuración de un DataFrame realista con reseñas simuladas de e-commerce en múltiples idiomas y con oraciones compuestas.
* **Módulo 2: Core Deep Learning:** Inicialización y configuración del pipeline de Hugging Face optimizado con soporte para aceleración por hardware (GPU/CPU).
* **Módulo 3: Extractor Semántico y Reglas Sintácticas:** Motor encargado de fragmentar las cláusulas, rastrear las palabras clave asignadas a cada aspecto de negocio y estandarizar las respuestas del modelo.
* **Módulo 4: Analítica Visual:** Generación automatizada de reportes gráficos utilizando `Seaborn` y `Matplotlib` para mostrar la distribución de frecuencias de sentimientos por cada atributo extraído.

---

## 🎯 Impacto en el Escenario de Negocio

Este nivel de granularidad transforma la analítica de datos en decisiones corporativas ejecutables:
* **Área de Producto / Hardware:** El pipeline detecta que la experiencia con la cámara es sobresaliente de forma global, pero alerta que el departamento de ingeniería debe rediseñar de inmediato la autonomía de la batería.
* **Área de Operaciones / Logística:** Permite aislar quejas específicas sobre demoras en los tiempos de entrega (`delivery`) sin que estas afecten la percepción de la calidad intrínseca del producto vendido.

---

## ⚙️ Requisitos y Reproducción del Entorno

Para clonar y ejecutar este laboratorio de forma reproducible, asegúrese de cumplir con las siguientes especificaciones:

### 1. Archivo `requirements.txt`
Coloque en la raíz de su espacio de trabajo un archivo con las versiones congeladas de las librerías:
```text
transformers==4.48.0
torch==2.5.1
pandas==2.2.2
matplotlib==3.7.1
seaborn==0.13.1
spacy==3.8.14
