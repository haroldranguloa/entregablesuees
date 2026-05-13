# Reporte de Figuras — Análisis No Supervisado UNESCO

---

## 1. Resumen de Figuras
https://github.com/haroldranguloa/entregablesuees/tree/main/Imagenes

| Título | Descripción de la Imagen |
|--------|--------------------------|
| **EDA Distribuciones y Atípicos por Nivel Educativo y Género** | Seis paneles: histogramas con curvas KDE (fila superior) y diagramas de caja con outliers (fila inferior) para Primaria, Secundaria Baja y Secundaria Alta, desagregados por hombres y mujeres. |
| **Correlación de Pearson entre Features / Primaria vs. Secundaria Alta por Género** | Panel izquierdo: matriz de correlación de Pearson entre las 6 variables educativas. Panel derecho: dispersión de Tasa de Matrícula Primaria vs. Secundaria Alta distinguiendo hombres (círculos) y mujeres (triángulos). |
| **Selección del número óptimo de clusters para K-Means** | Dos gráficos: Método del Codo (Elbow) que marca k=4 como punto de inflexión, y Silhouette Score que identifica k=2 como el valor con mayor puntuación (~0.50). |
| **K-Means Visualización de Clusters** | Panel izquierdo: dispersión de países en espacio PCA 2D coloreados por cluster K-Means (k=2) con centroides marcados. Panel derecho: gráfico de radar (spider chart) con el perfil normalizado de cada cluster en las 6 variables. |
| **K-Distance Graph Selección de eps para DBSCAN** | Curva de distancia al 7.º vecino más cercano ordenada de mayor a menor, con eps sugerido ≈ 3.2 (línea discontinua azul) para min_samples=7. |
| **DBSCAN Clusters y Perfiles Educativos** | Panel izquierdo: clusters DBSCAN (eps=1.1, ms=5) en espacio PCA 2D con puntos de ruido (-1) marcados con ×. Panel derecho: gráfico de barras del perfil medio por cluster en las 6 variables educativas. |
| **PCA Estructura de Varianza y Loadings** | Tres subgráficos: varianza explicada por componente (PC1=62.3%, PC2=29.0%), varianza acumulada con umbrales 80% y 90%, y biplot de contribución de variables a PC1 y PC2. |
| **PCA Posicionamiento de países y clusters** | Panel izquierdo: mapa PCA 2D de países coloreado por cluster K-Means con etiquetas de casos extremos. Panel derecho: ranking horizontal de países extremos en PC1 (desarrollo educativo general). |
| **t-SNE Impacto de la perplejidad** | Tres proyecciones t-SNE (perplexity=10, 30, 50) coloreadas por asignación K-Means, mostrando cómo varía la separación y compacidad de los clusters al cambiar el hiperparámetro. |
| **t-SNE Validación visual de estructuras de clustering** | Dos proyecciones t-SNE (perp=30): panel izquierdo coloreado por K-Means (k=2) y panel derecho coloreado por DBSCAN (eps=1.1, ms=5), para comparar la coherencia visual de ambos métodos. |
| **Comparativa de los 4 Métodos No Supervisados** | Panel 2×2: K-Means (k=2) en PCA 2D, DBSCAN en PCA 2D, PCA Biplot (91% varianza explicada) y t-SNE (perp=30 coloreado por K-Means), todos en el mismo espacio visual para facilitar la comparación. |

---

## 2. Detalles de las Figuras

---

### Figura 1: EDA Distribuciones y Atípicos por Nivel Educativo y Género

**Descripción**

La figura presenta un análisis exploratorio de datos (EDA) organizado en dos filas y tres columnas. La fila superior muestra distribuciones de densidad (histogramas + KDE) para cada nivel educativo —Primaria, Secundaria Baja y Secundaria Alta— separadas por género (hombres en azul, mujeres en rojo). La fila inferior contiene diagramas de caja (boxplots) con los outliers individuales identificados, incluyendo el rango intercuartílico (IQR) y el conteo de valores atípicos por grupo.

**Valor Analítico**

- **Primaria:** Las distribuciones de hombres y mujeres están muy próximas y centradas alrededor del 100%, indicando paridad de género en la mayoría de los países. Sin embargo, se detectan 13 outliers en hombres y 26 en mujeres (IQR ≈ 9.4 para mujeres), lo que refleja países con tasas anómalas, tanto por exceso como por déficit.
- **Secundaria Baja:** Mayor dispersión que en primaria (IQR ≈ 26.2 para ambos géneros). Los outliers ascienden a 14 (hombres) y 17 (mujeres), con casos extremos por debajo de 25%, evidenciando países con acceso muy limitado a este nivel.
- **Secundaria Alta:** Es el nivel con mayor variabilidad (IQR ≈ 52.4 en mujeres). Las distribuciones presentan colas largas y menor concentración en el 100%, lo que sugiere que la retención en educación secundaria alta es el mayor reto sistémico. Solo 2 outliers en cada género, pero la dispersión intrínseca es elevada.
- En conjunto, la figura permite identificar que la desigualdad de género en la matrícula es más pronunciada en los niveles secundarios que en primaria, y que la heterogeneidad entre países aumenta conforme sube el nivel educativo.

---

### Figura 2: Correlación de Pearson y Dispersión Primaria vs. Secundaria Alta

**Descripción**

El panel izquierdo es una matriz de correlación de Pearson triangular inferior entre las seis variables: `Prim_H`, `Prim_M`, `SecBaja_H`, `SecBaja_M`, `SecAlta_H` y `SecAlta_M`. La intensidad del verde indica la fuerza de la correlación positiva. El panel derecho es un gráfico de dispersión que enfrenta la tasa de matrícula en primaria (eje X) contra secundaria alta (eje Y), diferenciando hombres (círculos azules) y mujeres (triángulos rojo-rosado).

**Valor Analítico**

- **Correlaciones intra-género en primaria:** `Prim_H` y `Prim_M` tienen una correlación de 0.91, la más alta de la matriz, confirmando que los países con alta matrícula masculina en primaria también presentan alta matrícula femenina.
- **Correlaciones intra-nivel secundario:** `SecBaja_H`–`SecBaja_M` = 0.97 y `SecAlta_H`–`SecAlta_M` = 0.96, indicando que dentro de cada nivel secundario la paridad de género es estructural: los países avanzan o retroceden conjuntamente para ambos sexos.
- **Correlaciones entre niveles:** La relación entre primaria y secundaria alta es débil (0.02 a 0.26), mientras que secundaria baja y alta están más correlacionadas (0.73–0.81). Esto sugiere que superar la barrera primaria no garantiza la progresión a secundaria alta.
- **Dispersión Primaria vs. Secundaria Alta:** Se observa una nube densa alrededor de matrícula primaria ≈ 100% con alta variabilidad en secundaria alta (0%–220%), validando visualmente la baja correlación entre ambos niveles. No se aprecia una brecha de género sistemática en este gráfico: los puntos de hombres y mujeres se mezclan, aunque con valores extremos dispersos.

---

### Figura 3: Selección del Número Óptimo de Clusters para K-Means

**Descripción**

Dos gráficos de línea: el Método del Codo (izquierda) muestra la inercia (suma de distancias al cuadrado) frente al número de clusters k (de 2 a 10), marcando k=4 como punto de inflexión. El Silhouette Score (derecha) grafica la cohesión y separación media para cada k, con el máximo en k=2 (~0.50).

**Valor Analítico**

- **Método del Codo:** La inercia disminuye de ~720 (k=2) a ~225 (k=10). El codo más pronunciado ocurre en k=4, donde la reducción marginal de inercia comienza a aplanarse. Sin embargo, este criterio por sí solo no es concluyente.
- **Silhouette Score:** El valor máximo se alcanza en k=2 (~0.50), descendiendo bruscamente a k=3 (~0.43) y mínimo en k=4 (~0.25). Esto indica que la partición en dos grupos produce los clusters más compactos y bien separados.
- **Decisión metodológica:** La discrepancia entre ambos criterios (k=4 vs. k=2) se resuelve a favor de k=2 dado que el Silhouette Score es el indicador más robusto de calidad intrínseca de clustering. Esto concuerda con la estructura bimodal que revelan el PCA y el t-SNE.

---

### Figura 4: K-Means Visualización de Clusters

**Descripción**

Panel izquierdo: mapa de dispersión de países en el espacio PCA 2D (PC1 explica 62.3%, PC2 el 29.0%) con puntos coloreados según su asignación al Cluster 0 (azul oscuro) o Cluster 1 (cian), y los dos centroides marcados con ×. Panel derecho: gráfico de radar (spider/polar chart) con los perfiles normalizados de ambos clusters en las 6 variables educativas.

**Valor Analítico**

- **Separación espacial:** En el espacio PCA, el Cluster 1 (cian) se ubica predominantemente en valores negativos de PC1 (izquierda), mientras que el Cluster 0 (azul) concentra la mayoría de países en valores positivos de PC1 (derecha), indicando que PC1 captura el principal eje de diferenciación.
- **Perfil del Cluster 0 (desarrollo alto):** El radar muestra valores normalizados cercanos a 0.8–0.95 en Primaria (H y M) y Secundaria Baja (H y M), pero más bajos en Secundaria Alta (~0.6–0.8). Estos son países con sistemas educativos relativamente consolidados.
- **Perfil del Cluster 1 (desarrollo bajo):** Valores normalizados marcadamente inferiores en todas las variables, especialmente en Secundaria Alta (≈0.1–0.2), representando países con acceso educativo limitado en todos los niveles.
- **Interpretación:** La separación binaria refleja una brecha global entre países con sistemas educativos inclusivos y aquellos con rezago estructural, siendo la secundaria alta la variable más discriminante.

---

### Figura 5: K-Distance Graph Selección de eps para DBSCAN

**Descripción**

Gráfico de línea que muestra la distancia al 7.º vecino más cercano (eje Y) de cada punto ordenado de mayor a menor distancia (eje X), con una línea discontinua azul que indica el eps sugerido ≈ 3.2.

**Valor Analítico**

- **Criterio del codo:** El gráfico K-Distance se usa para identificar el punto de inflexión ("codo") donde la curva comienza a aplanarse, lo que determina el valor de eps para DBSCAN. El codo se localiza aproximadamente en las primeras posiciones (puntos 1–5), con una distancia alrededor de 3.2.
- **Interpretación del eps sugerido:** Con eps ≈ 3.2 y min_samples=7, los puntos cuya distancia al 7.º vecino supera 3.2 serán clasificados como ruido (-1). La gráfica muestra que una fracción reducida de puntos supera este umbral, coherente con el bajo porcentaje de ruido observado en el resultado final de DBSCAN.
- **Limitación observada:** En la implementación final se usó eps=1.1 con ms=5 (más restrictivo que el sugerido por la gráfica), lo que produjo más puntos de ruido. Esto sugiere que el eps sugerido por el k-distance graph fue ajustado manualmente para obtener clusters más puros.

---

### Figura 6: DBSCAN Clusters y Perfiles Educativos

**Descripción**

Panel izquierdo: proyección en espacio PCA 2D de los clusters identificados por DBSCAN (eps=1.1, ms=5): Cluster 0 (verde azulado), Cluster 1 (verde amarillento) y puntos de ruido (-1) marcados con ×. Panel derecho: gráfico de barras agrupadas con la tasa media de matrícula por cluster en las 6 variables.

**Valor Analítico**

- **Estructura de clusters:** DBSCAN identifica dos clusters densos y un conjunto considerable de puntos de ruido. El Cluster 0 agrupa la mayoría de los países (región central y derecha del espacio PCA), mientras que el Cluster 1 es más pequeño y se localiza en la zona de transición izquierda.
- **Puntos de ruido:** Los países marcados como ruido son aquellos con perfiles educativos atípicos o muy aislados que no pertenecen a ningún vecindario denso. Su presencia en los extremos izquierdo y superior del PCA confirma que son casos outliers (p. ej., Somalia, Niger).
- **Perfil del Cluster 0:** Tasas medias altas en todas las variables (Primaria ≈ 105%, Secundaria Baja ≈ 95%, Secundaria Alta ≈ 80–83%), correspondiente a países con sistemas educativos más desarrollados.
- **Perfil del Cluster 1:** Tasas sustancialmente menores (Primaria ≈ 90%, Secundaria Baja ≈ 48%, Secundaria Alta ≈ 24–31%), representando países con acceso educativo muy limitado, especialmente en los niveles secundarios.
- **Ventaja de DBSCAN:** A diferencia de K-Means, este método no fuerza la asignación de outliers a un cluster, ofreciendo una visión más honesta de la heterogeneidad del conjunto de datos.

---

### Figura 7: PCA Estructura de Varianza y Loadings

**Descripción**

Tres subgráficos: (1) gráfico de barras de varianza explicada por componente principal (PC1–PC6); (2) curva de varianza acumulada con umbrales horizontales al 80% y 90%; (3) biplot de contribución de variables originales a PC1 y PC2.

**Valor Analítico**

- **Concentración de varianza:** PC1 explica el 62.3% de la varianza total y PC2 el 29.0%, sumando el 91.3% con solo dos componentes. Esto indica una estructura de datos altamente comprimible y dominada por dos dimensiones latentes.
- **PC3 en adelante:** PC3 aporta apenas un 6.3% adicional; los componentes restantes son marginales. La varianza acumulada supera el umbral del 90% con dos componentes, validando la reducción a 2D para la visualización.
- **Interpretación de los loadings (biplot):**
  - **PC1 (eje horizontal):** Todos los vectores de variables apuntan hacia la derecha, con `Prim_H`, `Prim_M`, `SecBaja_H`, `SecBaja_M`, `SecAlta_H` y `SecAlta_M` contribuyendo positivamente. PC1 representa el **nivel de desarrollo educativo general**.
  - **PC2 (eje vertical):** `Prim_H` y `Prim_M` tienen carga positiva en PC2 (apuntan hacia arriba), mientras que las variables de secundaria tienden hacia abajo. PC2 captura la **brecha entre educación primaria y secundaria** (o el perfil de género relativo al nivel).
- **Implicación:** La reducción a 2D mediante PCA retiene el 91% de la información, lo que justifica su uso como espacio de representación para los demás algoritmos.

---

### Figura 8: PCA Posicionamiento de Países y Clusters

**Descripción**

Panel izquierdo: mapa PCA 2D de todos los países coloreados por su cluster K-Means (k=2), con etiquetas de países extremos y centroides marcados. Panel derecho: gráfico de barras horizontales con los países de mayor y menor score en PC1 (desarrollo educativo general).

**Valor Analítico**

- **Extremos positivos en PC1 (alta cobertura educativa):** Belgium, Tokelau, Australia, Australia and New Zealand, Monaco, Montserrat, Sweden y Denmark encabezan el ranking con scores superiores a 2.5. Estos países tienen altas tasas de matrícula en todos los niveles y géneros.
- **Extremos negativos en PC1 (bajo desarrollo educativo):** Somalia (score ≈ -7.5), Niger, South Sudan, Central African Republic, Equatorial Guinea, Chad y Burkina Faso presentan los valores más bajos, reflejando sistemas educativos con cobertura muy limitada.
- **Separación geográfica implícita:** Los países en el extremo negativo son mayoritariamente del África Subsahariana, mientras que los positivos corresponden a Europa Occidental y Oceanía, sugiriendo que PC1 es también un proxy del desarrollo económico regional.
- **Eje PC2 (brecha primaria-secundaria):** Países como Madagascar, Rwanda y Malawi, con posiciones altas en PC2 pero bajas en PC1, podrían tener una matrícula primaria relativamente alta pero secundaria muy baja, reflejando una pirámide educativa muy estrecha.
- **Coherencia con K-Means:** La separación visual entre Cluster 0 y Cluster 1 a lo largo de PC1 confirma que el eje principal de clustering es el desarrollo educativo global.

---

### Figura 9: t-SNE Impacto de la Perplejidad

**Descripción**

Tres proyecciones t-SNE del mismo conjunto de datos con perplexity=10, 30 y 50, todas coloreadas según la asignación K-Means (k=2). Las escalas de los ejes varían significativamente entre los tres paneles.

**Valor Analítico**

- **Perplexity=10:** Los clusters aparecen muy fragmentados y dispersos. La estructura global no es clara y los puntos de ambos clusters se mezclan parcialmente, lo que indica que con baja perplejidad t-SNE captura relaciones muy locales y pierde la estructura global.
- **Perplexity=30:** Configuración de equilibrio. El Cluster 1 (cian, países de bajo desarrollo) forma un grupo compacto y bien separado en la zona inferior-izquierda, mientras que el Cluster 0 ocupa la región superior-derecha con mayor dispersión interna. Esta es la visualización más informativa.
- **Perplexity=50:** Los clusters están aún más comprimidos pero también más solapados. La separación visual es menor que con perp=30, indicando que la perplejidad alta homogeneiza demasiado las estructuras locales.
- **Conclusión metodológica:** Perplexity=30 es el valor óptimo para este dataset, ofreciendo el mejor balance entre preservación de estructura local y global. Este parámetro fue seleccionado para las visualizaciones finales del análisis.

---

### Figura 10: t-SNE Validación Visual de Estructuras de Clustering

**Descripción**

Dos proyecciones t-SNE (perp=30) del mismo espacio de datos: la izquierda coloreada por K-Means (k=2) y la derecha coloreada por DBSCAN (eps=1.1, ms=5) con puntos de ruido marcados con ×.

**Valor Analítico**

- **Coherencia de K-Means:** En el panel izquierdo, el Cluster 1 (cian) forma una región compacta en la esquina inferior-izquierda, bien separada del Cluster 0 (azul). La frontera entre clusters es clara, validando visualmente que K-Means captura una separación real en los datos.
- **Coherencia de DBSCAN:** El panel derecho muestra que el Cluster 1 de DBSCAN (verde amarillento) se superpone casi exactamente con la zona donde K-Means asignó su Cluster 1. Los puntos de ruido (×) se concentran en la zona de transición y en los bordes del espacio t-SNE.
- **Concordancia entre métodos:** Ambos algoritmos identifican esencialmente la misma estructura bipartita, lo que aumenta la robustez de la conclusión: existe una división global real entre países con alta y baja cobertura educativa.
- **Ventaja de la comparación:** Los puntos de ruido de DBSCAN que aparecen en la zona de transición de t-SNE corresponden a países que K-Means fuerza a asignar a un cluster, siendo DBSCAN más conservador al reconocerlos como casos ambiguos. Esto enriquece el análisis al identificar países en situación educativa de frontera.

---

### Figura 11: Comparativa de los 4 Métodos No Supervisados

**Descripción**

Panel 2×2 que consolida los cuatro métodos en un único espacio visual: K-Means (k=2) en PCA 2D (superior izquierda), DBSCAN (eps=1.1, ms=5) en PCA 2D (superior derecha), PCA Biplot con 91% de varianza explicada (inferior izquierda) y t-SNE (perp=30) coloreado por K-Means (inferior derecha).

**Valor Analítico**

- **K-Means vs. DBSCAN (espacio PCA):** Ambos paneles comparten el mismo sistema de ejes PCA. K-Means produce una partición exhaustiva y limpia, mientras que DBSCAN deja sin clasificar una fracción de puntos (ruido). La separación entre Cluster 0 y Cluster 1 es consistente entre métodos.
- **PCA Biplot:** Los vectores de loading confirman que todas las variables educativas contribuyen positivamente a PC1, y que las variables de primaria tienen mayor proyección en PC2 que las de secundaria. Esto es coherente con la interpretación de que PC1 = desarrollo educativo global y PC2 = brecha entre niveles.
- **t-SNE como validación:** La proyección t-SNE (perp=30) en el cuadrante inferior derecho confirma la separación binaria de K-Means en un espacio no lineal, aumentando la confianza en que la estructura identificada no es un artefacto del espacio lineal PCA.
- **Síntesis metodológica:** Los cuatro métodos convergen en la misma conclusión: el conjunto de datos tiene una estructura de dos grandes grupos educativos globales. K-Means y DBSCAN los identifican directamente; PCA explica sus dimensiones subyacentes; y t-SNE los valida en un espacio no lineal. Esta triangulación metodológica fortalece la validez interna del análisis.

---

*Reporte de imagenes generado a partir del análisis no supervisado de datos educativos UNESCO. Realizado por el grupo5 en la materia de ML*
