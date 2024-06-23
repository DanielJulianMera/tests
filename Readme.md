# Análisis de Datos sobre la Población y la Calidad del Aire

## Introducción
Este análisis tiene como objetivo investigar la relación entre la población de las ciudades y la calidad del aire. Se examina si las ciudades más pobladas tienen niveles más altos de contaminación del aire.

## Proceso de Obtención de Datos Demográficos
Los datos demográficos fueron obtenidos de un conjunto de datos público de OpenDataSoft. Estos datos incluyen información sobre la población, edad media, población masculina y femenina, entre otros datos relevantes para diversas ciudades de los Estados Unidos. Se limpiaron los datos eliminando columnas irrelevantes (Race, Count, Number of Veterans) y filas duplicadas para asegurar la calidad de los datos.

## Proceso de Obtención de Datos de Calidad del Aire
Los datos de calidad del aire fueron obtenidos mediante una API proporcionada por API Ninjas. Se realizaron solicitudes para cada ciudad presente en los datos demográficos, obteniendo información sobre varios contaminantes del aire como CO, PM10, SO2, PM2.5, O3, y NO2.

## Creación y Consulta de la Base de Datos
Se creó una base de datos SQLite para almacenar ambos conjuntos de datos (demografía y calidad del aire). La base de datos se estructuró de manera que ambas tablas pudieran ser unidas (join) sobre el nombre de la ciudad. La siguiente consulta SQL se utilizó para combinar estos datos y calcular la concentración total de contaminantes en las ciudades más pobladas:

```sql
SELECT d.City, d.[Total Population] as Population, ca.CO, ca.PM10, ca.SO2, ca.PM2_5, ca.O3, ca.NO2,
       (ca.CO + ca.PM10 + ca.SO2 + ca.PM2_5 + ca.O3 + ca.NO2) AS total_concentration
FROM demografia d
JOIN calidad_aire ca ON d.City = ca.City
ORDER BY d.[Total Population] DESC
LIMIT 11
```

## Resultados

| City          | Population | CO    | PM10  | SO2  | PM2_5 | O3    | NO2   | total_concentration |
|---------------|------------|-------|-------|------|-------|-------|-------|----------------------|
| New York      | 8550405    | 433.92| 26.76 | 12.04| 13.78 | 39.70 | 73.34 | 599.54               |
| Los Angeles   | 3971896    | 267.03| 26.52 | 7.39 | 18.57 | 178.81| 16.11 | 514.43               |
| Chicago       | 2720556    | 393.87| 23.00 | 3.99 | 19.74 | 95.84 | 39.76 | 576.20               |
| Houston       | 2298628    | 240.33| 4.25  | 5.48 | 2.66  | 45.42 | 15.25 | 313.39               |
| Philadelphia  | 1567442    | 220.30| 15.31 | 7.27 | 6.22  | 88.69 | 15.77 | 353.56               |
| Phoenix       | 1563001    | 220.30| 11.39 | 1.04 | 3.91  | 75.82 | 4.67  | 317.13               |
| San Antonio   | 1469824    | 216.96| 3.78  | 1.37 | 3.40  | 77.25 | 6.26  | 309.02               |
| San Diego     | 1394907    | 193.60| 16.99 | 1.70 | 8.17  | 80.82 | 6.77  | 308.05               |
| Dallas        | 1300082    | 260.35| 3.92  | 1.36 | 2.83  | 76.53 | 12.00 | 356.99               |
| San Jose      | 1026919    | 240.33| 14.99 | 3.04 | 9.99  | 103.00| 13.71 | 385.06               |
| Austin        | 931840     | 201.94| 2.54  | 0.63 | 1.99  | 69.38 | 5.27  | 281.75               |

## Conclusiones

1. Nueva York y Chicago tienen las mayores concentraciones totales de contaminantes entre las ciudades más pobladas, sugiriendo una correlación entre alta población y alta contaminación del aire en estas ciudades.

2. Los Ángeles también muestra una alta concentración total de contaminantes, lo que refuerza la tendencia de que las ciudades más grandes tienden a tener mayores problemas de calidad del aire.

3. Houston y Filadelfia tienen poblaciones relativamente grandes, pero presentan concentraciones totales de contaminantes más bajas en comparación con Nueva York y Chicago, indicando que no todas las ciudades con grandes poblaciones tienen necesariamente la peor calidad del aire.

4. San Antonio, San Diego, Phoenix, Dallas, y San Jose tienen concentraciones totales de contaminantes que varían, pero en general son menores en comparación con las ciudades más grandes como Nueva York y Los Ángeles.

En resumen, aunque hay una tendencia general a que las ciudades más pobladas tengan mayores concentraciones de contaminantes del aire, no es una regla estricta. Otros factores además de la población juegan un papel significativo en la calidad del aire de una ciudad.
