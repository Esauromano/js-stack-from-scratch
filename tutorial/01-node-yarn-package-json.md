# 01 - Node, Yarn, y `package.json`

El código para este capítulo esta disponible [aquí](https://github.com/JMEspiz/js-stack-walkthrough/tree/master/01-node-yarn-package-json).

en esta sección configuraremos Node, Yarn, un archivo `package.json` básico y probaremos un paquete.

## Node

> 💡 **[Node.js](https://nodejs.org/)** es un entorno de ejecución Javascript. Es mayormente usado para desarrollo Back-End, pero también para scripting en general. En el contexto de desarrollo Front-End, puede ser usado para realizar una gran cantidad de tareas como linting, pruebas y ensamblamiento de archivos.

Usaremos Nodo a lo largo de este tutorial, por tanto va a necesitarlo. Diríjase a la [página de descarga](https://nodejs.org/en/download/current/) para los binarios de **macOS** o **Windows**, o la [página de instalación de manejador de paquetes](https://nodejs.org/en/download/package-manager/) para su distribuciones Linux.

Por ejemplo, en **Ubuntu / Debian**, correría el siguiente comando para instalar Node:

```sh
curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Si desea cualquier versión de Node > 6.5.0.

## NVM

Si Node esta ya instalado, o si desea más flexibilidad, instale NVM ([Node Version Manager](https://github.com/creationix/nvm)), y gracias a NVM instale y use la versión ḿás reciente de Node.

## NPM

NPM es el manejador de paquetes por defecto de Node. El cual se instala automaticametne con Node. Los manejadores de paquetes son usados para instalar y manejar paquetes (modulos del código que tú o alguien más escribió). Vamos a usar un montón de paquetes en este tutorial, pero usaremos Yarn, otro manejador de paquetes.

## Yarn

> 💡 **[Yarn](https://yarnpkg.com/)** es un manejador de paquetes de Node.js que destaca por ser más rápido que NPM, tiene soporte offline y obtiene dependencias [más previsiblemente](https://yarnpkg.com/en/docs/yarn-lock).

Desde su [llegada](https://code.facebook.com/posts/1840075619545360) en Octubre de 2016, ha recivido una rápida adopción y en poco tiempo se convirtió en el manejador de paquetes por preferencia de la comunidad JavaScript. Si desea en cambio usar NPM puede simplemente reemplazar todos los comandos `yarn add` y `yarn add --dev` de este tutorial por `npm install --save` y `npm install --save-dev`.

Puede instalar Yarn con las siguientes [instrucciones](https://yarnpkg.com/en/docs/install) para tu Sistema Operativp. Recomiendo usar el **Script de instalación** desde la tab de *Alternativas* si esta usando macOS o Unix, para [evitar](https://github.com/yarnpkg/yarn/issues/1505) confiar en otros manejadores de paquetes:

```sh
curl -o- -L https://yarnpkg.com/install.sh | bash
```

## `package.json`

> 💡 **[package.json](https://yarnpkg.com/en/docs/package-json)** es el archivo usado para describir y configurar tu proyecto JavaScript. Contiene información general (nombre del proyecto, versión, contribuidores, licencia, entre otros), opciones de configuración para las herramientas que use e incluso una sección para correr *tareas*.

- Cree una nueva carpeta para trabajar, y entre en ella con `cd`.
- Corra `yarn init` y responda las preguntas (`yarn init -y` para saltarse todas las preguntas), para generar un archivo `package.json` automaticamente.

Aquí puede ver un `package.json` básico que usaremos en este tutorial:

```json
{
  "name": "tu-proyecto",
  "version": "1.0.0",
  "license": "MIT"
}
```

## Hello world

- Crea un archivo `index.js` que contenga `console.log('Hello world')`

🏁 Corre `node .` en esta carpeta (`index.js` es el archivo que Node buscará por defecto dentro de la carpeta). Debería imprimir "Hello world".

**Nota**: ves es emoji de bandera de carrera 🏁? Lo usaré cada vez que llegues a un **punto de control**. Algunas veces vamos a hacer muchos cambios en algunas lineas y su código no funcionará hasta llegar al siguiente punto de control.

## Script `start`

Usar `node .` para ejecutar nuestro programa es ir a muy bajo-nivel. Vamos a usar una script NPM/Yarn en su lugar para disparar la ejecución del código. Esto nos brindará una buena abstración para poder siempre usar `yarn start`, aun cuando nuestro programa se vuelva mas complejo.

- En `package.json`, agrega un objeto `scripts` de la siguiente forma:

```json
{
  "name": "tu-proyecto",
  "version": "1.0.0",
  "license": "MIT",
  "scripts": {
    "start": "node ."
  }
}
```

`start` es el nombre que le daemos a la *tarea* que correr nuestro programa. Vamos a crear un montón de diferentes tareas en este objeto `scripts` a lo largo de este tutorial. `start` es tipicamente el nombre que le damos a la tarea por defecto de una aplicación. Algunas otras tareas estandard son llamadas `stop` y `test`.

`package.json` debe ser un archivo JSON valido, lo cual significa que no puede tener comillas. Tenga mucho cuidado cuando edite manualmente tu archivo `package.json`.

🏁 Corre `yarn start`. Debería imprimir `Hello world`.

## Git y `.gitignore`

- Inicializa un repositorio Git con `git init`

- Crea un archivo `.gitignore` y agrega lo siguiente en el:

```gitignore
.DS_Store
/*.log
```

Los archivos `.DS_Store` son auto-generados en macOS que nunca deberias tener dentro de tu repositorio.

`npm-debug.log` y `yarn-error.log` son archivos que son creados cuando tu manejador de paquetes encuentra un error, no queremos que esten versionados en nuestro repositorio.

## Instalando y udando un paquete

En esta sección intalaremos y usaremos un paquete. Un "paquete" es simplemente una pieza de código que alguien mas escribió y puede ser usado en tu propio código. Pueden ser cualquier cosa. Aquí, vamos a probar un paquete que le ayudará a manipular los colores por ejemplo.

- Instala el paquete hecho por la comunidad llamado `color` usando `yarn add color`

Abre `package.json` y veras como Yarn automaticamente añadió `color` en  `dependencies`.

Una carpeta `node_modules` ha sido creada para almacenar los paquetes.

- Agrega `node_modules/` a tu `.gitignore`

También notará que un archivo `yarn.lock` fue generado por Yarn. Deberías agregar de este archivo a tu repositorio, con esto te aseguras que todo en tu equipo usen la misma versión que tus paquetes. Si estas usando NPM en vez de Yarn, el equivalente de este archivo es el *shrinkwrap*.

- Escribe los siguiente en tu archivo `index.js`:

```js
const color = require('color')

const redHexa = color({ r: 255, g: 0, b: 0 }).hex()

console.log(redHexa)
```

🏁 Correo `yarn start`. Debería imprimir `#FF0000`.

¡Felicitaciones, instaló y uso un paquete!

`color` es usado en esta sección para enseñarle como usar un simple paquete. No lo necesitaremos mas, así que puedes desintalarlo:

- Usa `yarn remove color`

## Dos tipos de dependecias

Hay dos tipo sde dependencias de paquetes, `"dependencies"` y `"devDependencies"`:

**Dependencias** son librerias que necesita tu aplicación para funcionar (React, Redux, Lodash, jQuery, etc). Las instala usando `yarn add [paquete]`.

**Dependencias De Desarrollo** son libreias usadas durante el desarrollo o para contruir tu aplicación (Webpack, SASS, linters, frameworks de pruebas, etc). La instala usando `yarn add --dev [paquete]`.

Siguiente Sección: [02 - Babel, ES6, ESLint, Flow, Jest, Husky](02-babel-es6-eslint-flow-jest-husky.md#readme)

Volver a la [tabla de contenidos](https://github.com/JMEspiz/js-stack-from-scratch#table-of-contents).