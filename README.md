# mcdaa-intro-cd
Repositorio del Curso Introduccion a Ciencia de Datos de la Maestria en Ciencia de Datos y Aprendizaje Automatico

# Pasos para Setup con Pyenv

1. Crear ambiente pyenv y activarlo
```bash
pyenv virtualenv 3.12.0 intro-cd
pyenv activate intro-cd
eval "$(pyenv init --path)"
```

2. Instalar depdencias
```bash
pip install -r requirements.txt
```
python -m spacy download en_core_web_sm

3. Crear un Kernel específico para esto

```bash
ipython kernel install --user --name introcd
```

# Errores conocidos

### AttributeError: 'Engine' object has no attribute 'cursor'

Cuando ejecutamos la siguiente celda de la tarea se obtiene el error mencionado.

```python
# Creamos el directorio Tarea_1/data/shakespeare
data_dir = Path("data") / "shakespeare"
data_dir.mkdir(parents=True, exist_ok=True)


def load_table(table_name, engine):
    """
    Leer la tabla con SQL y guardarla como CSV,
    o cargarla desde el CSV si ya existe
    """
    path_table = data_dir / f"{table_name}.csv"
    if not path_table.exists():
        print(f"Consultando tabla con SQL: {table_name}")
        t0 = time()
        df_table = pd.read_sql(f"SELECT * FROM {table_name}", engine)
        t1 = time()
        print(f"Tiempo: {t1 - t0:.1f} segundos")

        print(f"Guardando: {path_table}\n")
        df_table.to_csv(path_table)
    else:
        print(f"Cargando tabla desde CSV: {path_table}")
        df_table = pd.read_csv(path_table, index_col=[0])
    return df_table


print("Conectando a la base...")
conn_str = "mysql+pymysql://guest:relational@relational.fit.cvut.cz:3306/Shakespeare"
engine = create_engine(conn_str)

# DataFrame con todas las obras:
df_works = load_table("works", engine)

# Todos los párrafos de todas las obras
df_paragraphs = load_table("paragraphs", engine)

# TODO: cargar el resto de las tablas
```

Revisando errores similares en internet encontramos en [stackoverflow](https://stackoverflow.com/questions/38332787/pandas-to-sql-to-sqlite-returns-engine-object-has-no-attribute-cursor) que al parecer con la última version de Pandas (2.2.0) cambió la forma en que se crean las conexiones por lo que hay que cambiar el código para que funcione o hacer un downgrade de pandas.

# Referencias

1. Instalación de Jupyter con pyenv
https://brandonrozek.com/blog/jupyterwithpyenv/

