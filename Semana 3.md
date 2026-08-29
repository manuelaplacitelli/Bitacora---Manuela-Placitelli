# Semana 3

## Interpretación de errores en R y Python

Vimos cómo identificar y diagnosticar problemas en R y Python, diferenciando entre **errores, advertencias y resultados incorrectos que no generan ningún aviso**. :contentReference[oaicite:0]{index=0}

### Tipos de errores frecuentes

- Nombre u objeto inexistente (`NameError`, `KeyError`)
- Errores de sintaxis
- Tipo de dato incorrecto (`TypeError`)
- Valores que no pueden convertirse (`ValueError`)
- Posiciones o columnas inexistentes
- Problemas al leer archivos

También vimos que R y Python pueden reaccionar distinto ante el mismo problema. Por ejemplo, ante valores faltantes Python puede ignorarlos al calcular un promedio, mientras que R devuelve `NA` por defecto. :contentReference[oaicite:1]{index=1}

### Comandos útiles para diagnosticar en Python

```python
"df" in globals()      # verificar si existe
type(x)                # ver el tipo
df.info()              # información general
df.dtypes              # tipo de cada columna
df.shape               # filas y columnas
df.head()               # primeras filas
df.isna().sum()        # contar faltantes
df.columns             # nombres de columnas
```

### Comandos útiles en R

```r
exists("datos")             # verificar si existe
class(x)                    # ver el tipo
str(datos)                  # estructura
sapply(datos, class)        # tipo de cada columna
dim(datos)                  # filas y columnas
head(datos)                 # primeras filas
colSums(is.na(datos))       # contar faltantes
names(datos)                # nombres de columnas

warnings()                  # ver advertencias
traceback()                 # ver origen del último error
```



### Procedimiento ante un error

1. Leer el mensaje completo.
2. Verificar el objeto y su tipo.
3. Encontrar la línea que genera el problema.
4. Comparar lo obtenido con lo esperado.
5. Registrar la causa y la solución. :contentReference[oaicite:4]{index=4}

## Ejercicio 3 - Interpretación de errores

### Python

#### Error 1 - Nombre inexistente

**Línea que produce el error:**

```python
print(datos)
```

**Mensaje:**

```text
NameError: name 'datos' is not defined
```

**Diagnóstico:**

1. **¿Qué tipo de problema es?** Un problema de nombre.
2. **¿Sobre qué objeto ocurrió?** Sobre `datos`.
3. **¿Qué esperaba el programa y qué encontró?** Esperaba encontrar un objeto llamado `datos`, pero ese objeto no había sido definido.

**Corrección:**

```python
datos = [1, 2, 3]
print(datos)
```

---

#### Error 2 - Error de sintaxis

**Línea que produce el error:**

```python
print("hola"
```

**Mensaje:**

```text
SyntaxError: '(' was never closed
```

**Diagnóstico:**

1. **¿Qué tipo de problema es?** Un error de sintaxis.
2. **¿Sobre qué objeto ocurrió?** Sobre la instrucción `print()`.
3. **¿Qué esperaba el programa y qué encontró?** Esperaba encontrar el cierre del paréntesis.

**Corrección:**

```python
print("hola")
```

---

#### Error 3 - Archivo inexistente

**Línea que produce el error:**

```python
import pandas as pd
pd.read_csv("datos.csv")
```

**Mensaje:**

```text
FileNotFoundError: [Errno 2] No such file or directory: 'datos.csv'
```

**Diagnóstico:**

1. **¿Qué tipo de problema es?** Un problema con la ubicación del archivo.
2. **¿Sobre qué objeto ocurrió?** Sobre el archivo `datos.csv`.
3. **¿Qué esperaba el programa y qué encontró?** Esperaba encontrar el archivo en la carpeta de trabajo, pero no estaba allí.

**Corrección:**

```python
from pathlib import Path

Path.cwd()
list(Path.cwd().iterdir())
```

Luego se debe indicar correctamente la ruta del archivo.

---

### R

#### Error 4 - Tipo de dato incorrecto

**Línea que produce el error:**

```r
edad <- "35"
edad + 1
```

**Mensaje:**

```text
Error in edad + 1 : non-numeric argument to binary operator
```

**Diagnóstico:**

1. **¿Qué tipo de problema es?** Un problema de tipo de dato.
2. **¿Sobre qué objeto ocurrió?** Sobre `edad`.
3. **¿Qué esperaba el programa y qué encontró?** Esperaba un valor numérico, pero encontró texto.

**Corrección:**

```r
edad <- 35
edad + 1
```

---

#### Error 5 - Valor que no puede convertirse

**Línea que produce el problema:**

```r
as.numeric("45,0")
```

**Mensaje:**

```text
[1] NA
Warning message:
NAs introduced by coercion
```

**Diagnóstico:**

1. **¿Qué tipo de problema es?** Un problema de conversión de valores.
2. **¿Sobre qué objeto ocurrió?** Sobre el texto `"45,0"`.
3. **¿Qué esperaba el programa y qué encontró?** Esperaba un valor que pudiera interpretarse como número, pero encontró una coma como separador decimal.

**Corrección:**

```r
as.numeric("45.0")
```

---

#### Error 6 - Columna inexistente

**Línea que produce el error:**

```r
datos <- data.frame(peso = c(45, 120.5))

datos[, "pesoo"]
```

**Mensaje:**

```text
Error in `[.data.frame`(datos, , "pesoo") : 
  undefined columns selected
```

**Diagnóstico:**

1. **¿Qué tipo de problema es?** Un problema de selección de columna.
2. **¿Sobre qué objeto ocurrió?** Sobre el data frame `datos`.
3. **¿Qué esperaba el programa y qué encontró?** Esperaba encontrar una columna llamada `pesoo`, pero la columna existente se llama `peso`.

**Corrección:**

```r
datos[, "peso"]
```

### Errores silenciosos

También vimos que algunos problemas no generan un error, por lo que es importante controlar los resultados.

```python
df.dropna()           # calcula el resultado pero no modifica df
df = df.dropna()      # ahora sí se guarda el cambio
```

```r
x <- c(1, NA, 3)

x == NA
# [1] NA NA NA

is.na(x)
# [1] FALSE TRUE FALSE
```

En R, para detectar valores faltantes se debe usar `is.na()` y no comparar directamente con `NA`.

### Ejemplo mínimo reproducible

Cuando aparece un error, conviene reducirlo a un ejemplo pequeño que permita identificar fácilmente la causa.

```python
import pandas as pd

d = pd.DataFrame({"peso": ["45,0", "120,5"]})
d["peso"].mean()
```


### Formatos de datos

Trabajamos con un mismo conjunto de datos guardado en distintos formatos para comparar cómo se cargan y estructuran.

- CSV
- Excel
- Stata (`.dta`)
- SPSS (`.sav`)
- SQLite (`.db`)
- JSON, similar a una respuesta de API

:contentReference[oaicite:0]{index=0}

### CSV

Vimos que al guardar o leer un CSV hay que tener en cuenta el separador, el separador decimal y la codificación.

```python
base.to_csv(
    "ventas_local.csv",
    sep=";",
    decimal=",",
    index=False,
    encoding="latin-1"
)
```

:contentReference[oaicite:1]{index=1}

### Rutas de archivos

```python
from pathlib import Path

BASE = Path(__file__).parent
CARPETA = BASE / "datos_clase"
CARPETA.mkdir(exist_ok=True)
```

Esto permite trabajar con rutas relativas al archivo y no depender de la carpeta desde donde se ejecuta el programa. :contentReference[oaicite:2]{index=2}

### Otros formatos

```python
base.to_excel(...)
base.to_stata(...)
base.to_sql(...)
```

También vimos que un JSON puede contener información anidada, como suele ocurrir en las respuestas de una API. :contentReference[oaicite:3]{index=3}
