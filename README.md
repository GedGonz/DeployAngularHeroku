# Paso 1
Vas a necesitar algo para servir tus archivos. Vamos con el expreso. También necesitaremos una "ruta" para configurar nuestro servidor (a menos que quiera codificar esos en usted)

```js
npm install --save express path
```

# Paso 2
Ahora, si queremos que Heroku construya nuestro proyecto en sus servidores, necesitamos decirles dos cosas.

1. Cómo construir nuestro proyecto y 
2. Con qué versiones de node / npm trabaja nuestro código.

Puedes hacer esto poniendo lo siguiente en package.json

```javascript

"scripts": {
    ...
    "postinstall": "ng build --prod"
  },
  "engines": {
    "node": "8.11.3",
    "npm": "6.1.0"
  },

```

Recuerda reemplazar las versiones de node y npm por las que tienes. 
Puedes averiguarlo con

```js

node --version
npm --version
```
# Paso 3
De forma predeterminada, el ángulo separa de las implementaciones lo que cree que son adiciones de "desarrollo" únicamente. Sin embargo, dado que Heroku está construyendo nuestro código, debemos darle la capacidad de ejecutar esos módulos.

Para hacer esto usted puede mover *@angular/cli, @angular/compiler-cli*, *typescripty* *"@angular-devkit/build-angular": "~0.6.8"*__ __ * de nuestros devDependencies a las dependencias. O podemos hacer que Heroku instale esos módulos por su cuenta.

Personalmente prefiero la primera ya que le permite especificar versiones, sin embargo, si desea hacer la última, debería colocar lo siguiente debajo de postinstall.

```javascript
 "preinstall": "npm install -g @angular/cli @angular/compiler-cli typescript",
```

# Etapa 4

Crea nuestro archivo de servidor. En su directorio de aplicación principal (el que tiene package.json) cree un archivo llamado server.js. Agrega lo siguiente


```js

const path = require('path');
const express = require('express');
const app = express();

// Serve static files
app.use(express.static(__dirname + '/dist/MY_APP_NAME'));

// Send all requests to index.html
app.get('/*', function(req, res) {
  res.sendFile(path.join(__dirname + '/dist/MY_APP_NAME/index.html'));
});

// default Heroku port
app.listen(process.env.PORT || 5000);

```

Recuerde reemplazar MY_APP_NAME (ambos) al nombre de su aplicación.

# Paso 5

Ahora, para crear un *Procfile* para decirle a Heroku "cómo" queremos que se ejecute nuestra aplicación. En su directorio de proyecto (el mismo con package.json) cree un archivo llamado Procfiley coloque lo siguiente

```javascript
 web: node server.js
```

# Paso 6. Paso final

Ahora podemos compilar nuestra aplicación *npm install* y ejecutarla con 'node server.js'. 
Si todo funciona, ahora deberíamos ver un sitio de trabajo en http: // localhost: 5000

Si tiene algún problema no dude en dejar un mensaje en los comentarios.

Para empujar a heroku, asumiendo que tienes el cli instalado. 
Si no es así (https://devcenter.heroku.com/articles/heroku-cli#download-and-install)