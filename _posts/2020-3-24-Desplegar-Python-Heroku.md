---
layout: post
title: Desplegar un App Flask en Heroku
---

Como ejemplo en esta guia voy a desplegar un sencillo Hola Mundo! en Flask enla plataforma Heroku. Para esto sera necesario crear una cuenta gratuita en **Heroku** e instalar Heroku CLI.

Localmente creamos un directorio para nuestra aplicacion:

```sh
mkdir test-app-py && cd test-app-py
```

Creamos ademas un entorno virtual para instalar Flask y las dependencias de nuestra aplicacion de forma aislada de los paquetes de Python en nuestro sistema:

```sh
python -m venv .env && source .env/bin/activate
```

Luego inicializamos nuestra aplicacion con el comando:

```sh
heroku create
```

El comando nos devolvera un nombre generado para nuestra aplicacion, su URL y la URL de su repositorio Git en Heroku.

E inicializamos un repositorio local con git:

```sh
git init
```

Seguido agregamos el repositorio git de la app en heroku como remoto de nuestro repositorio local:

```sh
heroku git:remote -a <nombre de nuestra aplicacion>
```

Instalamos Flask en nuestro entorno virtual con el comando:

```sh
pip install flask
```

Luego creamos un archivo `__init__.py` con el siguiente contenido:

```python

import os
from flask import Flask
app = Flask(__name__)

_port = os.environ.get('PORT', 5000)

@app.route('/')
def hello():
    return "Hello World!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=_port)

```

Heroku requiere que el servicio Web de nuestra aplicacion pueda asignarse a un puerto de forma dinamica mediante la variable de entorno `$PORT`.
Creamos ademas un archivo `Procfile` mediante el cual le indicaremos a Heroku como ejecutar nuestra aplicacion:

```yaml
web: python __init__.py
```

Agregamos un archivo `.gitignore` para indicarle a git que ignore el directorio .env:

```text
.env*
```

Seguido creamos un archivo `requirements.txt` con las dependencias de nuestra aplicacion:

```sh
pip freeze > requirements.txt
```

Con esto, ya podemos hacer el commit inicial de nuestra aplicacion:

```sh
git add *
git commit -am "Version Inicial"
```

Le indicaremos a Heroku que nuestra aplicacion requiere un entorno Python al agregar el **buildpack** de Python:

```sh
heroku buildpacks:set heroku/python
```

Por ultimo, desplegamos la aplicacion con el comando:

```sh
git push heroku master
```
