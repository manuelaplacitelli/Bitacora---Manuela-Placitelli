## Semana 1 - Ejercicio práctico

```python
# Ver las primeras filas
print(pinguinos.head())

# Ver las últimas filas
print(pinguinos.tail())

# Información general
print(pinguinos.info())

# Estadísticas descriptivas
print(pinguinos.describe())

# Filtrar filas por condición
filtro = pinguinos[pinguinos["edad"] > 30]
print(filtro)

# Seleccionar columnas específicas
print(df[["nombre", "edad"]])

# Contar valores únicos
print(df["ciudad"].value_counts())

# Eliminar valores nulos
df = df.dropna()

# Rellenar valores nulos
df["edad"] = df["edad"].fillna(df["edad"].mean())
```

---

## Semana 3 - Ejercicio práctico parte 2

Se agregó una función auxiliar en Python para limpiar la consola, adaptándose al sistema operativo utilizado.

```python
import os

def clearConsole():
    command = "clear"

    if os.name in ("nt", "dos"):
        command = "cls"

    os.system(command)

clearConsole()
```
