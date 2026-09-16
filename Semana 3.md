# Semana 4 - Carga de datos

## Organización de un proyecto de datos

Vimos una estructura básica para mantener ordenados los archivos de un proyecto:

```text
data/raw/         # datos originales
data/processed/   # datos procesados
notebooks/        # notebooks
src/ o scripts/   # código
outputs/          # resultados
```

## Rutas

Se recomienda utilizar rutas relativas en lugar de absolutas para que el proyecto sea reproducible en distintas computadoras.

### R

```r
getwd()   # ver el directorio de trabajo
setwd("ruta")   # cambiar el directorio
```

### Python

```python
import os

os.getcwd()      # ver directorio actual
os.chdir("..")   # subir un nivel
```

### Manejo moderno de rutas

En R:

```r
library(here)

ruta_csv <- here(
    "clases",
    "clase4",
    "cantidad_de_residuos_en_la_estacion_de_transferencia_2023.csv"
)
```

En Python:

```python
from pathlib import Path

BASE_DIR = Path.cwd()

ruta_csv = BASE_DIR / "clases" / "clase4" / "cantidad_de_residuos_en_la_estacion_de_transferencia_2023.csv"
```

## Carga de un CSV

En Python:

```python
import pandas as pd

if not ruta_csv.exists():
    raise FileNotFoundError(f"No se encontró el archivo en: {ruta_csv}")

df = pd.read_csv(ruta_csv)

df.info()
df.head()
```

En R:

```r
library(readr)

df <- read_csv(ruta_csv)

spec(df)
head(df)
```

## Data profiling

Antes de comenzar el análisis se revisa la estructura y calidad del dataset.

```python
df.info()
df.shape
df.head()
```

### Identificación de variables ID

```python
df["matricula_letra"].nunique()
```

La unidad de observación indica qué representa cada fila del dataset. Identificar correctamente los IDs es importante para detectar registros únicos y poder relacionar distintas tablas.

### Valores faltantes

```python
df.isnull().sum()
(df == "").sum()
```

### Duplicados

```python
df.duplicated().sum()
```

Estos controles permiten detectar problemas de completitud, valores faltantes, identificadores y registros duplicados antes de comenzar el análisis.


### Pathlib

Vimos la jerarquía de clases de `pathlib` y cómo `Path` selecciona automáticamente la implementación correspondiente según el sistema operativo.

<img width="1344" height="797" alt="image" src="https://github.com/user-attachments/assets/ae77654a-44d1-4ae1-86e8-fd68c437a4ae" />
