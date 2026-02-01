# 🛡️ Identificación y Clasificación Automática de Logs con IA

![Python](https://img.shields.io/badge/Python-3.9-blue?style=for-the-badge&logo=python&logoColor=white)
![GCP](https://img.shields.io/badge/Google_Cloud-Vertex_AI-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-Machine_Learning-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

> **Resumen:** Implementación de un pipeline de **Machine Learning (AIOps)** para la ingesta, procesamiento y clasificación automática de *Windows Event Logs* en entornos empresariales híbridos. El sistema detecta anomalías y clasifica la severidad de eventos utilizando algoritmos supervisados y no supervisados sobre infraestructura Cloud.

---

## 📖 Sobre el Proyecto

Las infraestructuras modernas generan volúmenes exponenciales de logs, haciendo imposible su revisión manual. Este proyecto soluciona este problema mediante:

1.  **Ingesta de Datos:** Extracción de logs (*Application, Security, System*) de servidores empresariales (anonimizados como Zeus, Poseidon, Ares).
2.  **Log Parsing:** Transformación de datos semi-estructurados (XML) a tabulares (CSV/SQL).
3.  **Machine Learning:** Entrenamiento de modelos para predecir la severidad (`Critical`, `Error`, `Warning`, `Info`) y agrupar patrones desconocidos.

---

## 🛠️ Stack Tecnológico y Herramientas

El proyecto se desplegó utilizando una arquitectura **Multi-Cloud** para validar la portabilidad y eficiencia de costos.

### ☁️ Infraestructura Cloud

| Plataforma | Servicio | Uso en el Proyecto |
| :--- | :--- | :--- |
| **Google Cloud** | ![Vertex AI](https://img.shields.io/badge/-Vertex_AI-4285F4?logo=google-cloud&logoColor=white) | **Vertex AI Workbench:** Entorno principal para desarrollo de Notebooks Jupyter y entrenamiento de modelos. |
| | ![BigQuery](https://img.shields.io/badge/-BigQuery-4285F4?logo=google-cloud&logoColor=white) | **BigQuery:** Data Warehouse para consultas SQL de alto rendimiento sobre el dataset consolidado. |
| | ![Cloud Storage](https://img.shields.io/badge/-Cloud_Storage-4285F4?logo=google-cloud&logoColor=white) | **Cloud Storage:** Almacenamiento de objetos para los archivos CSV crudos y procesados. |
| **Microsoft Azure** | ![Azure ML](https://img.shields.io/badge/-Azure_ML_Studio-0078D4?logo=microsoft-azure&logoColor=white) | **ML Studio (Serverless Spark):** Entorno alternativo de ejecución distribuida para validación cruzada. |
| | ![Blob Storage](https://img.shields.io/badge/-Blob_Storage-0078D4?logo=microsoft-azure&logoColor=white) | **Blob Storage:** Repositorio de datos para el entorno Microsoft. |

### 🧠 Machine Learning & Data Science

| Categoría | Herramienta | Función |
| :--- | :--- | :--- |
| **Lenguaje** | ![Python](https://img.shields.io/badge/-Python_3.9-3776AB?logo=python&logoColor=white) | Lenguaje base del proyecto. |
| **Modelado** | ![Scikit-Learn](https://img.shields.io/badge/-Scikit--Learn-F7931E?logo=scikit-learn&logoColor=white) | Algoritmos: **Random Forest, SVM, K-Means, Isolation Forest**. |
| **Datos** | ![Pandas](https://img.shields.io/badge/-Pandas-150458?logo=pandas&logoColor=white) | Manipulación de DataFrames y limpieza de datos (ETL). |
| **NLP** | ![TF-IDF](https://img.shields.io/badge/-TF--IDF-grey) | Vectorización de texto para procesar los mensajes de los logs. |
| **Viz** | ![Seaborn](https://img.shields.io/badge/-Seaborn-blue) | Generación de matrices de correlación y gráficas de distribución. |

---

## 📊 Metodología y Modelos

El flujo de trabajo siguió un enfoque cuantitativo riguroso:

### 1. Preprocesamiento (ETL)
* **Limpieza:** Eliminación de duplicados y normalización de timestamps.
* **Feature Engineering:** Codificación de variables categóricas (`ProviderName`) y vectorización de texto (`Message`).
* **Balanceo:** Manejo del desbalance de clases (predominancia de logs "Information") mediante `class_weight='balanced'`.

### 2. Modelos Implementados
* 🌲 **Random Forest Classifier:** El modelo "ganador". Robusto, interpretable y eficiente. Hiperparámetros optimizados: `n_estimators=200`, `max_depth=30`.
* 📈 **Support Vector Machine (SVM):** Evaluado como comparativa, con kernel RBF.
* 🧪 **K-Means Clustering:** Aprendizaje no supervisado para detectar 6 clústeres naturales de eventos (K=6).

---

## 🏆 Resultados Clave

El sistema demostró una alta capacidad para clasificar incidentes operativos:

| Métrica | Resultado (Random Forest) | Análisis |
| :--- | :---: | :--- |
| **Accuracy** | **87.3%** | Supera el umbral de éxito del 80% definido en los objetivos. |
| **F1-Score** | **0.85** | Balance óptimo entre precisión y exhaustividad. |
| **Eficiencia** | **45 seg** | Tiempo de entrenamiento significativamente menor que SVM (187s). |

> **Nota:** En pruebas de validación temporal post-experimento, el modelo demostró robustez frente al paso del tiempo, manteniendo métricas estables.

---

## 🚀 Cómo Ejecutar (Snippet)

Este proyecto requiere Python 3.8+ y las librerías listadas en `requirements.txt`.

```python
# 1. Instalar dependencias
# pip install pandas scikit-learn google-cloud-bigquery matplotlib seaborn

# 2. Cargar datos desde BigQuery (Ejemplo)
from google.cloud import bigquery
client = bigquery.Client()
query = "SELECT * FROM `tu_proyecto.dataset.tabla_logs`"
df = client.query(query).to_dataframe()

# 3. Entrenar Modelo (Ejemplo Random Forest)
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier(n_estimators=200, class_weight='balanced')
model.fit(X_train, y_train)