# proyecto_final

Este es un borrador

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