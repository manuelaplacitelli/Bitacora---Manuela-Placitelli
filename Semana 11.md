## Práctico - Tránsito y calidad del aire en Python

### Preparación de los datos

Primero cargué la base y creé las variables necesarias para los ejercicios.

A partir de la fecha y hora obtuve:

```text
hora del día
tipo de día
franja horaria
hora pico
```

También calculé el tránsito relativo de cada estación, el tránsito expresado en miles de vehículos y el logaritmo del PM2.5.

En Python usamos herramientas como `np.where()`, `pd.cut()` y `groupby()` para construir estas variables y trabajar por grupos.

---

### Ejercicio 1 - Valores faltantes

Primero revisé nuevamente los valores faltantes, pero incluyendo también las variables que habíamos creado.

Observé que `franja`, `tipo_dia` y `transito_rel` no agregaban nuevos faltantes.

En cambio, `log_pm25` tenía exactamente los mismos faltantes que `pm25`:

```text
Curva de Maronias: 0
Museo Romántico: 144
Tres Cruces: 326
```

Esto ocurre porque `log_pm25` se calcula directamente a partir de PM2.5. Si falta el dato original, tampoco puede calcularse su transformación.

---

### Ejercicio 2 - Revisar la definición de hora pico

Después analicé el tránsito promedio por hora en Tres Cruces durante los días de semana.

La definición inicial consideraba como hora pico:

```text
7, 8, 9, 17, 18 y 19
```

Al observar el gráfico vi que las 7, 18 y 19 no tenían tanto tránsito como varias de las horas comprendidas entre las 10 y las 16.

Por eso redefiní el período de **tránsito alto** como:

```text
8 a 17 horas
```

Con esta nueva clasificación quedaron:

```text
900 observaciones de tránsito alto
1260 observaciones en el resto
```

Este ejercicio mostró que una clasificación definida previamente puede no representar correctamente el patrón que realmente aparece en los datos.

---

### Ejercicio 3 - PM2.5 por franja horaria

Reutilicé el gráfico que mostraba el tránsito relativo por franja, pero lo adapté para representar el **PM2.5 promedio**.

Al comparar los resultados vi nuevamente que los máximos no coinciden:

```text
Tránsito máximo → tarde
PM2.5 máximo   → noche
```

En Tres Cruces obtuve aproximadamente:

```text
PM2.5 tarde = 17,2 µg/m³
PM2.5 noche = 31,9 µg/m³
```

Por lo tanto, el PM2.5 de una franja no sigue directamente el tránsito de esa misma franja.

También tuve que modificar elementos del gráfico, como sacar la línea horizontal que marcaba tránsito relativo igual a 1, porque esa referencia ya no tenía sentido para PM2.5.

---

### Ejercicio 4 - Agregar la franja al modelo

Partí del modelo que explicaba el NO2 utilizando tránsito y PM2.5:

```text
NO2 ~ tránsito + PM2.5
```

Este modelo tenía un:

```text
R² ≈ 0,47
```

Después agregué la franja horaria:

```text
NO2 ~ tránsito + PM2.5 + franja
```

El nuevo modelo obtuvo aproximadamente:

```text
R² ≈ 0,49
```

El ajuste mejora, aunque relativamente poco teniendo en cuenta que agregamos tres coeficientes adicionales.

En Python, la madrugada quedó como categoría de referencia.

El coeficiente de la noche fue aproximadamente:

```text
+6,2 µg/m³
```

Esto significa que, manteniendo constantes el tránsito y el PM2.5, una hora de la noche tiene en promedio unos **6,2 µg/m³ más de NO2 que una hora de la madrugada**.

---

### Ejercicio 5 - Crear una variable binaria

Al revisar los coeficientes del modelo anterior observé que la **noche** era la franja que mostraba la diferencia más clara.

Por eso creé una nueva variable:

```text
noche = 1 → la observación es de noche
noche = 0 → corresponde a otra franja
```

Después estimé un modelo utilizando tránsito, PM2.5 y esta variable binaria.

Obtuve aproximadamente:

```text
R² = 0,481
```

y el coeficiente de la noche fue cercano a:

```text
+3,82 µg/m³ de NO2
```

Esto permitió resumir el efecto que nos interesaba con una variable más simple, en lugar de mantener las cuatro categorías de franja.

---

### Ejercicio 6 - Interacciones

Después graficamos por separado la relación entre tránsito y NO2 para cada franja horaria.

Las rectas no eran paralelas, lo que indicaba que la relación entre tránsito y NO2 podía cambiar según la franja.

La **madrugada** era la franja con la pendiente más pronunciada.

Por eso creé una variable binaria para identificar la madrugada e incorporé una interacción entre esta variable y el tránsito.

El modelo obtuvo aproximadamente:

```text
R² = 0,398
```

Para el resto del día, la pendiente del tránsito fue aproximadamente:

```text
0,58 µg/m³ de NO2 por cada mil vehículos adicionales
```

La interacción con madrugada agregó aproximadamente:

```text
+2,36
```

Por lo tanto, durante la madrugada la pendiente total fue cercana a:

```text
0,58 + 2,36 = 2,94
```

Esto muestra que el efecto asociado al tránsito no necesariamente es constante durante todo el día.

En Python, la interacción se incorpora en la fórmula utilizando `*` entre las variables.

---

### Ejercicio 7 - ¿Qué pasa durante la tarde?

En el gráfico del ejercicio anterior observé que los puntos correspondientes a la tarde parecían formar **dos grupos diferentes**.

Para investigar qué podía estar generando esa separación filtré únicamente las observaciones de la tarde y las diferencié según `tipo_dia`.

Al separar entre:

```text
lunes a viernes
fin de semana
```

los dos grupos quedaron mucho más claros.

En Tres Cruces, durante la tarde obtuve aproximadamente:

```text
Lunes a viernes:
tránsito promedio = 42.181 vehículos/h
NO2 promedio = 50,0 µg/m³

Fin de semana:
tránsito promedio = 28.903 vehículos/h
NO2 promedio = 41,8 µg/m³
```

Por lo tanto, la separación que inicialmente aparecía en el gráfico estaba asociada principalmente al **tipo de día**.

Este ejercicio mostró que cuando aparece un patrón que no podemos explicar con dos variables, puede ser útil buscar una tercera variable que diferencie las observaciones.

 el resultado del modelo, sino utilizar los gráficos y las nuevas variables para entender qué patrones de los datos están detrás de ese resultado.
