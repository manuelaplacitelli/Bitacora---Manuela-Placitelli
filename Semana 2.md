Trabajamos con la configuración de Python y su integración con R mediante `reticulate`.

### R y Python

```r
version()
version

reticulate::repl_python()

reticulate::py_install("pandas")

reticulate::py_require(c("pandas", "numpy", "matplotlib"))

reticulate::repl_python()
```

### Reproducibilidad

```python
np.random.seed(42)
```

Se utilizó una semilla para poder reproducir los mismos resultados en procesos aleatorios.

### Problemas con Python y pip

En Windows, vimos cómo verificar Python e instalar paquetes cuando `pip` no es reconocido: :contentReference[oaicite:0]{index=0}

```bash
py --version
py -m pip install pandas
```

También vimos cómo comprobar la instalación:

```bash
python --version
pip install pandas
where.exe python
```

En caso de problemas, se revisó la configuración de Python en las variables de entorno y los alias de ejecución de Windows. :contentReference[oaicite:1]{index=1}

### En macOS

```bash
python3 --version



## Clase 2 - Generación de datos

Trabajamos con el concepto de **Proceso Generador de Datos (DGP)** y con la creación de datos simulados y sintéticos. :contentReference[oaicite:0]{index=0}

### Tipos de variables

- Nominales
- Ordinales
- Numéricas discretas
- Numéricas continuas
- Metadatos :contentReference[oaicite:1]{index=1}

### Crear un DataFrame en Python

```python
import pandas as pd
import numpy as np

datos_caja = {
    "objeto_id": [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    "textura": ["suave", "rugoso", "poroso", "suave", "rugoso",
                "suave", "poroso", "rugoso", "suave", "poroso"],
    "rigidez": ["blando", "rígido", "medio", "blando", "rígido",
                "rígido", "blando", "medio", "medio", "blando"],
    "num_aristas": [0, 8, 0, 4, 12, 0, 0, 6, 4, 0],
    "peso_estimado_g": [45.0, 120.5, 15.0, 85.0, 210.0,
                        35.0, 18.5, 95.0, 70.0, 22.0],
    "certeza_observador": [5, 4, 3, 5, 4, 2, 4, 3, 4, 3]
}

df_python = pd.DataFrame(datos_caja)
```

### Convertir variables categóricas

```python
df_python["textura"] = df_python["textura"].astype("category")

df_python["rigidez"] = pd.Categorical(
    df_python["rigidez"],
    categories=["blando", "medio", "rígido"],
    ordered=True
)

df_python.info()
df_python.head()
```

:contentReference[oaicite:2]{index=2}

### Datos sintéticos

Vimos dos métodos: **Bootstrap**, que realiza muestreo con reemplazo, y **generación paramétrica**, utilizando una distribución probabilística. :contentReference[oaicite:3]{index=3}

#### Bootstrap

```python
np.random.seed(42)

df_bootstrap = (
    df_python
    .sample(n=100, replace=True)
    .reset_index(drop=True)
)
```

#### Generación paramétrica

```python
mu = df_python["peso_estimado_g"].mean()
sigma = df_python["peso_estimado_g"].std()

pesos_sinteticos = np.random.normal(
    loc=mu,
    scale=sigma,
    size=500
)

pesos_sinteticos = np.clip(
    pesos_sinteticos,
    a_min=1.0,
    a_max=None
)

df_sintetico_pesos = pd.DataFrame({
    "peso_estimado_g": pesos_sinteticos,
    "tipo": "Sintético"
})
```

:contentReference[oaicite:4]{index=4}

### Limitaciones

Los datos sintéticos pueden no representar toda la complejidad de los datos reales y pueden introducir sesgos si el modelo utilizado para generarlos no representa correctamente el DGP. :contentReference[oaicite:5]{index=5}
python3 -m pip install pandas
which -a python3
```
