# Semana 5

## Flujo de análisis de datos

Vimos el flujo general que sigue un proyecto de ciencia de datos, desde que se obtienen los datos hasta que se comunican los resultados.

![Flujo de análisis de datos](imagenes/flujo_datos.png)

Las principales etapas son:

- **Ingesta / Carga:** incorporación de los datos crudos desde distintas fuentes, intentando conservarlos sin modificaciones.
- **Data Profiling:** primera inspección para conocer la estructura, calidad y posibles problemas del dataset.
- **Data Wrangling:** limpieza y preparación de los datos, incluyendo tratamiento de faltantes, tipos incorrectos y duplicados.
- **EDA (Exploratory Data Analysis):** exploración de los datos mediante estadísticas descriptivas y relaciones entre variables.
- **Transformación y modelado:** creación de nuevas variables, agregaciones y preparación de los datos para el análisis.
- **Entrega de resultados:** comunicación de los resultados mediante SQL, dashboards, reportes, planillas, APIs u otros medios.

Una idea importante es que estas etapas no siempre son completamente lineales: durante el análisis puede ser necesario volver a transformar o revisar los datos.

## Data Profiling

Retomamos el concepto de *profiling* visto la semana anterior.

Antes de analizar un conjunto de datos es importante revisar:

- cantidad de filas y columnas;
- tipos de variables;
- valores únicos;
- valores faltantes;
- posibles falsos nulos;
- registros duplicados;
- formatos inconsistentes.

El objetivo es comprender la calidad y estructura de los datos antes de modificarlos.

## Tipos de carga

Vimos dos formas principales de incorporar datos:

### Full Load

Se cargan nuevamente todos los datos disponibles.

Es útil para una carga inicial o una migración completa, aunque puede consumir muchos recursos cuando el volumen de datos es grande.

### Carga incremental

Se cargan solamente los registros nuevos o modificados desde la última actualización.

Para identificar estos cambios se pueden utilizar:

- timestamps;
- secuencias numéricas;
- CDC (*Change Data Capture*).

La carga incremental es más eficiente, pero requiere controlar correctamente duplicaciones e inconsistencias.

## Distintas fuentes y formatos de datos

Los datos no siempre provienen de un CSV. Vimos diferentes posibilidades:

- CSV, TSV y TXT;
- Excel;
- JSON y XML;
- Parquet, Avro y ORC;
- APIs REST;
- Web Scraping;
- bases de datos.

Cada formato tiene características distintas y puede ser más adecuado dependiendo del volumen, estructura y forma en que se generan los datos.

## PostgreSQL dentro del flujo de datos

Vimos cómo una base de datos como PostgreSQL puede funcionar como parte central del flujo:

```text
Fuentes → Ingesta → Perfilado → PostgreSQL → Ciencia de Datos → Entrega
```

PostgreSQL permite almacenar los datos de forma estructurada y luego conectarlos con herramientas de análisis, Python o plataformas de Business Intelligence.

También permite trabajar con:

- claves primarias y foráneas;
- restricciones;
- tipos de datos definidos;
- Views y Materialized Views.

## Excel vs. base de datos

También discutimos por qué Excel no cumple la misma función que una base de datos relacional.

**Excel** está pensado principalmente como hoja de cálculo para edición, cálculos y visualización de datos.

**PostgreSQL**, en cambio, es un sistema de gestión de bases de datos que permite controlar relaciones, tipos de datos, integridad y acceso simultáneo de múltiples usuarios.

## Seudocódigo

Vimos el seudocódigo como una forma de pensar primero la lógica de un procedimiento antes de escribirlo en un lenguaje como Python o R.

Permite definir:

- qué pasos deben ejecutarse;
- en qué orden;
- qué condiciones deben verificarse;
- cómo actuar frente a posibles errores.

Por ejemplo, antes de programar la lectura de un CSV se puede plantear:

```text
INTENTAR cargar archivo con UTF-8

SI FALLA
    intentar cargar con latin-1
```

Luego esa lógica puede traducirse a código.

## Ejercicio: del seudocódigo al código

Como ejercicio se trabajó con una lista de temperaturas:

```text
[18, 22, 15, 25, 30]
```

El objetivo era plantear primero en seudocódigo cómo seleccionar solamente las temperaturas mayores a 20 y luego traducir esa lógica a R o Python.

El resultado esperado era:

```text
22, 25, 30
```
