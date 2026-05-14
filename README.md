
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

La educación es uno de los indicadores más representativos del nivel de desarrollo de una sociedad. Sin embargo, comparar sistemas educativos entre países es un reto complejo: las diferencias culturales, económicas y geográficas producen patrones de matrícula muy distintos entre regiones. En este contexto, las técnicas de aprendizaje no supervisado ofrecen una herramienta poderosa: permiten descubrir agrupaciones naturales en los datos sin necesidad de definir etiquetas o categorías a priori.

El presente informe aplica cuatro métodos complementarios —K-Means, DBSCAN, PCA y t-SNE— sobre indicadores educativos de la UNESCO para responder a la pregunta: 
**¿Es posible identificar grupos naturales de países con perfiles educativos similares, y cómo difieren los patrones encontrados según el método de aprendizaje no supervisado utilizado?**

---

## ⚙️ Metodología
Para garantizar la validez científica del análisis, seguimos un flujo de trabajo estructurado:

1.  **Curación de Datos:** Se consolidaron indicadores de matrícula bruta en primaria, secundaria baja y secundaria alta.
2.  **Tratamiento de Datos Faltantes:** Dado que no todos los países reportan cada año, se aplicó una imputación basada en la mediana para mantener la representatividad sin sesgar los resultados.
3.  **Ingeniería de Características:** Los datos fueron normalizados utilizando `StandardScaler`. Esto es vital en aprendizaje no supervisado para evitar que las variables con rangos más amplios dominen injustamente el cálculo de distancias.
4.  **Validación Cruzada de Modelos:** No nos fiamos de un solo algoritmo; contrastamos métodos de partición (K-Means) con métodos de densidad (DBSCAN) y técnicas de reducción de dimensionalidad (PCA y t-SNE).

---
## 🤖 Modelos Implementados
El proyecto utiliza una combinación estratégica de algoritmos para extraer diferentes "capas" de información:

* **K-Means (Segmentación):** Lo usamos para encontrar el número óptimo de grupos (k=2). Es el modelo que nos da la "foto general" de la división educativa mundial.
* **DBSCAN (Detección de Ruido):** A diferencia de K-Means, este modelo no obliga a todos los países a pertenecer a un grupo. Nos permitió identificar **31 países "atípicos"** que tienen comportamientos educativos únicos o extremos.
* **PCA (Reducción de Dimensionalidad):** Logramos comprimir la información de todas las variables en solo 2 componentes principales que explican el **91.3% de la varianza**, permitiéndonos entender qué factores pesan más en la brecha educativa.
* **t-SNE (Visualización Avanzada):** Utilizado para mapear los datos en un espacio bidimensional y confirmar visualmente que los grupos identificados por los otros algoritmos son realmente cohesivos.

---

## 🏆 Resultados Clave
Nuestros hallazgos desafían algunas percepciones comunes sobre la educación global:

* **El "Cuello de Botella" es la Secundaria:** La diferencia real entre los países desarrollados y en desarrollo no está en la primaria (donde casi todos tienen coberturas cercanas al 100%), sino en la **retención en secundaria alta**, donde la cobertura cae drásticamente al 32% en el grupo menos favorecido.
* **Dos Realidades Marcadas:** El mundo se divide naturalmente en dos perfiles: uno de alta continuidad educativa (154 países) y otro de deserción temprana (53 países).
* **Identificación de Casos Críticos:** El modelo detectó anomalías severas en países como Somalia y Sudán del Sur, pero también marcó como "outliers" a países como Australia o Bélgica debido a patrones de reporte o estructuras educativas que no siguen la tendencia global.
* **Efectividad del PCA:** Se demostró que la "salud educativa" de un país se puede medir con una altísima precisión analizando solo la transición entre niveles escolares, más que los niveles aislados.
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
