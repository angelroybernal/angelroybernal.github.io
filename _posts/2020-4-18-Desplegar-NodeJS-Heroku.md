---
layout: post
title: Desplegar un App en ExpressJS en Heroku
---

Como ejemplo en esta guia voy a desplegar un sencillo Hola Mundo! en Express enla plataforma Heroku. Para esto sera necesario crear una cuenta gratuita en **Heroku** e instalar Heroku CLI.

Localmente creamos un directorio para nuestra aplicacion:

```sh
mkdir test-app-py && cd test-app-node
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

Luego creamos un archivo `package.json` con el siguiente contenido:

```json
{
  "name": "test-app-node",
  "version": "1.0.0",
  "description": "Simple node test app",
  "main": "run.js",
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {},
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node run.js"
  },
  "author": "",
  "license": "ISC"
}
```

Y la aplicacion en un archivo `run.js`:

```js
const express = require('express')
const app = express()
const port = process.env.PORT || 3000;

app.get('/', (request, response) => {
  response.send('Hello from Express!')
})

app.listen(port, (err) => {
  if (err) {
    return console.log('something bad happened', err)
  }

  console.log(`server is listening on ${port}`)
})
```

Heroku requiere que el servicio Web de nuestra aplicacion pueda asignarse a un puerto de forma dinamica mediante la variable de entorno `$PORT`.
Creamos ademas un archivo `Procfile` mediante el cual le indicaremos a Heroku como ejecutar nuestra aplicacion:

```yaml
web: npm start
```

Probamos la aplicacion localmente con:

```sh
npm install
npm start
```

Agregamos un archivo `.gitignore` para indicarle a git que ignore el directorio node_modules:

```text
node_modules*
```

Con esto, ya podemos hacer el commit inicial de nuestra aplicacion:

```sh
git add *
git commit -am "Version Inicial"
```

Le indicaremos a Heroku que nuestra aplicacion requiere un entorno NodeJS al agregar el **buildpack** de NodeJS:

```sh
heroku buildpacks:set heroku/nodejs
```

Por ultimo, desplegamos la aplicacion con el comando:

```sh
git push heroku master
```
