# Semana 7

## Tránsito y calidad del aire

Esta semana trabajamos con una investigación paso a paso sobre la relación entre el tránsito vehicular y la calidad del aire en Montevideo, utilizando datos de tránsito, NO2 y PM2.5.

Una idea central de la clase fue que cada gráfico permite responder una pregunta, pero también deja nuevas preguntas abiertas. El análisis se fue construyendo siguiendo la lógica:

```text
Pregunta → Visualización → Qué observamos → Qué todavía no sabemos → Siguiente análisis
```

## Relacionar datos en el espacio y en el tiempo

Para poder comparar tránsito y contaminación primero fue necesario hacer que los datos fueran comparables.

Los datos ambientales se llevaron a frecuencia horaria y se relacionaron con los puntos de conteo vehicular cercanos a cada estación de calidad del aire.

Se consideraron puntos de tránsito ubicados a menos de **1,5 km** de cada estación.

Esto mostró que antes de analizar una relación entre variables hay que revisar:

- dónde fueron medidas;
- en qué momento;
- con qué frecuencia;
- qué cobertura tiene cada fuente.

No alcanza con tener dos datasets sobre el mismo tema: deben representar observaciones comparables. :contentReference[oaicite:0]{index=0}

## Elegir dónde analizar

Se compararon las estaciones según la cantidad de puntos de tránsito cercanos y la disponibilidad de mediciones de NO2 y PM2.5.

La estación elegida para comenzar fue la que tenía mayor cantidad de tránsito medido en su entorno y disponibilidad de NO2.

Esto introduce una idea importante: **la calidad y cobertura de los datos condicionan qué preguntas podemos responder**.

Si en una zona hay pocos sensores o pocos puntos de conteo, no encontrar una relación no significa necesariamente que esa relación no exista.

## Mirar primero las series

Antes de calcular correlaciones se observaron las variables a lo largo del tiempo.

En una semana se veía que:

- el tránsito aumenta durante el día y disminuye durante la noche;
- el NO2 también presenta un ritmo diario;
- el PM2.5 muestra un comportamiento diferente y con picos menos parecidos al tránsito.

Luego se construyó un “día típico” separando días de semana y fines de semana.

El NO2 presentaba picos en horarios similares a los del tránsito, especialmente durante la semana. :contentReference[oaicite:1]{index=1}

## Correlación cruda y patrones compartidos

Se calculó primero una correlación directa entre tránsito y contaminantes.

A simple vista, el NO2 mostraba una relación positiva con el tránsito, mientras que el PM2.5 no mostraba una relación inmediata tan clara.

Pero apareció un problema: dos variables pueden estar correlacionadas simplemente porque ambas siguen un **ciclo diario**.

Por ejemplo, que ambas aumenten durante determinadas horas no significa necesariamente que una esté explicando a la otra.

Por eso la correlación cruda no era suficiente. :contentReference[oaicite:2]{index=2}

## Sacar el ciclo diario

Para estudiar mejor la relación se comparó cada observación con el valor habitual para esa misma hora y tipo de día.

La idea fue analizar:

```text
valor observado - valor habitual
```

De esta manera se estudia si en una hora con **más tránsito de lo normal** también hay **más contaminación de lo normal**.

Al quitar el ciclo diario, la relación del NO2 con el tránsito disminuyó, mostrando que una parte de la correlación inicial se debía al patrón horario compartido.

Esto muestra la importancia de controlar patrones que pueden generar relaciones aparentes.

## Rezagos temporales

También vimos que un efecto no necesariamente ocurre al mismo tiempo que su posible causa.

Por eso se comparó la contaminación actual con el tránsito de horas anteriores utilizando **rezagos (lags)**.

En el ejemplo:

- el NO2 mostró su relación más fuerte con el tránsito aproximadamente **1 hora antes**;
- el PM2.5 mostró una respuesta más lenta, apareciendo varias horas después y alcanzando valores mayores alrededor de las **12–13 horas**.

Esto permitió ver que dos contaminantes pueden responder al mismo fenómeno con dinámicas temporales diferentes. 

## ¿La relación podría aparecer por casualidad?

Después de buscar el rezago con mayor correlación apareció una nueva pregunta: si probamos muchos rezagos, alguno podría dar una correlación alta simplemente por azar.

Para evaluarlo se desplazó la serie de tránsito varios días, rompiendo la relación temporal real pero conservando la estructura de cada serie.

Luego se comparó la correlación observada con las correlaciones obtenidas al desplazar los datos.

En el caso del NO2, la relación observada quedó por encima de las obtenidas con las series desplazadas, mientras que para PM2.5 el resultado fue menos claro en ese análisis inmediato.

La idea importante fue **comparar un resultado observado contra lo que podría obtenerse por azar**, en lugar de interpretar directamente el valor máximo encontrado. 

## Del análisis exploratorio a un modelo

Finalmente se planteó un modelo sencillo:

```text
valor medido =
valor habitual de esa hora
+ efecto asociado al tránsito
+ error
```

En forma simplificada:

```text
y(t) = h(t) + a + b · x(t-k) + error
```

donde:

- `h(t)` representa lo habitual para esa hora;
- `x(t-k)` representa el tránsito de horas anteriores;
- `b` representa cuánto cambia el contaminante cuando cambia el tránsito;
- el error incluye factores que no fueron incorporados, como viento, lluvia u otras fuentes de contaminación.

También vimos el uso de **R²** para evaluar cuánto de la variación observada consigue reproducir el modelo.

Agregar información del tránsito mejoró la explicación respecto de utilizar solamente el patrón horario habitual.

## Importancia de la calidad de medición

El análisis se repitió en otras estaciones que contaban con mediciones de NO2.

La relación se distinguía mejor donde había más puntos de medición de tránsito cercanos.

Una conclusión importante fue:

**no detectar una relación no implica necesariamente que no exista; también puede significar que no tenemos suficientes datos para verla.**

Por eso, además del resultado estadístico, siempre hay que considerar cómo y dónde fueron generados los datos.

## Idea principal de la semana

Esta clase mostró cómo una investigación de datos se construye de forma progresiva.

No se pasó directamente de los datos a una conclusión. Primero se revisó la cobertura, después se visualizaron los patrones, se calculó una correlación, se controló el ciclo diario, se incorporaron rezagos, se comparó con el azar y finalmente se planteó un modelo.

La idea que me queda es que **cada resultado debe llevar a preguntarse qué explica realmente, qué puede estar confundiendo la relación y qué información todavía falta**.
