# mcdaa-intro-cd

<p align="center">
  <img src="assets/William-Shakespeare.png" style="width: 350px"/>
</p>

William Shakespeare, nació el 23 de abril de 1564 en Stratford-upon-Avon, Inglaterra y en lo que hoy en día parecería una vida corta (52 años), se transformó en una figura titánica del mundo de la literatura. Este dramaturgo y poeta inglés dejó un legado imborrable con sus más de 39 obras literarias, existen al menos dos corrientes que discuten incluso hoy en día la atribución de ciertas obras, entre entre las que se destacan sus tragedias y comedias, obras como "Hamlet", "Romeo y Julieta" y "El rey Lear". Ya sea si has leído alguna obra de William Shakespeare o no, es muy probable que reconozcas algunas frases con origen en su obra como "Ser o no ser, esa es la cuestión" o "El amor es un humo hecho con el vapor de suspiros". Estas líneas no solo demuestran su maestría lingüística, sino que también reflejan las intrigas universales sobre el amor, el poder y la tragedia, manteniendo su relevancia a través de los siglos.

En este trabajo llevado adelante en el contexto del primer Laboratorio [2] del curso Introducción a la Ciencia de Datos de la Facultad de Ingeniería, UdelaR, edición 2024, nos proponemos adentrarnos en la obra de William Shakespeare con un enfoque de ciencia de datos, analizando sus principales obras utilizando algunas técnicas sencillas de análisis de datos.

Esperamos que disfrutes este viaje a través de los datos, el tiempo y principalmente, de la lengua inglesa, tanto como nosotros lo hemos disfrutado.


_"Ten más de lo que muestras habla menos de lo que sabes."_

_William Shakespeare_

# Pasos para Setup con Pyenv

Ejecutar los siguientes pasos en una terminal:

1. Crear ambiente pyenv y activarlo

[OPCIONAL]: En caso de no tener instalado aún Python 3.12.0
```bash
pyenv install 3.12.0
```

Luego:

```bash
pyenv virtualenv 3.12.0 intro-cd
pyenv activate intro-cd
eval "$(pyenv init --path)"
```

2. Instalar depdencias
```bash
pip install -r requirements-lock.txt
```

3. Descarga Modelo Spacy
```bash
python -m spacy download en_core_web_sm
```

3. Crear un Kernel específico para esto

```bash
ipython kernel install --user --name introcd
```

Ahora debería poder ejecutar el siguiente comando para levantar jupyter notebook localmente y correr el laboratorio:

```bash
jupyter notebook laboratorio_1.ipynb
```
<p align="center">
  <img src="assets/image_02.png" style="width: 200px"/>
</p>

También puede ejecutarlo desde VSCode o Google Colab.

# Informe

El informe detallado elaborado a partir de los experimentos conducidos en este proyecto se encuentran en el archivo [Laboratorio_1_Informe.pdf](Laboratorio_1_Informe.pdf).

# Experimentos

Se realizaron varios experimentos a lo largo de este trabajo laboratorio que pueden encontrarse en la carpeta [notebooks](notebooks). Los experimentos finales de los que se deducen los resultados registrados en el informe final, consolidados en un único archivo se encuentran en [laboratorio_1.ipynb](laboratorio_1.ipynb).

# Errores conocidos

Algunos errores a los que nos enfrentamos durante esta tarea y que también te podrían ocurrir a tí, en esta sección.

### AttributeError: 'Engine' object has no attribute 'cursor'

Cuando ejecutamos la siguiente celda de la tarea se obtiene el error mencionado. Esta celda se encarga de descargar los datos desde el repositorio online de Shakespeare. Lo que sucede es que versiones más recientes de pandas cambiaron esto.

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

Nosotros lo dejamos así:

```python
def load_table(data_dir, table_name, engine):
    """
    Leer la tabla con SQL y guardarla como CSV,
    o cargarla desde el CSV si ya existe
    """
    path_table = data_dir / f"{table_name}.csv"
    if not path_table.exists():
        print(f"Consultando tabla con SQL: {table_name}")
        t0 = time()
        with engine.connect() as conn:
            df_table = pd.read_sql(
                sql=f"SELECT * FROM {table_name}", con=conn.connection
            )
        t1 = time()
        print(f"Tiempo: {t1 - t0:.1f} segundos")

        print(f"Guardando: {path_table}\n")
        df_table.to_csv(path_table)
    else:
        print(f"Cargando tabla desde CSV: {path_table}")
        df_table = pd.read_csv(path_table, index_col=[0])
    return df_table
```

# Referencias

1. Instalación de Jupyter con pyenv
https://brandonrozek.com/blog/jupyterwithpyenv/

