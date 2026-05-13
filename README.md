
## Maestría en Inteligencia Artificial
Aprendizaje Automático — Modelos No Supervisados | Grupo Nº 5 · 2026

![Python](https://img.shields.io/badge/python-3.10%2B-blue.svg)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-latest-orange.svg)
![Estado](https://img.shields.io/badge/status-completado-success.svg)
---

## 👤 Equipo de Trabajo
- **Harold Rodrigo Angulo Arellano**
- **Melissa Figallo Sánchez**
- **Jorge Javier Maldonado Mahauad**
- **Paula Elizabeth Noboa Ramírez**
---

## 📋 Tabla de Contenidos
1. [Descripción del Problema](#-dataset)
2. [Metodología](#-dataset)
3. [Modelos Implementados](#-dataset)
4. [Resultados Clave](#-dataset)
5. [Dataset](#-dataset)
6. [Licencia](#-dataset)
---


### Descripción del problema

> La educación es uno de los indicadores más representativos del nivel de desarrollo de una sociedad. Sin embargo, comparar sistemas educativos entre países es un reto complejo: las diferencias culturales, económicas y geográficas producen patrones de matrícula muy distintos entre regiones. En este contexto, las técnicas de aprendizaje no supervisado ofrecen una herramienta poderosa: permiten descubrir agrupaciones naturales en los datos sin necesidad de definir etiquetas o categorías a priori.

El presente informe aplica cuatro métodos complementarios —K-Means, DBSCAN, PCA y t-SNE— sobre indicadores educativos de la UNESCO para responder a la pregunta: ¿Es posible identificar grupos naturales de países con perfiles educativos similares, y cómo difieren los patrones encontrados según el método de aprendizaje no supervisado utilizado? 

---

## ⚙️ Metodología
El flujo de trabajo sigue las mejores prácticas de Ciencia de Datos:
1. **Análisis Exploratorio (EDA):** Limpieza y tratamiento de datos faltantes (UNESCO UIS).
2. **Preprocesamiento:** Transformación de formato largo a ancho (pivotado por País-Año).
3. **Ingeniería de Características:** Creación de variables de contexto histórico para evitar fugas de datos.
4. **Modelado:** Entrenamiento de 4 modelos de clasificación no supervisada: K-Means · DBSCAN · PCA · t-SNE
5. **Evaluación:** Comparación mediante métricas de AUC-ROC, F1-Score y precisión.

---  

## 🤖 Modelos Implementados
Comparamos el rendimiento de los siguientes algoritmos:
* **Decision Tree**
* **SVM (Linear & SVC)**
* **Random Forest** (Modelo recomendado)
* **Regresión Logística**
* **KNN**
* **Naive Bayes**
* **XGBoost**

---

## 📊 Resultados Clave
El modelo **Random Forest** demostró ser el más eficaz para la implementación.
* **AUC-ROC:** ~0.86
* **Interpretabilidad:** Alta, gracias a la importancia de las *features* integradas.
* **Robustez:** Excelente manejo de *outliers* y distribuciones no normales presentes en los indicadores globales.

> *Nota: Se recomienda utilizar el cuaderno `Grupo nro 5.ipynb` para visualizar los gráficos de importancia de variables y las curvas ROC.*

---


## Resumen

Este informe presenta un análisis comparativo de cuatro métodos de aprendizaje no supervisado aplicados a datos de matrícula escolar de 207 países, provenientes del Instituto de Estadística de la UNESCO. Los resultados muestran que existen grupos naturales y bien diferenciados en los sistemas educativos globales, siendo el nivel de cobertura de la educación secundaria la principal dimensión que separa a los países.

| **Modelo** | **Clusters** | **Silhouette** | **Davies-Bouldin** | **Rol principal** |
|---|---|---|---|---|
| K-Means (k=2) | 2 clusters | 0.4833 | 0.9533 | Segmentación |
| DBSCAN (eps=1.1, ms=5) | 2 + 31 outliers | 0.5328 | 0.5764 | Detección de anomalías |
| PCA (2 componentes) | — | 91.3% var. expl. | — | Reducción / visualización |
| t-SNE (perp=30) | — | Visual | — | Validación visual |

---

## 1. Introducción

La educación es uno de los indicadores más representativos del nivel de desarrollo de una sociedad. Sin embargo, comparar sistemas educativos entre países es un reto complejo: las diferencias culturales, económicas y geográficas producen patrones de matrícula muy distintos entre regiones. En este contexto, las técnicas de aprendizaje no supervisado ofrecen una herramienta poderosa: permiten descubrir agrupaciones naturales en los datos sin necesidad de definir etiquetas o categorías a priori.

El presente informe aplica cuatro métodos complementarios —K-Means, DBSCAN, PCA y t-SNE— sobre indicadores educativos de la UNESCO para responder a la pregunta: ¿existen grupos naturales de países con perfiles educativos similares, y cómo difieren los hallazgos según el método utilizado?

### 1.1 Justificación del dataset

El dataset contiene 5.765 registros sobre tasas de matrícula por país, año, nivel educativo (primaria, secundaria baja y secundaria alta) y género, abarcando 210 países entre los años 2000 y 2022. La riqueza multidimensional del dataset lo hace idóneo para clustering: cada país puede representarse como un vector de seis indicadores que captura tanto el nivel de acceso educativo como las diferencias de género entre niveles.

### 1.2 Objetivos específicos

- Construir un perfil educativo multidimensional para cada país a partir de las tasas de matrícula brutas.
- Identificar grupos de países con perfiles similares usando K-Means y DBSCAN.
- Analizar la estructura de varianza del espacio de indicadores con PCA.
- Validar visualmente la coherencia de los clusters con t-SNE.
- Comparar los cuatro métodos en términos de las estructuras que descubren.

---

## 2. Descripción del Dataset y Análisis Exploratorio

### 2.1 Estructura original

El dataset original se encuentra en formato largo (long format): cada fila representa una observación única identificada por la combinación país × año × tipo de indicador.

| **Campo** | **Tipo** | **Descripción** |
|---|---|---|
| País | Texto | Nombre del país o región |
| Año | Entero | Año del registro (2000–2022) |
| Tipo de Dato Educativo | Texto | Indicador específico (9 categorías) |
| Tasa | Decimal | Valor numérico del indicador (0–299) |

### 2.2 Hallazgos exploratorios clave

| **Variable** | **Media** | **Mediana** | **Desv. Est.** | **Mínimo** | **Máximo** | **CV (%)** |
|---|---|---|---|---|---|---|
| Prim_H | 104.9 | 107.6 | 12.8 | 30.1 | 143.6 | 12.2 |
| Prim_M | 102.1 | 104.9 | 14.0 | 16.6 | 150.0 | 13.7 |
| SecBaja_H | 88.7 | 97.0 | 25.2 | 10.2 | 182.1 | 28.4 |
| SecBaja_M | 87.2 | 94.8 | 27.5 | 4.7 | 170.0 | 31.5 |
| SecAlta_H | 73.4 | 74.1 | 34.4 | 6.9 | 215.2 | 46.9 |
| SecAlta_M | 75.5 | 76.3 | 36.7 | 3.2 | 193.6 | 48.6 |

**Hallazgos principales:**
- Primaria: distribución sesgada izquierda, cobertura casi universal global.
- Secundaria alta: indicador más heterogéneo (CV ≈ 47–49%).
- Brecha de género: especialmente visible en secundaria baja y alta.

### 2.3 Correlaciones entre indicadores

| | Prim_H | Prim_M | SecBaja_H | SecBaja_M | SecAlta_H | SecAlta_M |
|---|---|---|---|---|---|---|
| Prim_H | 1.00 | 0.91 | 0.22 | 0.21 | 0.02 | 0.06 |
| Prim_M | 0.91 | 1.00 | 0.39 | 0.42 | 0.19 | 0.26 |
| SecBaja_H | 0.22 | 0.39 | 1.00 | 0.97 | 0.74 | 0.80 |
| SecBaja_M | 0.21 | 0.42 | 0.97 | 1.00 | 0.73 | 0.81 |
| SecAlta_H | 0.02 | 0.19 | 0.74 | 0.73 | 1.00 | 0.96 |
| SecAlta_M | 0.06 | 0.26 | 0.80 | 0.81 | 0.96 | 1.00 |

**Patrones estructurales:**
1. Alta correlación intra-nivel entre géneros (r > 0.85)
2. Correlación moderada entre niveles contiguos (0.73–0.81)
3. Correlación débil entre primaria y secundaria alta (r ≈ 0.02–0.26)

---

## 3. Preprocesamiento — Diseño de Decisiones

### 3.1 Transformación de formato largo a ancho

Se pivotó el dataset para obtener una matriz donde cada fila es un país y cada columna es uno de los seis indicadores. La agregación temporal se realizó calculando la media por país sobre todos los años disponibles (2000–2022).

### 3.2 Filtro de calidad y umbral de completitud

Se estableció un umbral mínimo de 4 de 6 features con datos por país. Los países que no alcanzan este umbral se excluyen. Los valores faltantes se imputan con la mediana global de cada feature (robusta ante distribuciones con sesgo positivo).

### 3.3 Estandarización con StandardScaler

Tanto K-Means como DBSCAN calculan distancias euclidianas. Sin estandarización, las features con mayor rango numérico (SecAlta: 0–299%) dominarían las distancias. Se aplicó StandardScaler (media=0, desviación=1).

---

## 4. K-Means Clustering

### 4.1 Fundamentos

K-Means divide los datos en K grupos minimizando la inercia (suma de distancias cuadradas de cada punto a su centroide). Se utilizó inicialización k-means++.

### 4.2 Selección del número óptimo de clusters

| **k** | **Inercia** | **Silhouette** | **Observación** |
|---|---|---|---|
| 2 | 717.1 | **0.4833** | **ÓPTIMO** |
| 3 | 522.3 | 0.4355 | Baja el silhouette |
| 4 | 435.1 | 0.2645 | Codo visible |
| 5 | 385.2 | 0.2937 | Ganancia marginal |

**Resultado:** k=2 obtiene el Silhouette Score más alto (0.4833).

### 4.3 Perfiles de los clusters

| **Cluster** | **Prim_H** | **Prim_M** | **SecBaja_H** | **SecBaja_M** | **SecAlta_H** | **N** |
|---|---|---|---|---|---|---|
| Cluster 0 — Alta cobertura | 105.4 | 104.3 | 100.7 | 100.9 | 87.4 | 154 |
| Cluster 1 — Baja cobertura | 103.4 | 95.7 | 53.9 | 47.4 | 32.6 | 53 |

### 4.4 Limitaciones de K-Means

- Asume clusters de forma esférica y tamaño similar.
- Obliga a todos los países a pertenecer a un cluster (incluyendo outliers).
- Sensible a outliers.

---

## 5. DBSCAN — Clustering Basado en Densidad

### 5.1 Fundamentos

DBSCAN agrupa puntos en regiones de alta densidad sin necesidad de especificar K. Los puntos que no alcanzan ninguna región densa se etiquetan como ruido (-1). Esta propiedad es clave para identificar países con perfiles genuinamente atípicos.

### 5.2 Selección de parámetros (Grid Search)

| **eps** | **min_samples** | **N Clusters** | **N Ruido** | **% Ruido** | **Silhouette** |
|---|---|---|---|---|---|
| **1.1** | **5** | 2 | 31 | 15.0% | **0.5328 ★** |
| 1.0 | 5 | 2 | 33 | 15.9% | 0.5219 |
| 1.2 | 5 | 2 | 25 | 12.1% | 0.5105 |

**Configuración seleccionada:** eps=1.1, min_samples=5

### 5.3 Resultados

- **2 clusters genuinos** (países con perfiles similares)
- **31 países identificados como ruido** (outliers genuinos)

**Países ruido (selección):** Afganistán, Australia, Bélgica, Burundi, República Centroafricana, Eritrea, Gambia, Madagascar, Malawi, Mozambique, Níger, Ruanda, Sierra Leona, Somalia, Sudán del Sur, Uganda, Zambia, entre otros.

---

## 6. PCA — Análisis de Componentes Principales

### 6.1 Fundamentos y doble rol

PCA transforma las 6 variables originales en componentes principales ortogonales que maximizan progresivamente la varianza explicada. Cumple dos roles: (1) herramienta de análisis (dimensiones latentes), (2) herramienta de visualización.

### 6.2 Varianza explicada

| **Componente** | **Varianza explicada** | **Varianza acumulada** | **Interpretación** |
|---|---|---|---|
| PC1 | 62.3% | 62.3% | Desarrollo educativo general |
| PC2 | 29.0% | **91.3%** | Brecha primaria vs. secundaria |
| PC3 | 6.3% | 97.6% | Variación residual |
| PC4-PC6 | 2.4% | 100% | Ruido / microdiferencias |

> Solo 2 componentes explican el 91.3% de toda la varianza.

### 6.3 Interpretación de los componentes

| **Variable** | **Loading PC1** | **Loading PC2** |
|---|---|---|
| Prim_H | 0.180 | **+0.693** |
| Prim_M | 0.278 | **+0.620** |
| SecBaja_H | **+0.484** | -0.076 |
| SecBaja_M | **+0.486** | -0.067 |
| SecAlta_H | **+0.445** | -0.268 |
| SecAlta_M | **+0.471** | -0.231 |

- **PC1 — "Desarrollo educativo general"**: carga alta en Secundaria (0.44–0.49)
- **PC2 — "Brecha primaria vs. secundaria"**: carga positiva en Primaria (0.62–0.69), negativa en Secundaria Alta

---

## 7. t-SNE — Visualización No Lineal

### 7.1 Fundamentos

t-SNE preserva la estructura local del espacio de alta dimensión en una proyección 2D. A diferencia de PCA, es no lineal y está diseñado específicamente para visualización. **No es un algoritmo de clustering**, sino una herramienta de validación visual.

### 7.2 Parámetros

| **Parámetro** | **Valor usado** | **Descripción** |
|---|---|---|
| perplexity | 10, 30, 50 | Vecinos efectivos considerados |
| learning_rate | 200 | Velocidad de ajuste iterativo |
| max_iter | 1000 | Iteraciones hasta convergencia |
| random_state | 42 | Semilla para reproducibilidad |

Se seleccionó **perplexity=30** (recomendación estándar de los autores originales).

### 7.3 Rol en el análisis

- Validar visualmente que los clusters de K-Means y DBSCAN tienen separación geométrica real.
- Detectar posibles subclusters no capturados.
- Identificar visualmente outliers (puntos DBSCAN etiquetados como ruido aparecen en márgenes).

> **Advertencia metodológica:** Las distancias entre clusters en t-SNE NO son interpretables cuantitativamente. Solo informa sobre separabilidad local.

---

## 8. Evaluación Comparativa de Métodos

### 8.1 Métricas de evaluación interna

| **Métrica** | **Rango** | **Mejor valor** | **Qué mide** |
|---|---|---|---|
| Silhouette Score | [-1, 1] | Cercano a 1 | Cohesión vs. separación |
| Davies-Bouldin Index | [0, ∞) | Cercano a 0 | Similaridad entre clusters |
| Calinski-Harabasz | [0, ∞) | Mayor es mejor | Ratio inter/intra-cluster |

### 8.2 Comparativa cuantitativa

| **Modelo** | **Silhouette** | **Davies-Bouldin** | **Calinski-Harabasz** |
|---|---|---|---|
| K-Means (k=2) | 0.4833 | 0.9533 | **150.04 ★** |
| DBSCAN (eps=1.1, ms=5) | **0.5328 ★** | **0.5764 ★** | 72.38 |

### 8.3 Tabla comparativa completa

| **Dimensión** | **K-Means** | **DBSCAN** | **PCA** | **t-SNE** |
|---|---|---|---|---|
| Tipo de método | Clustering | Clustering | Reducción dim. | Reducción dim. |
| Produce clusters | Sí | Sí + ruido | No | No |
| Requiere K a priori | Sí | No | No | No |
| Detecta outliers | No (asigna) | Sí (etiqueta -1) | Parcial | Visual |
| Interpretabilidad | Alta | Media | Alta (loadings) | Baja |
| Escalabilidad | Alta | Media | Alta | Baja O(n²) |

---

## 9. Conclusiones Principales

### 9.1 Respuesta a la pregunta de investigación

**Sí.** Los cuatro métodos aplicados convergen en identificar una estructura de agrupación real y estadísticamente significativa en los datos educativos de la UNESCO. La estructura no es trivial ni artefacto del preprocesamiento: tiene respaldo en múltiples métricas cuantitativas (Silhouette 0.48–0.53) y en el PCA (91.3% de varianza explicada por solo 2 componentes).

### 9.2 Perfiles educativos identificados

| **Perfil** | **N Países** | **Características clave** | **Regiones típicas** |
|---|---|---|---|
| Cluster 0 — Alta cobertura | 154 (74%) | Primaria~105%, SecBaja~101%, SecAlta~87–92% | Europa, América Latina, Asia Oriental |
| Cluster 1 — Baja cobertura | 53 (26%) | Primaria~103% pero SecBaja cae a 51–54%, SecAlta a 29–33% | África Subsahariana, Asia Meridional |
| Ruido DBSCAN | 31 (15%) | Perfiles únicos: extremadamente bajos o micro-territorios | Dispersos globalmente |

**Hallazgo clave:** La brecha entre países no está en la educación primaria (ambos clusters tienen tasas similares), sino en la **transición a la secundaria**.

### 9.3 Diferencias clave entre los modelos

| **Aspecto** | **K-Means** | **DBSCAN** |
|---|---|---|
| Tratamiento de Somalia | Asignado al Cluster 1 (baja cobertura) | Marcado como RUIDO (perfil extremo) |
| Tratamiento de Mónaco | Asignado al Cluster 0 (alta cobertura) | Marcado como RUIDO (micro-estado) |
| Calidad de separación | Silhouette=0.4833 | Silhouette=0.5328 (mejor) |

### 9.4 Limitaciones y trabajo futuro

**Limitaciones:**
- Datos faltantes imputados con medianas (suaviza diferencias)
- Tasas brutas vs. netas (las brutas pueden superar 100%)
- PCA asume relaciones lineales
- Solo 6 variables educativas (sin PISA, inversión pública)

---

## Sobre el dataset datos_educativos.csv

El dataset datos_educativos.csv, contiene información educativa que incluye tasas de matrícula. Está estructurado con registros para diferentes países, años y tipos de datos educativos, junto con la tasa correspondiente y la fuente de los datos.

Origen: Los datos fueron recolectados del archivo datos_educativos.csv.

Documentación del Dataset de Datos Educativos de la ONU

https://www.kaggle.com/datasets/isabelocastillo/datos-educativos-globales/data

Variables Disponibles: Las variables disponibles en el dataset original son:
- Índice: Un identificador numérico de fila.
- ID: Un identificador numérico.
- País: La variable categórica que representa el país.
- Año: El año del registro de la tasa.
- Tipo de Dato Educativo: La categoría específica de la tasa educativa (ej. Tasa Matrícula Primaria (Mujeres)).
- Tasa: El valor numérico de la tasa educativa.
- Fuente Datos: La fuente de donde se obtuvo el dato educativo.

---


## ⚖️ Licencia
Este proyecto se distribuye bajo la licencia MIT. 

*Fuente de datos: UNESCO Institute for Statistics (UIS) — último acceso: abril 2023.*

---
