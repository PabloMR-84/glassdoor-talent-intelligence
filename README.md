# 📊 Glassdoor Intelligence: Evaluación del Ecosistema Laboral de Nokia vs. Big Tech

[📄 Ver Presentación Ejecutiva (PDF)](reports/presentacion_autoexplicativa_glassdoor_nokia.pdf)

### 1) Objetivo
Este proyecto aplica técnicas avanzadas de **Procesamiento de Lenguaje Natural (NLP)** y **Deep Learning** para transformar datos no estructurados en insights estratégicos de *People Analytics*. Su objetivo principal es evaluar la marca empleadora (*Employer Branding*) de Nokia frente a la competencia global (Google, Apple, Microsoft, Uber), identificando con precisión los motivos de fuga de talento (*churn drivers*) y midiendo las palancas reales de atracción (*Employee Value Proposition*).

### 2) Fuentes de Datos
Para simular un entorno competitivo real y aislar las variables del mercado tecnológico, se trabaja sobre un conjunto masivo de datos:
1. **Dataset Original (Glassdoor Reviews)**: Repositorio completo con cientos de miles de opiniones de empleados de diversas industrias y firmas a nivel global.
2. **Filtrado y Muestreo Estratégico (10.000 Reseñas)**: 
   - Para equilibrar el peso de cada firma y optimizar la carga computacional en los modelos Transformer y BERTopic, se realiza una selección balanceada de **2.000 opiniones por empresa**.
   - *Target*: Nokia.
   - *Peers de Industria*: Google, Apple, Microsoft y Uber.
   - *Periodo Analítico*: 2018 - Presente.

### 3) Ingeniería de Datos y Control de Calidad Textual
El proceso de preparación (ETL) abordó las inconsistencias típicas de los textos de scraping:
- **Tratamiento de Metadatos**: Separación de las variables de situación (empleados *Actuales* vs. *Antiguos*) y antigüedad. Imputación de nulos en variables categóricas de recomendación.
- **Depuración de Textos Relevantes**: Eliminación de registros vacíos, cadenas irrisorias (ruido de <4 caracteres) y saltos de línea para asegurar representaciones semánticas reales en los campos de `pros` y `cons`.

### 4) Deep Learning y Análisis de Sentimiento
Para superar las limitaciones del análisis léxico tradicional, se implementó una comparativa metodológica:
- **Enfoque Léxico (VADER)**: Demostró ser insuficiente para el entorno corporativo, clasificando correctamente solo el 38.6% de las críticas (*Cons*) debido a su incapacidad para entender sarcasmo corporativo y quejas educadas.
- **Arquitectura Transformer (DistilBERT)**: Implementación de un modelo preentrenado de Hugging Face (`distilbert-base-uncased-finetuned-sst-2-english`) con inferencia por lotes (*batching*). Gracias a su mecanismo de **Auto-Atención**, la precisión en la detección de críticas verdaderas saltó al **78.6% (+40.0 pp)**, filtrando el ruido real generado en el scraping.

### 5) Modelado de Tópicos (BERTopic) y Mapeo de Negocio
Se diseñó un pipeline de aprendizaje no supervisado para agrupar temáticamente las opiniones del subconjunto seleccionado:
- **Embeddings y Dimensionalidad**: Vectorización contextual densa (`all-MiniLM-L6-v2`) y reducción a 2D/5D mediante **UMAP**.
- **Clustering (HDBSCAN)**: Agrupación natural de comentarios por afinidad semántica.
- **Representación c-TF-IDF**: Extracción de los términos más representativos de cada clúster.
- **Traducción a RRHH**: Los clústeres matemáticos fueron mapeados manualmente a dimensiones estratégicas de negocio (*Cultura y Entorno Laboral, Desarrollo y Promoción, Liderazgo, Compensación, Conciliación*).

### 6) Insights Principales y Diagnóstico Estratégico
- **El Estancamiento como Churn Driver en Nokia**: Al comparar empleados actuales frente a antiguos, se detectó que el **45.9%** de las quejas de quienes abandonan la empresa se concentra en *Desarrollo y Promoción*. El vocabulario crítico (extraído vía c-TF-IDF: *promotion, career, hard*) confirma el esfuerzo no recompensado.
- **Liderazgo en Conciliación**: Nokia posee una ventaja competitiva masiva en *Conciliación y Flexibilidad* (**+31.7 pp** de Balance Neto frente a la media de Big Tech).
- **Matriz de Priorización**: Se diseñó una matriz 2x2 interactiva (Python/Matplotlib) cruzando *Frecuencia de la queja* vs *Desventaja frente a peers*, donde el tamaño de la burbuja refleja el impacto real en la fuga de talento, delineando una hoja de ruta clara de intervención urgente.

### 7) Hoja de Ruta para Recursos Humanos
- **Retención (Foco Interno)**: Auditoría y rediseño urgente de los itinerarios de carrera profesional y planes de promoción para desbloquear el cuello de botella organizativo y frenar el *churn*.
- **Atracción (Foco Externo)**: Campaña de *Talent Targeting* utilizando la flexibilidad laboral y la conciliación como núcleo de la Propuesta de Valor (EVP) para captar talento desencantado por las políticas presenciales de competidores como Apple o Uber.

### 8) Estructura del Proyecto

### 9) Instrucciones de Uso

1. **Clonar el repositorio** en tu equipo local:
   ```bash
   git https://github.com/PabloMR-84/glassdoor-talent-intelligence
   cd nokia-glassdoor-nlp
   ```

2. **Crear y activar un entorno virtual (`.venv`)**:
   - **En Linux / macOS:**
     ```bash
     python3 -m venv .venv
     source .venv/bin/activate
     ```
   - **En Windows:**
     ```bash
     python -m venv .venv
     source .venv\Scripts\activate
     ```

3. **Instalar dependencias**:
   ```bash
   pip install --upgrade pip
   pip install -r requeriments.txt
   ```

4. **Descargar y ubicar los datos**:
   - Descarga el dataset original `glassdoor_reviews_hr.parquet` desde [este enlace de Google Drive](https://drive.google.com/file/d/1FUcrrw35FQHKii4megWQPAC1KUajJ9i0/view?usp=sharing).
   - Crea la carpeta `data/raw/` si no existe y coloca ahí el archivo descargado:
     ```text
     data/raw/glassdoor_reviews_hr.parquet
     ```

5. **Ejecución del Pipeline**: Abre y ejecuta el notebook o script principal. El código filtrará automáticamente la muestra de 2.000 opiniones por firma para proceder con el ETL, la inferencia con DistilBERT y el modelado de tópicos con BERTopic.
   > **Nota**: Se recomienda contar con soporte para GPU (CUDA) para acelerar la inferencia del modelo Transformer.

### 10) Tecnologías Utilizadas
- **Python**: Lenguaje principal de análisis y desarrollo.
- **Deep Learning / NLP**: Hugging Face `transformers` (DistilBERT), `sentence-transformers`, NLTK (VADER).
- **Topic Modeling**: `BERTopic`, `UMAP` (Reducción de dimensionalidad), `HDBSCAN` (Clustering denso).
- **Manipulación de Datos**: `pandas`, `numpy`, `pyarrow`.
- **Visualización Analítica**: `matplotlib`, `seaborn` (Heatmaps, Matriz de burbujas, Gráficos de barras apiladas).

---

## 👤 Autor
- **Pablo Muñoz**
*Data Science / HR & People Analytics*
