# proyecto_final

Este es un borrador. Los datos se han extraído de: https://www.kaggle.com/datasets/likithagedipudi/linkedin-compatibility-dataset-50k-profiles

Análisis de Conectividad y Compatibilidad Profesional
📌 Descripción del Proyecto
Este proyecto consiste en un Análisis Exploratorio de Datos (EDA) sobre un dataset de red profesional que contiene perfiles de usuarios y métricas de compatibilidad entre ellos. El objetivo final es la creación de un Dashboard interactivo en Power BI para identificar patrones de networking, brechas de habilidades y oportunidades de mentoría.
📊 Especificaciones Técnicas
El dataset final cumple con los requisitos mínimos establecidos para el proyecto:
    • Registros totales: 200,000 filas.
    • Atributos (Columnas): 33 columnas tras la limpieza y unificación.

🛠️ Metodología de Procesamiento
1. Integración de Datos (Data Merging)
Se utilizaron dos fuentes de datos principales:
    • profiles.csv: Información biográfica y profesional de 50,000 usuarios únicos.
    • muestra_eda.csv: Registro de 200,000 interacciones o "matches" entre usuarios.
Se realizó un Inner Join utilizando la llave profile_a_id (de la muestra) y profile_id (de los perfiles) para enriquecer cada interacción con los datos del usuario correspondiente.
2. Limpieza Exhaustiva
Se aplicó un pipeline de limpieza en Python para asegurar la calidad en Power BI:
    • Normalización: Estandarización de textos en columnas como name e industry.
    • Tratamiento de Listas: Limpieza de caracteres especiales en columnas de skills, goals y needs.
    • Imputación de Nulos: Los valores faltantes en experiencia se trataron con la mediana, y las categorías vacías se marcaron como "Not Specified".
    • Optimización de Tipos: Ajuste de formatos numéricos para mejorar el rendimiento del reporte.

⚠️ Nota sobre la Integridad de los Datos: Duplicados de Nombre
Durante el análisis, se identificó una alta frecuencia de nombres repetidos (ej. "David Smith" aparece 121 veces). Es fundamental aclarar que estos no son errores de duplicidad, sino que responden a dos fenómenos válidos:
    1. Naturaleza Relacional: El dataset registra emparejamientos. Una persona única aparece múltiples veces porque se está evaluando su compatibilidad con diferentes perfiles. Cada fila representa una relación distinta, no un usuario repetido.
    2. Homónimos Reales: En una base de datos de 50,000 personas, existen individuos distintos con el mismo nombre pero diferentes IDs únicos (profile_id).
Recomendación de Uso: Para obtener métricas de personas únicas en el Dashboard, se debe utilizar la función DISTINCTCOUNT sobre la columna profile_id en lugar de contar los nombres directamente.

A. Distribución del Talento
El análisis muestra cómo se distribuyen los 50,000 perfiles. Si la mediana de "Años de Experiencia" es menor a la media, tienes una base joven con algunos perfiles expertos muy destacados. Esto justifica la creación de la columna Rango de Experiencia para segmentar el Dashboard.

B. Análisis de los Scores de Red
Alineación de Carrera: Este valor indica qué tan parecidos son los caminos profesionales de los usuarios comparados.

Complementariedad de Habilidades: Un puntaje alto aquí sugiere que el dataset es ideal para escenarios de Mentoría, donde uno tiene lo que el otro necesita.

C. Matriz de Correlación
Este es el hallazgo más técnico. En el README, indica cuál de los scores (Habilidades, Geográfico o Carrera) tiene el coeficiente más cercano a 1.0. Esa es la variable que "mueve la aguja" en la compatibilidad de tu modelo.

3. Resumen de Calidad de Datos
Para cerrar tu proceso, es importante mencionar en el README que:

Integridad: El dataset final cuenta con 0 nulos en las métricas clave.

Normalización: Los nombres han sido traducidos y estandarizados para facilitar el consumo por usuarios no técnicos en Power BI.

Histograma con KDE (Densidad):

Objetivo: Detectar si los perfiles están concentrados en una puntuación "media" o si hay una polarización (muchos perfiles con 0 y muchos con 100).

Rigor: Si la media y la mediana están muy separadas, el dataset tiene un sesgo que debemos reportar.

Scatter Plot con Línea de Regresión:

Objetivo: Validar la hipótesis: "A mayor experiencia, mayor red de contactos".

Rigor: La pendiente de la línea roja nos dirá qué tan fuerte es este crecimiento profesional en la muestra.

Boxplot (Diagrama de Caja y Bigotes):

Objetivo: Identificar el rango intercuartílico.

Rigor: Nos permite ver los outliers (puntos aislados). Si un Junior tiene compatibilidad de Senior, es un perfil excepcional que Power BI debe destacar.

Heatmap (Mapa de Calor):

Objetivo: Evitar la multicolinealidad.

Rigor: Si dos variables tienen una correlación de 0.99, son redundantes. Esto nos ayuda a simplificar el modelo para el Dashboard.

1. Análisis de Densidad y Tendencia Central
Al observar la Distribución de la Compatibilidad, vemos una curva con un ligero sesgo a la derecha:

Media (36.66) vs. Mediana (35.80): La cercanía de ambos valores indica que el dataset es bastante estable, pero la media está siendo "arrastrada" por perfiles de alta compatibilidad (puntuaciones superiores a 50).

Interpretación: El grueso de los usuarios tiene una compatibilidad de nivel medio-bajo. En Power BI, esto sugiere que encontrar una "pareja profesional perfecta" (puntuación > 50) es un evento poco común, lo que añade valor a tu algoritmo de búsqueda.

2. Correlaciones Críticas (El motor del sistema)
Tu Mapa de Calor revela relaciones matemáticas fundamentales:

Factor Dominante: Los Años de Experiencia y las Conexiones tienen una correlación altísima de 0.87. Esto confirma que la red en este dataset crece de forma orgánica y robusta con la carrera profesional.

Impacto en la Compatibilidad: El Índice de Compatibilidad Total está impulsado principalmente por el Valor de Red (0.60) y la Puntuación de Coincidencia de Habilidades (0.56).

Insight Riguroso: La ubicación geográfica (Puntaje Geográfico) tiene una correlación de 0.21, lo que significa que en este ecosistema, lo que sabes y a quién conoces pesa casi tres veces más que dónde vives.

3. El Salto Profesional (Experiencia vs. Red)
El gráfico de Relación Lineal muestra un patrón muy interesante:

Comportamiento por tramos: Se observan "escalones" claros en el volumen de conexiones a los 2, 7, 15 y 30 años de experiencia.

Techo de Red: Existe una clara saturación de conexiones (el límite de 5000) que muchos perfiles alcanzan a partir de los 15 años de experiencia. Esto es un dato vital para segmentar perfiles "Top Connectors".

4. Variabilidad por Seniority (Boxplots)
Este gráfico es el que más "limpieza" visual aporta:

Aumento de la mediana: A medida que subimos de entry a senior, el índice de compatibilidad sube consistentemente.

El fenómeno "Executive": Los perfiles ejecutivos muestran la mayor dispersión (la caja es más grande). Esto indica que en niveles altos, las compatibilidades son extremas: o son muy altas o muy bajas, sin mucho término medio.

Outliers: Hay muchos perfiles de nivel entry y mid que tienen compatibilidades muy por encima de su promedio (puntos negros), lo que indica "talento emergente" con un potencial de networking superior a su rango actual.

5. Concentración Industrial
Tu gráfico de barras muestra un equilibrio casi perfecto entre sectores:

Industria Líder: Consulting destaca ligeramente sobre las demás.

Uniformidad: El hecho de que todas las industrias (Education, Healthcare, Finance, etc.) tengan volúmenes similares (alrededor de 14,000 perfiles) indica que el dataset es robusto y no está sesgado hacia un solo nicho, lo que da validez universal a tus conclusiones.


COnclusiones 

Tras procesar una muestra de 50,000 perfiles y analizar las interdependencias entre variables de carrera, networking y compatibilidad, se extraen las siguientes conclusiones clave:

1. El Motor de la Red: Experiencia y Capital Social
Correlación Determinante: Se identificó una correlación positiva muy fuerte (0.87) entre los Años de Experiencia y el Número de Conexiones. Esto indica que la red de contactos en este ecosistema no es azarosa, sino que se construye de forma acumulativa y orgánica conforme avanza la carrera profesional.

Hitos de Crecimiento: El volumen de red presenta "saltos" críticos en etapas de transición (2, 7 y 15 años), lo que sugiere periodos de expansión profesional acelerada.

2. Jerarquía y Eficacia del Matching
Progresión de Compatibilidad: El análisis de Boxplots confirma que el Índice de Compatibilidad Total aumenta proporcionalmente con el nivel de seniority. Los perfiles de nivel Senior no solo tienen más contactos, sino que su afinidad con el ecosistema es más alta y estable.

Dispersión en Niveles Ejecutivos: Se observó que los perfiles de nivel Executive presentan la mayor variabilidad. Esto sugiere que a niveles de alta dirección, la compatibilidad es altamente especializada: los matches son excepcionales o nulos, sin una zona media clara.

3. Factores de Peso en la Compatibilidad
Habilidades vs. Geografía: El éxito de una conexión está impulsado principalmente por el Valor de Red (0.60) y la Coincidencia de Habilidades (0.56). Por el contrario, la ubicación geográfica mostró una influencia menor (0.21).

Insight: En el mercado laboral analizado, el "qué sabes" y el "a quién conoces" son factores tres veces más potentes para generar compatibilidad que el "dónde estás ubicado".

4. Estructura Industrial Equilibrada
Consistencia de Datos: El dataset muestra una distribución equitativa entre 10 sectores principales, con una ligera predominancia de Consulting. Esta uniformidad garantiza que las conclusiones obtenidas son transversales y aplicables a múltiples industrias sin sesgos sectoriales significativos.