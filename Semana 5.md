# Semana 5

Trabajamos sobre dos temas principales: el diseño de encuestas como forma de generar datos y el proceso de carga, limpieza y transformación de datos.

## Diseño de encuestas

Una encuesta es una herramienta para recopilar información estadística sobre una población a partir de preguntas.

Antes de decidir hacer una encuesta es importante preguntarse:

- si hay tiempo y recursos para diseñarla y aplicarla correctamente;
- si los datos que necesitamos ya existen en otra fuente;
- si realmente necesitamos información que no podemos observar directamente, como opiniones, percepciones, intenciones o razones.

### Del concepto a la variable

Una de las ideas principales de la clase fue que una pregunta de una encuesta es, en definitiva, una forma de **medición**.

El proceso puede pensarse como:

```text
Concepto → Dimensión → Indicador → Pregunta → Variable
```

Por lo tanto, una pregunta no debería escribirse sin pensar primero qué información queremos obtener de ella.

### Diseñar la tabla antes que el formulario

La regla principal trabajada fue:

> Diseñar primero la tabla de datos y después el cuestionario.

La estructura esperada es:

- **una fila por respondente**;
- **una columna por variable**.

Cada pregunta debería permitir identificar:

1. qué columna va a generar;
2. qué tipo de variable será;
3. cómo se van a codificar las respuestas y los valores faltantes.

### Tipos de preguntas y variables

El formato de la pregunta determina cómo quedarán registrados los datos.

| Tipo de pregunta | Tipo de variable |
|---|---|
| Opción única sin orden | Nominal |
| Opción única con orden | Ordinal |
| Sí / No | Binaria |
| Escala 1–5 | Ordinal |
| Respuesta numérica | Cuantitativa |
| Respuesta por tramos | Ordinal |
| Selección múltiple | Varias variables binarias |
| Pregunta abierta | Texto a codificar |

Un aspecto que me pareció importante es que una pregunta de selección múltiple puede generar **varias columnas**, una por cada opción.

### Libro de códigos

También vimos la importancia de crear un **libro de códigos** antes de analizar los datos.

El libro de códigos establece:

- nombre de cada variable;
- qué representa;
- tipo de dato;
- posibles valores;
- orden de las categorías;
- cómo se representan los faltantes.

Esto permite que después los datos puedan interpretarse correctamente y que el significado de cada columna no dependa solamente del nombre que tenga.

### Faltantes estructurales y no respuesta

No todos los valores faltantes significan lo mismo.

Por ejemplo, una pregunta filtro puede hacer que una persona no tenga que responder una pregunta posterior.

En ese caso el valor faltante es **estructural**, porque la pregunta no correspondía.

Esto es diferente de la **no respuesta**, donde la persona debía responder pero no lo hizo.

El libro de códigos debería permitir distinguir ambas situaciones.

### Validez y confiabilidad

Dos conceptos importantes para evaluar una pregunta son:

- **Validez:** que realmente mida lo que se quiere medir.
- **Confiabilidad:** que la medición sea consistente si se vuelve a realizar.

Por eso las preguntas deben ser claras, breves, específicas y tener un período de referencia bien definido cuando sea necesario.

También es importante escribirlas desde el punto de vista de la persona que responde y no solamente desde el punto de vista de quien diseña la encuesta.

### Datos declarados, revelados y huella digital

Vimos que una encuesta es solo una de las formas posibles de generar información sobre personas.

Se pueden distinguir:

- **Datos declarados:** lo que una persona dice en una encuesta.
- **Datos revelados:** lo que efectivamente hace, observado por ejemplo en transacciones.
- **Huella digital:** información registrada automáticamente por un sistema, como clics, ubicación o tiempo de uso.

Aunque puedan intentar medir algo similar, no significan exactamente lo mismo.

### Datos personales

En el diseño de encuestas también se debe considerar la protección de datos personales.

Entre los principios trabajados se encuentran:

- finalidad;
- consentimiento informado;
- pertinencia;
- seguridad y reserva;
- derechos del titular.

La idea es recolectar únicamente la información necesaria para el objetivo definido y dejar claro para qué van a utilizarse los datos.

---

## Carga y transformación de datos

En la segunda parte trabajamos con datos reales de medioambiente, principalmente mediciones de **ozono**, material particulado y conteo vehicular.

### Data profiling

Antes de transformar un dataset se debe realizar una inspección inicial.

Entre los aspectos a revisar están:

- dimensiones del dataset;
- tipos de variables;
- variable o combinación de variables que funciona como identificador;
- valores faltantes;
- duplicados;
- problemas de formato o codificación.

En el ejemplo del ozono, una observación se relaciona con una **estación y una fecha**, por lo que la identificación puede requerir la combinación de ambas variables.

### Problemas de codificación

Al cargar los datos aparecieron nombres como:

```text
ColÃ³n
Curva de MaroÃ±as
```

Esto mostró que los problemas de `encoding` también forman parte de la limpieza de datos.

No alcanza con que el archivo se pueda abrir: hay que verificar que el contenido haya sido interpretado correctamente.

### Valores faltantes y duplicados

Se revisó qué variables contenían valores `NA` y si existían filas duplicadas.

Una idea importante es que no siempre corresponde eliminar inmediatamente un valor faltante. Primero hay que entender **por qué falta**.

En datos provenientes de sensores pueden existir diferentes tipos de faltantes:

- **Explícito:** la observación existe, pero el valor es `NA`.
- **En rachas:** faltan muchos valores consecutivos, posiblemente porque el sensor estuvo fuera de servicio.
- **Implícito:** directamente no existe la fila correspondiente al momento que debería haberse registrado.

Por eso eliminar todos los `NA` automáticamente puede hacer que se pierda información sobre cómo se generaron los datos.

### Fechas y horas

También trabajamos con variables temporales.

En R, `POSIXct` permite representar fechas y horas exactas y trabajar con ellas para:

- obtener año;
- obtener mes;
- obtener hora;
- calcular diferencias entre momentos;
- agrupar observaciones temporalmente.

### Transformación y agregación

Los datos originales de ozono eran mediciones muy frecuentes.

Para analizarlos se pueden transformar:

```text
Minutal → Horario → Diario → Mensual
```

Pero al agregar datos hay que decidir **qué estadístico representa mejor el fenómeno**.

Vimos distintas posibilidades:

- **Media:** representa un nivel típico.
- **Máximo:** permite detectar episodios puntuales, pero es muy sensible a valores extremos.
- **Percentil 95:** representa valores altos sin depender tanto de picos aislados.
- **Cantidad de observaciones (n):** permite conocer cuántos datos respaldan el resultado y evaluar la cobertura.

La elección del estadístico cambia la historia que cuentan los mismos datos.

### Media y percentil 95

También vimos que la media es más sensible a valores extremos porque cada observación influye según su valor.

El percentil 95 depende principalmente de la posición de los datos dentro de la distribución, por lo que es más robusto frente a unos pocos picos extremos.

Esto muestra que transformar y resumir datos no es solamente una operación técnica: implica tomar decisiones sobre qué característica del fenómeno queremos representar.

### Visualización

Finalmente, las transformaciones realizadas se pueden utilizar para generar gráficos y analizar la evolución de las mediciones.

Los elementos básicos de una visualización son:

- dataset;
- variable del eje X;
- variable del eje Y;
- tipo de gráfico;
- título;
- nombres de los ejes.

La visualización aparece después de haber entendido, limpiado y transformado correctamente los datos.
