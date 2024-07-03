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

```bash
python -m spacy download en_core_web_lg
```

3. Crear un Kernel específico para esto

```bash
ipython kernel install --user --name introcd
```

Ahora debería poder ejecutar el siguiente comando para levantar jupyter notebook localmente y correr el laboratorio:

```bash
jupyter notebook laboratorio_2.ipynb
```
<p align="center">
  <img src="assets/image_02.png" style="width: 200px"/>
</p>

También puede ejecutarlo desde VSCode o Google Colab, sin embargo recomendamos el uso de JupyterLab para una mejor experiencia.

# Informes y

El informe detallado elaborado a partir de los experimentos conducidos en este proyecto se encuentran en el archivo [Laboratorio_2_Informe_v1.0.pdf](Laboratorio_2_Informe_v1.0.pdf).

# Experimentos

Se realizaron varios experimentos a lo largo de este trabajo laboratorio que pueden encontrarse en la carpeta [notebooks](notebooks). Los experimentos finales de los que se deducen los resultados registrados en el informe final, consolidados en un único archivo se encuentran en [laboratorio_2.ipynb](laboratorio_2.ipynb).

Adicionalmente, se realizaron experimentos con la biblioteca Fasttext que se encuentran implementados el en [notebooks/laboratorio_2_ev_fasttext_classifier.ipynb](notebooks/laboratorio_2_ev_fasttext_classifier.ipynb).

# Referencias

1. Instalación de Jupyter con pyenv
https://brandonrozek.com/blog/jupyterwithpyenv/

2. Spacy Models Downlaod
https://spacy.io/usage/models 

