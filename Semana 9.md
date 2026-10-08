## Conceptos principales

En esta clase avanzamos desde la descripción de los datos hacia el uso de modelos lineales para estudiar varias variables al mismo tiempo.

Vimos que el **R²** indica qué proporción de la variación observada consigue explicar el modelo. Un R² mayor implica un mejor ajuste descriptivo, aunque no demuestra causalidad.

También incorporamos nuevas variables al modelo, como la franja horaria y el PM2.5, para analizar si aportaban información adicional.

Trabajamos con **variables categóricas**, donde una categoría funciona como referencia, y con **variables binarias**, que toman valores 0 y 1.

Otro concepto nuevo fueron las **interacciones**, que permiten representar situaciones donde el efecto de una variable cambia según otra. En este caso, la relación entre tránsito y NO2 no era igual en todas las franjas horarias.

Finalmente analizamos los **residuos**, es decir, la diferencia entre el valor observado y el valor predicho por el modelo. Esto permite revisar qué parte de los datos todavía no está siendo explicada.


### Actividad práctica

#### Ejercicio 1 - Valores faltantes

Al revisar los valores faltantes después de crear las nuevas variables vimos que `franja`, `tipo_dia` y `transito_rel` no generan nuevos faltantes.

En cambio, `log_pm25` conserva los faltantes de `pm25`, porque si no existe una medición de PM2.5 tampoco es posible calcular su logaritmo.

Los resultados fueron:

| Estación | Faltantes NO2 | Faltantes PM2.5 | Faltantes log(PM2.5) |
|---|---:|---:|---:|
| Curva de Maronias | 65 | 0 | 0 |
| Museo Romántico | 720 | 144 | 144 |
| Tres Cruces | 429 | 326 | 326 |

Esto muestra que transformar una variable no elimina automáticamente los problemas de datos faltantes.

#### Ejercicio 2 - ¿Está bien definida la hora pico?

La definición inicial consideraba como hora pico las 7, 8, 9, 17, 18 y 19 horas.

Al observar el tránsito promedio de Tres Cruces durante los días de semana vimos que esta clasificación no coincide del todo con las horas de mayor tránsito.

El gráfico muestra un bloque de tránsito alto principalmente entre las **8 y las 17 horas**. Por ejemplo, las 7, 18 y 19 horas tienen menos tránsito que varias horas que originalmente habían quedado clasificadas como “resto”.

Por eso una clasificación más representativa de los datos sería considerar como período de tránsito alto aproximadamente las horas **8 a 17**.

La idea del ejercicio fue comprobar que una categoría definida previamente debe contrastarse con los datos antes de utilizarla en el análisis.

#### Ejercicio 3 - PM2.5 por franja horaria

Al comparar el PM2.5 promedio según la franja horaria vimos que el valor máximo aparece durante la **noche** en las tres estaciones.

En Tres Cruces, por ejemplo:

- tarde: aproximadamente **17,2 µg/m³**;
- noche: aproximadamente **31,9 µg/m³**.

Esto no coincide con el patrón del tránsito, que alcanza su mayor nivel durante la **tarde**.

Por lo tanto, el PM2.5 no sigue de forma inmediata el mismo patrón horario que el tránsito.

#### Ejercicio 4 - Agregar una variable al modelo

Partimos del modelo que explicaba el NO2 utilizando tránsito y PM2.5.

Su R² era:

**R² = 0,473**

Luego agregamos la variable `franja`.

El nuevo modelo obtuvo:

**R² = 0,486**

Por lo tanto, el R² aumentó aproximadamente **0,013**, es decir, alrededor de **1,3 puntos porcentuales**.

La mejora existe, pero es relativamente pequeña.

Los efectos estimados de las franjas, tomando madrugada como referencia, fueron aproximadamente:

- mañana: +2,80 µg/m³;
- tarde: +5,41 µg/m³;
- noche: +6,21 µg/m³.

La noche fue la franja que mostró la diferencia más clara respecto de la madrugada.

#### Ejercicio 5 - Variable binaria

A partir del resultado anterior se creó una variable binaria que vale:

**1 = noche**  
**0 = cualquier otra franja**

Al estimar nuevamente el modelo con tránsito, PM2.5 y esta variable se obtuvo:

**R² = 0,481**

El coeficiente de `noche` fue aproximadamente:

**+3,82 µg/m³ de NO2**

Esto significa que, manteniendo constantes el tránsito y el PM2.5, las observaciones nocturnas presentan en promedio alrededor de **3,8 µg/m³ más de NO2** que las observaciones del resto del día.

#### Ejercicio 6 - Interacciones

Al analizar por separado la relación entre tránsito y NO2 según la franja horaria vimos que la **madrugada** presenta la pendiente más pronunciada.

Por eso se creó una variable binaria que identifica la madrugada y se incorporó una interacción con el tránsito.

El modelo obtuvo:

**R² = 0,398**

Para las horas que no pertenecen a la madrugada, la pendiente del tránsito fue aproximadamente:

**0,58 µg/m³ de NO2 por cada mil vehículos adicionales.**

La interacción entre tránsito y madrugada fue aproximadamente:

**+2,36**

Por lo tanto, durante la madrugada la pendiente total es aproximadamente:

**0,58 + 2,36 = 2,94 µg/m³**

Esto significa que la relación entre tránsito y NO2 es bastante más intensa durante la madrugada que durante el resto del día.

La idea principal de una interacción es justamente que **el efecto de una variable puede depender del valor de otra**.

#### Ejercicio 7 - ¿Qué pasa en la tarde?

Al observar solamente las horas de la tarde aparecían dos grupos de observaciones.

Al diferenciarlos según `tipo_dia` vimos que corresponden principalmente a:

**lunes a viernes** y **fin de semana**.

Durante la tarde, en Tres Cruces:

- de lunes a viernes el tránsito promedio fue aproximadamente **42.181 vehículos por hora** y el NO2 promedio fue **50,0 µg/m³**;
- durante el fin de semana el tránsito promedio fue aproximadamente **28.903 vehículos por hora** y el NO2 promedio fue **41,8 µg/m³**.

Por lo tanto, los dos grupos que aparecían visualmente estaban asociados al tipo de día.

Esto muestra cómo una separación que inicialmente aparece en un gráfico puede entenderse mejor al incorporar una tercera variable.
