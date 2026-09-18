# Clase 8 - Visualización de datos

## Partes de un gráfico

Trabajamos sobre cómo construir un gráfico de forma que no solo muestre datos, sino que también sea fácil de interpretar.

En `matplotlib` se distinguen dos objetos principales:

- **fig:** representa la figura completa o lienzo.
- **ax:** representa el sistema de ejes donde se dibujan los datos, escalas y marcas.

Una misma figura puede contener varios sistemas de ejes.

## Elementos principales de una visualización

Vimos que un gráfico está compuesto por distintas partes:

- **Título:** debe indicar qué se está midiendo, sobre quién o qué y en qué período.
- **Ejes:** deben indicar claramente la variable y su unidad.
- **Marcas:** definen los valores que aparecen sobre los ejes.
- **Datos:** son la información que realmente aporta el gráfico.
- **Geometría:** determina cómo se representan los datos, por ejemplo mediante puntos o líneas.
- **Atributos estéticos:** color, tamaño o grosor.
- **Grilla:** sirve como ayuda para leer valores.
- **Bordes:** se pueden eliminar si no aportan información.
- **Fuente y notas:** permiten aclarar de dónde vienen los datos o posibles limitaciones.

## Línea y puntos

Una idea importante fue que distintas geometrías transmiten información diferente.

- Una **línea** ayuda a observar una tendencia y genera una idea de continuidad.
- Los **puntos** muestran dónde existen observaciones reales.

Por eso se pueden combinar ambas: la línea facilita ver la tendencia mientras que los puntos recuerdan dónde se encuentran los datos observados.

## Escalas y etiquetas

También vimos que una mala elección de escalas o etiquetas puede dificultar o incluso modificar la interpretación del gráfico.

Por ejemplo, si el mes se guarda como un número, el programa puede mostrar valores intermedios como `1.5`, `2.5` o `3.5`, aunque esos meses no existan.

En ese caso conviene definir manualmente:

```text
1 → Ene
2 → Feb
3 → Mar
4 → Abr
```

También es importante que el eje Y incluya la **variable y su unidad**, en lugar de utilizar una etiqueta genérica como "Valores".

## Diseño y legibilidad

Los colores, tamaños y grosores no siempre representan una variable. A veces se utilizan solamente para mejorar la lectura.

La idea general es evitar elementos que compitan visualmente con los datos.

Por ejemplo:

- usar una grilla suave;
- eliminar bordes innecesarios;
- evitar colores o elementos decorativos que no agreguen información;
- utilizar leyendas solamente cuando hay más de una serie.

El criterio trabajado fue mantener únicamente los elementos que ayudan a interpretar el gráfico.

## Guardado de una figura

La figura puede guardarse como un archivo para utilizarla posteriormente en un informe o presentación.

```python
fig.savefig("grafico.png", dpi=300, bbox_inches="tight")
```

`dpi=300` permite guardar una imagen con buena resolución y `bbox_inches="tight"` elimina márgenes sobrantes.

## Del gráfico inicial al gráfico corregido

Trabajamos sobre un gráfico de concentración media mensual de ozono y vimos cómo mejorarlo sin cambiar los datos.

Se corrigieron:

- el título;
- los nombres y unidades de los ejes;
- las etiquetas de los meses;
- la escala del eje vertical;
- la fuente de los datos.

La idea principal es que **los mismos datos pueden comunicarse mucho mejor si la visualización está correctamente diseñada**.

## Integración de datasets

También se continuó trabajando con los datos ambientales de ozono y PM2.5.

Para poder unir dos datasets es necesario identificar correctamente las variables que funcionan como clave.

En este caso se trabajó con:

```text
estación + fecha
```

Antes de unir las tablas puede ser necesario llevar ambas al mismo nivel de agregación, por ejemplo a datos horarios.

Esto permite que la clave sea única en ambos conjuntos de datos y evita generar combinaciones incorrectas al hacer el merge.

## Correlación y series temporales

A partir de los datos unidos se comenzó a explorar la relación entre ozono y PM2.5.

También vimos que en una **serie temporal** el orden de las observaciones importa porque cada dato está asociado a un momento específico.

Conceptos trabajados:

- serie temporal con intervalos regulares;
- rezagos (`lags`);
- autocorrelación;
- comparación de variables en distintos momentos del día.

Esto permite analizar no solo cuánto vale una variable, sino también cómo evoluciona en el tiempo y cuánto se relaciona con sus propios valores pasados.

## Clase 2 - Unión de fuentes y gráficos por grupo

En esta clase continuamos con visualización, pero trabajando con más de una serie y con datos provenientes de distintas fuentes.

### Más de una serie

Cuando hay varias series ya no alcanza con pensar solamente en la legibilidad del gráfico. También hay que decidir **cómo representar cada grupo**.

Por ejemplo:

- usar un color diferente por grupo;
- mostrar las series en el mismo gráfico;
- separarlas en distintos paneles;
- decidir si pueden compartir la misma escala.

Una idea importante es que dos variables pueden colocarse en el mismo eje cuando tienen unidades y rangos comparables. Si las escalas son muy diferentes, conviene utilizar paneles separados.

### Unión de distintas fuentes de datos

Trabajamos con datos de **ozono (O3)** y **PM2.5**.

Para unir dos tablas es necesario identificar primero cuál es la clave que permite relacionarlas.

En este caso:

```text
estación + fecha
```

Con `inner_join` se conservan solamente las observaciones que existen en ambas tablas.

Esto implica que al hacer una unión también es importante preguntarse:

- qué observaciones permanecen;
- cuáles quedan afuera;
- por qué no pudieron unirse.

Antes de unir las tablas también puede ser necesario llevarlas al mismo nivel de agregación. En el ejemplo se trabajó con valores horarios para obtener una fila por estación y hora.

### Variables representadas mediante color

Vimos la diferencia entre utilizar el color solamente como una decisión estética y utilizarlo para representar información.

Por ejemplo, en `ggplot`:

```text
color = "blue"        → color fijo, no representa una variable
aes(color = estacion) → el color representa la estación
```

Cuando el color representa una variable, también se genera una leyenda que permite identificar los grupos.

### Paneles y `facet_wrap`

Otra forma de comparar grupos es dividir una visualización en varios paneles.

`facet_wrap()` permite repetir el mismo tipo de gráfico para diferentes categorías.

Esto resulta útil porque se puede hacer **la misma pregunta a distintos grupos** sin superponer toda la información en un solo gráfico.

Cuando los paneles se quieren comparar directamente, conviene mantener las mismas escalas. Las escalas independientes se utilizan cuando los rangos de las variables son muy diferentes.

### Distintas geometrías responden distintas preguntas

Vimos que el tipo de gráfico depende de la pregunta que se quiere responder:

| Gráfico | Pregunta |
|---|---|
| Línea | ¿Cómo cambia una variable a lo largo de un eje ordenado? |
| Dispersión | ¿Cómo se relacionan dos variables? |
| Boxplot | ¿Cómo se distribuye una variable entre distintos grupos? |

El boxplot resume la distribución mediante mediana, cuartiles y valores extremos. Pierde detalle individual, pero facilita la comparación entre grupos.

### Gráfico de dispersión

Para estudiar la relación entre ozono y PM2.5 se utilizaron gráficos de dispersión.

Cada punto representa una observación conjunta de ambas variables.

También se utilizó la correlación `r` como medida de asociación.

Una idea importante es que:

```text
correlación ≠ causalidad
```

Una relación entre dos variables puede estar influida por otras variables.

### Color continuo y color discreto

La hora del día se utilizó de dos formas distintas.

Como variable continua:

```text
0, 1, 2, ..., 23 horas
```

o agrupándola en categorías:

```text
madrugada
mañana
tarde
noche
```

Agrupar una variable continua facilita la interpretación, pero los puntos de corte son una **decisión del análisis**. Cambiar esos límites puede cambiar la forma en que se interpreta el gráfico.

### Escala logarítmica

También vimos que una escala logarítmica puede ser útil cuando una variable tiene muchos valores pequeños y pocos valores muy grandes.

La escala logarítmica permite separar mejor los valores pequeños y comprimir los valores grandes.

No debe utilizarse automáticamente, sino solamente cuando la distribución de la variable lo justifica.

### Una misma información puede contar distintas historias

Un concepto importante de la clase fue que los datos pueden ser exactamente los mismos, pero distintas visualizaciones permiten responder preguntas diferentes.

Por ejemplo:

```text
Dispersión general
        ↓
Color según hora del día
        ↓
Paneles separados por franja horaria
```

Agregar información mediante color o paneles puede revelar patrones que no eran evidentes en el gráfico original.

Por eso, antes de hacer un gráfico conviene preguntarse: **¿Qué afirmación quiero que permita leer esta figura?**


### Actividad para la bitácora

#### 1. ¿Qué estaciones quedaron fuera de la unión y por qué?

La estación que quedó disponible para comparar O3 y PM2.5 fue **Curva de Maroñas**. Colón quedó fuera de la unión porque no tenía registros correspondientes en ambas fuentes para la misma combinación de estación y fecha.

Esto ocurre porque `inner_join` conserva únicamente las observaciones que tienen coincidencia en las dos tablas.

#### 2. ¿Qué cambia al utilizar `left_join` en lugar de `inner_join`?

Al utilizar `left_join` se mantienen todas las observaciones de la tabla de ozono, incluso cuando no existe una medición correspondiente de PM2.5.

Por eso quedan más filas que con `inner_join` y, en las observaciones que no encontraron coincidencia, la variable de PM2.5 aparece como faltante (`NA`).

La diferencia principal es que `inner_join` conserva solamente las coincidencias, mientras que `left_join` permite conservar toda la información de la tabla principal.

#### 3. ¿Qué cambia al modificar los cortes de las franjas horarias?

Al cambiar los límites de las franjas horarias, algunas observaciones pasan de un grupo a otro. Esto modifica la cantidad de datos que hay en cada panel y también puede cambiar la correlación entre O3 y PM2.5 calculada dentro de cada franja.

Esto muestra que la forma de agrupar una variable continua, como la hora, es una decisión del análisis y puede influir en la interpretación de los resultados.

#### 4. Identificar dos tablas del proyecto que se puedan unir y definir la clave.

En nuestro proyecto se podría unir la tabla con el **precio local del fertilizante** con la tabla del **tipo de cambio**.

La clave de unión sería la **fecha o mes**, ya que las variables se están trabajando con frecuencia mensual.

De la misma manera se podrían incorporar posteriormente el precio internacional del fertilizante y el precio del gas, siempre asegurándonos de que todas las variables estén expresadas con la misma frecuencia temporal y tengan una observación única por período.
