Trabajamos con operaciones básicas para explorar y manipular un DataFrame.

# Ver las primeras filas
pinguinos.head()

# Ver las últimas filas
pinguinos.tail()

# Información general
pinguinos.info()

# Estadísticas descriptivas
pinguinos.describe()

# Ver columnas
pinguinos.columns

# Filtrar filas
filtro = pinguinos[pinguinos["edad"] > 30]

# Seleccionar columnas
df[["nombre", "edad"]]

# Contar valores únicos
df["ciudad"].value_counts()

# Eliminar valores nulos
df = df.dropna()

# Rellenar valores nulos con la media
df["edad"] = df["edad"].fillna(df["edad"].mean())



### Introducción a Git

Vimos Git como herramienta para registrar cambios, mantener un historial del trabajo y trabajar de forma colaborativa mediante GitHub.

#### Flujo básico

```bash
git add <archivo>          # preparar cambios
git commit -m "mensaje"   # guardar una versión
git push                   # subir cambios a GitHub
git pull                   # traer cambios desde GitHub
```

#### Historial y cambios

```bash
git status                 # ver el estado del repositorio
git log --oneline          # ver historial de commits
git diff                   # ver cambios realizados
```

#### Ramas

```bash
git switch -c <rama>       # crear una rama
git switch main            # volver a main
git merge <rama>           # incorporar una rama a la actual
```

#### Otros comandos

```bash
git clone <url>            # clonar un repositorio
git tag hito-1             # marcar una versión o entrega
```

También vimos el uso de `.gitignore` para indicar qué archivos no queremos versionar y el concepto de **Pull Request** para revisar cambios antes de incorporarlos a la rama principal.
