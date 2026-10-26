Práctico - Tránsito y calidad del aire

Ejercicio 1 - Faltantes

Primero revisé la cantidad de datos faltantes por estación.

El tránsito estaba completo en las tres estaciones, pero la disponibilidad de datos ambientales era diferente.

Curva de Maronias: 65 faltantes de NO2 y ninguno de PM2.5.

Museo Romántico: 720 faltantes de NO2 y 144 de PM2.5.

Tres Cruces: 429 faltantes de NO2 y 326 de PM2.5.

Esto fue importante porque Museo Romántico no puede utilizarse para los análisis que requieren NO2 y, en Tres Cruces, el análisis de NO2 se realiza con menos de la mitad de las horas del mes.

Ejercicio 2 - Crear variables a partir de la hora

A partir de la fecha y hora creé nuevas variables para poder agrupar las observaciones.

Primero separé los datos entre:

lunes a viernes;

fin de semana.

También definí inicialmente como hora pico las 7, 8, 9, 17, 18 y 19 horas.

La creación de estas variables permite pasar de analizar únicamente valores individuales a comparar grupos con características comunes.

Ejercicio 3 - Franjas horarias

Después dividí las 24 horas del día en cuatro franjas:

Madrugada: 0 a 5
Mañana: 6 a 11
Tarde: 12 a 17
Noche: 18 a 23

Cada franja quedó con 540 observaciones, porque todas contienen seis horas por día durante 30 días y para tres estaciones.

También definí el orden de las categorías para que los gráficos respetaran el orden natural del día y no el orden alfabético.

Ejercicio 4 - Distribución del PM2.5

Para Curva de Maronias calculé la media y la mediana del PM2.5 y construí un histograma.

Obtuve aproximadamente:

Media = 20,1 µg/m³
Mediana = 11,9 µg/m³

La media es bastante mayor que la mediana.

Al mirar el histograma entendí por qué: la mayor parte de las horas tiene valores relativamente bajos de PM2.5, pero existen algunas observaciones muy altas que generan una cola hacia la derecha.

Esos valores extremos desplazan la media hacia arriba, mientras que la mediana es menos sensible.

Ejercicio 5 - Tránsito y PM2.5 por franja

Después analicé Tres Cruces y calculé el tránsito y el PM2.5 promedio para cada franja horaria.

Al graficar los resultados observé que el tránsito alcanza sus valores más altos durante la tarde, mientras que el PM2.5 alcanza su máximo durante la noche.

En Tres Cruces, el PM2.5 promedio fue aproximadamente:

Tarde: 17,2 µg/m³
Noche: 31,9 µg/m³

Por lo tanto, el PM2.5 no sigue exactamente el mismo patrón horario que el tránsito. Que el tránsito sea máximo en una franja no implica que el contaminante también tenga que ser máximo en ese mismo momento.

Ejercicio 6 - Tránsito relativo

Los niveles absolutos de tránsito son muy distintos entre estaciones, por lo que no era conveniente compararlos directamente.

Para solucionar esto calculé un tránsito relativo respecto al promedio de cada estación:

tránsito relativo = tránsito / tránsito medio de la estación

De esta forma:

1 = tránsito habitual de la estación
2 = doble del tránsito habitual

Al comparar días de semana y fines de semana observé que durante el fin de semana el tránsito disminuye en las tres estaciones.

El tránsito relativo durante el fin de semana fue aproximadamente:

Museo Romántico: 0,67

Tres Cruces: 0,78

Curva de Maronias: 0,90

Esto también muestra que la caída del tránsito durante el fin de semana no tiene la misma magnitud en todas las zonas.

Ejercicio 7 - Relación entre variables

Luego trabajé solamente con las observaciones que tenían información de NO2 y realicé gráficos de dispersión entre tránsito y NO2 para cada estación.

En Tres Cruces se veía una relación positiva bastante más clara: a mayor tránsito, en general también aumentaba el NO2.

En Curva de Maronias la relación era mucho más débil.

Las correlaciones entre tránsito y NO2 fueron aproximadamente:

Curva de Maronias: 0,15
Tres Cruces:       0,59

También calculé la correlación entre NO2 y PM2.5:

Curva de Maronias: 0,55
Tres Cruces:       0,40

Esto me permitió ver que una misma relación entre variables puede ser bastante distinta dependiendo de la estación que se esté analizando.

Además, una correlación positiva no implica automáticamente que una variable sea la causa de la otra.

Ejercicio 8 - Modelo lineal

Finalmente ajusté una recta para estudiar la relación entre tránsito y NO2 en Tres Cruces.

Expresé el tránsito en miles de vehículos para que los coeficientes fueran más fáciles de interpretar.

El modelo obtenido fue aproximadamente:

NO2 = 18,3 + 0,79 × tránsito en miles

La pendiente de 0,79 indica que, según la recta estimada, por cada mil vehículos por hora adicionales el NO2 aumenta aproximadamente 0,79 µg/m³.

Por ejemplo, para una hora con 30 mil vehículos:

NO2 ≈ 18,3 + 0,79 × 30
NO2 ≈ 42 µg/m³

El modelo obtuvo un:

R² ≈ 0,35

Esto significa que la recta consigue explicar aproximadamente el 35 % de la variación observada en el NO2.
