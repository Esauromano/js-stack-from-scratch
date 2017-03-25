# 01 - Node, Yarn, y `package.json`

El código para este capítulo esta disponible [aquí](https://github.com/verekia/js-stack-walkthrough/tree/master/01-node-yarn-package-json).

En esta sección configuraremos Node, Yarn, un archivo `package.json` básico y probaremos un paquete.

## Node

> 💡 **[Node.js](https://nodejs.org/es/)** es un entorno de ejecución para Javascript. Es mayormente usado para desarrollo Back-End, pero también para scripting en general. En el contexto de desarrollo Front-End, puede ser usado para realizar una gran cantidad de tareas como linting, pruebas y ensamblamiento de archivos.

Usaremos Node básicamente para todo a lo largo de este tutorial, por tanto vas a necesitarlo. Dirígete a la [página de descargas](https://nodejs.org/es/download/current/) para obtener los binarios para **macOS** o **Windows**, o a la [página de instalación mediante manejador de paquetes](https://nodejs.org/es/download/package-manager/) para distribuciones Linux.

Por ejemplo, en **Ubuntu / Debian**, ejecutarías los siguientes comandos para instalar Node:

```sh
curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Si deseas cualquier versión de Node > 6.5.0.

## Herramientas para gestionar distintas versiones de Node

Si necesitas flexibilidad para utilizar múltiples versiones de Node, checa [Node Version Manager](https://github.com/creationix/nvm), o [tj/n](https://github.com/tj/n).

## NPM

NPM es el manejador de paquetes por defecto de Node. El cual se instala automáticamente con Node. Los manejadores de paquetes son usados para instalar y manejar paquetes (módulos de código que tú o alguien más escribió). Vamos a usar un montón de paquetes en este tutorial, pero usaremos Yarn, otro manejador de paquetes.

## Yarn

> 💡 **[Yarn](https://yarnpkg.com/)** es un manejador de paquetes de Node.js que destaca por ser más rápido que NPM, tiene soporte offline y obtiene dependencias [más previsiblemente](https://yarnpkg.com/en/docs/yarn-lock).

Desde su [llegada](https://code.facebook.com/posts/1840075619545360) en Octubre de 2016, se ha adoptado rápidamente y en poco tiempo se convirtió en el manejador de paquetes preferido en la comunidad JavaScript. Si deseas en cambio usar NPM puedes simplemente reemplazar todos los comandos `yarn add` y `yarn add --dev` de este tutorial, por `npm install --save` y `npm install --save-dev`.

Puede instalar Yarn con las siguientes [instrucciones](https://yarnpkg.com/en/docs/install) para tu sistema operativo. Recomiendo usar el **script de instalación** desde la pestaña *Alternatives* si estas usando macOS o Unix, para [evitar](https://github.com/yarnpkg/yarn/issues/1505) confiar en otros manejadores de paquetes:

```sh
curl -o- -L https://yarnpkg.com/install.sh | bash
```

## `package.json`

> 💡 **[package.json](https://yarnpkg.com/en/docs/package-json)** es el archivo usado para describir y configurar tu proyecto JavaScript. Contiene información general (nombre del proyecto, versión, contribuidores, licencia, entre otros), opciones de configuración para las herramientas que uses e incluso una sección para ejecutar *tareas*.

- Crea un nuevo directorio para trabajar, y entra en este con `cd`.
- Ejecuta `yarn init` y responde las preguntas (`yarn init -y` permite saltarse todas las preguntas), para generar un archivo `package.json` automáticamente.

Aquí puedes ver un `package.json` básico que usaremos en este tutorial:

```json
{
  "name": "tu-proyecto",
  "version": "1.0.0",
  "license": "MIT"
}
```

## Hello world

- Crea un archivo `index.js` que contenga `console.log('Hello world')`

🏁 Ejecuta `node .` en esta carpeta (`index.js` es el archivo que Node buscará por defecto dentro del directorio). Deberá imprimir "Hello world".

**Nota**: ves este emoji de bandera de carrera 🏁? Lo usaré cada vez que llegues a un **punto de control**. Algunas veces vamos a hacer muchos cambios en algunas lineas y tu código no funcionará hasta llegar al siguiente punto de control.

## Script `start`

Usar `node .` para ejecutar nuestro programa es ir a muy bajo-nivel. Vamos a usar un script NPM/Yarn en su lugar para disparar la ejecución del código. Esto nos brindará una buena abstración para poder usar siempre `yarn start`, aun cuando nuestro programa se vuelva mas complejo.

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

`start` es el nombre que le daremos a la *tarea* que ejecutará nuestro programa. Vamos a crear un montón de diferentes tareas en este objeto `scripts` a lo largo de este tutorial. `start` es tipicamente el nombre que le damos a la tarea por defecto de una aplicación. Algunas otras tareas estándar son `stop` y `test`.

`package.json` debe ser un archivo JSON valido, lo cual significa que no puede tener comas finales. Ten mucho cuidado cuando edites manualmente tu archivo `package.json`.

🏁 Ejecuta `yarn start`. Debe `Hello world`.

## Git y `.gitignore`

- Inicializa un repositorio Git con `git init`

- Crea un archivo `.gitignore` y agrega lo siguiente en el:

```gitignore
.DS_Store
/*.log
```

Los archivos `.DS_Store` son auto-generados en macOS, que nunca deberias tener dentro de tu repositorio.

`npm-debug.log` y `yarn-error.log` son archivos que son creados cuando tu manejador de paquetes encuentra un error, no queremos que esten versionados en nuestro repositorio.

## Instalando y usando un paquete

En esta sección instalaremos y usaremos un paquete. Un "paquete" es simplemente una pieza de código que alguien mas escribió y puede ser usado en tu propio código. Puede ser cualquier cosa. Aquí por ejemplo, vamos a probar un paquete que te ayudará a manipular colores.

- Instala el paquete hecho por la comunidad llamado `color` usando `yarn add color`

Abre `package.json` y veras como Yarn automaticamente añadió `color` en `dependencies`.

Una carpeta `node_modules` ha sido creada para almacenar los paquetes.

- Agrega `node_modules/` a tu `.gitignore`

También notarás que un archivo `yarn.lock` fue generado por Yarn. Deberas agregar este archivo a tu repositorio, con esto te aseguras que todo en tu equipo usen la misma versión que tus paquetes. Si estas usando NPM en vez de Yarn, el equivalente de este archivo es el *shrinkwrap*.

- Escribe lo siguiente en tu archivo `index.js`:

```js
const color = require('color')

const redHexa = color({ r: 255, g: 0, b: 0 }).hex()

console.log(redHexa)
```

🏁 Ejecuta `yarn start`. Deberá imprimir `#FF0000`.

¡Felicitaciones, instalaste y usaste un paquete!

`color` es usado en esta sección para enseñarte como usar un simple paquete. No lo necesitaremos mas, así que puedes desintalarlo:

- Ejecuta `yarn remove color`

## Dos tipos de dependecias

Hay dos tipos de dependencias de paquetes, `"dependencies"` y `"devDependencies"`:

**Dependencias** son librerias que necesita tu aplicación para funcionar (React, Redux, Lodash, jQuery, etc). Se instalan usando `yarn add [paquete]`.

**Dependencias De Desarrollo** son librerías usadas durante el desarrollo o contrución de tu aplicación (Webpack, SASS, linters, frameworks de pruebas, etc). Se instalan usando `yarn add --dev [paquete]`.

Siguiente sección: [02 - Babel, ES6, ESLint, Flow, Jest, Husky](02-babel-es6-eslint-flow-jest-husky.md#readme)

Volver a la [tabla de contenidos](https://github.com/JMEspiz/js-stack-from-scratch#table-of-contents).
