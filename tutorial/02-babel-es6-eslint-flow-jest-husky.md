# 02 - Babel, ES6, ESLint, Flow, Jest, and Husky

El código para este capítulo esta disponible [aquí](https://github.com/verekia/js-stack-walkthrough/tree/master/02-babel-es6-eslint-flow-jest-husky).

Vamos a usar un poco de la sintaxis de ES6, la cual aporta una gran mejora en comparación con la sintaxis "antigua" de ES5. No obstante, todos los navegadores y entornos JS comprenden bien ES5, pero no ES6. Es allí donde una herramienta llamada Babel ¡viene al rescate!

## Babel

> 💡 **[Babel](https://babeljs.io/)**  es un compilar que transforma el código ES6 (y optras cosas como las sintaxis JSX de React) en código ES5. Es muy modular y puede ser usado en miles de [entornos](https://babeljs.io/docs/setup/) diferentes. Es por mucho el compilador ES% preferido por la comunidad de React.

- Mueve tu `index.js` a una nueva carpeta llamada `src`. Aquí es donde escribiras tu código ES6. Reemplaza el código anterior relacionado a `color` en `index.js` por:

```js
const str = 'ES6'
console.log(`Hello ${str}`)
```

Estamos usando  un *template string*, el cual e suna nueva característica de ES6 que nos permite inyectar variables dentro de los string sin concatenacion, usando `${}`. Note que los template strings son creados usando **backquotes**.

- Corra `yarn add --dev babel-cli` para instalar la CLI para Babel.

La CLI de Babel trae consigo [dos ejecutables](https://babeljs.io/docs/usage/cli/): `babel`, el cual compila los archivos ES6 a nuevos archivos ES5, y `babel-node`, el cual puede usar para reemplazar el llamado del binario de `node` y ejecutar archivos ES6 directamente sobre la marcha. `babel-node` es excelente para desarrollo pero es muy pesado y no esta orientado para su uso en producción. En este capítulo vamos a usar `babel-node` para preparar nuestro entorno de desarrrollo y en el siguiente usaremos `babel` para construri los archivos ES5 para producción.

- En el `package.json`, en tu script `start`, reemplaza `node .` por `babel-node src` (`index.js` es el archivo por defecto que Node buscará, por eso podemos omitir `index.js`).

Si pruebas corriendo `yarn start` ahora, debería impirmir la salida correcta, pero Babel no esta haciendo nada actualmente. Eso se debe a que, no le dimos ninguna información sobre cuales transformaciones queriamos aplicar. La razón de que haya impreso la salida correcta es porque Node nativamente entiende ES6 son ayuda de Babel. En cambio algunos navegadores o viejas versiones de Node el resultado no será el correcto.

- Corre `yarn add --dev babel-preset-env` Para instalar un paquete predefinido de Babel llamado `env`, que posee las configuraciones para las características más recientes de ECMAScript soportadas por Babel.

- Crea un archivo `.babelrc` en la raíz de tu proyecto, el cual es un archivo JSON para tu configuración de Babel. Escribe lo siguiente para hacer que Babel user el `env`:

```json
{
  "presets": [
    "env"
  ]
}
```

🏁 `yarn start` debería funcionar todavía, pero en realidad está haciendo algo ahora. Realmente no podemos saber si es así, ya que estamos usando `babel-node` para interpretar el código ES6 sobre la marcha. Pronto tendrás una prueba de que tu código ES6 se transforma realmente cuando llegas a la sección [Sintaxis Módulos ES6](#sintaxis-modulos-es6) de este capítulo.

## ES6

> 💡 **[ES6](http://es6-features.org/)**: Es la mejora más significativa del lenguaje JavaScript. Hay muchas nuevas características ES6 para listarlas todas aquí, pero el código ES6 tipicamente usa clases con `class`, `const` and `let`, template strings, and arrow functions (`(text) => { console.log(text) }`).

### Creando una Clase en ES6

- Crea un nuevo archivo, `src/dog.js`, que contenga la siguiente clase ES6:

```js
class Dog {
  constructor(name) {
    this.name = name
  }

  bark() {
    return `Wah wah, I am ${this.name}`
  }
}

module.exports = Dog
```

Esto no sería nada nuevo si has tenido la oportunidad de usar OOP (Programación Orientada a Objetos) en cual otro lenguaje. Sin embargo, es algo relativamente nuevo en JavaScript. LA clase es expuesta al mundo entero vía asignación al `module.exports`.

En `src/index.js`, escribe lo siguiente:

```js
const Dog = require('./dog')

const toby = new Dog('Toby')

console.log(toby.bark())
```

Como puedes apreciar, a diferencia del paquete hecho por la comunidad `color` que usamos anteriormente, En el caso que requiramos uno de nuestros archivos, usaremos `./` en el `require()`.

🏁 Corre `yarn start` y debería imprimir "Wah wah, I am Toby".

### Sintaxis modulos ES6

Aquí simplemente reemplaza `const Dog = require('./dog')` por `import Dog from './dog'`, esta es la nueva sintaxis de modules de ES6 (En contraposición a la sintaxis de los módulos "CommonJS"). Actualmente, es soportada forma nativa por NodeJS, lo que la hace la prueba de que Babel procesa correctamente esos archivos ES6.

En `dog.js`, también reemplaza `module.exports = Dog` por `export default Dog`

🏁 `yarn start` todavía debería imprimir "Wah wah, I am Toby".

## ESLint

> 💡 **[ESLint](http://eslint.org)** es el linter de elegido para código ES6. Un linter nos ofrece recomendaciones sobre el formato del código, asegurando un estilo consistenten en su código y el código que compartes con sus compañeros. También es una excelente manera de aprender sobre JavaScript al cometer errores que ESLint ira encontrando.

ESLint trabaja con *rules (reglas)*,  hay [muchas de ellas](http://eslint.org/docs/rules/). No obstante, en vez de configurar las reglas que queramos para nuestro código, usaremos la configuración creada por Airbnb. Esta configuración usa algunos plugins que necesitamos instalar para que funcionen correctamente.

Consulte las [instructiones](https://www.npmjs.com/package/eslint-config-airbnb) más recientes de Airbnb para instalar el paquete de configuración y sus respectivas dependencias correctamente. A partir de 2017-02-03, recomiendan el uso del siguiente comando en su terminal:

```sh
npm info eslint-config-airbnb@latest peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs yarn add --dev eslint-config-airbnb@latest
```

Con esto se debería instalar todo lo necesario y agregar `eslint-config-airbnb`, `eslint-plugin-import`, `eslint-plugin-jsx-a11y`, y `eslint-plugin-react` a su archivo`package.json` automaticamente.

**Nota**: He reemplazado `npm install` por `yarn add` en este comando. Adicionalmente,  esto no funcionará en Windows, por tanto revise el archivo `package.json` de este repositorio e instale todas las dependencias relacionadas con ESLint manualemnte usando `yarn add --dev packagename@^#.#.#` donde `#.#.#` sea la versión encontrada en el `package.json` para cada paquete.

- Crea un archivo `.eslintrc.json` en la raiz de su proyecto, tal cual como hicimos para Babel, y escribe lo siguiente en el:

```json
{
  "extends": "airbnb"
}
```

Crearemos un script NPM/Yarn para correr ESLint. Instalemos el paquete `eslint` para poder usar la CLI de `eslint`:

- Corre `yarn add --dev eslint`

Actualiza el `scripts` de tu `package.json` para incluir una nueva tarea llamada `test`:

```json
"scripts": {
  "start": "babel-node src",
  "test": "eslint src"
},
```

Esto le dirá a ESLint que queremos revisar todos los archivos JavaScript dentro de la carpeta `src`.

Usaremos la tarea `test` estándar para correr todos los comandos que validarán nuestro código, ya sea para hacer linting, vericar tipado, o test unitarios.

- Correr `yarn test`, y deberías ver un montón de errores por falta de punto y coma, y advertencias para usar `console.log()` en `index.js`. Agrega `/* eslint-disable no-console */`en la parte superior del archivo`index.js` para permitir el uso de `console` en este archivo.

**Nota**: Si estas en Windows, asegúrese de configurar su editor y Git para utilizar terminaciones de línea Unix LF y no Windows CRLF. Si tu proyecto es solo usado en entornos Windows, puedes agregar `"linebreak-style": [2, "windows"]` en el array de `rules` de ESLint (Vea el ejemplo a continuación) para reforzar CRLF en su lugar.

### Punto y Comas

Bien, este es probablemente el debate más acalorado en la comunidad de JavaScript, vamos a hablar de ello por un minuto. JavaScript tiene esta cosa llamada Inserción automática de Punto y Comas, que le permite escribir su código con o sin punto y coma. Realmente se reduce a la preferencia personal y no hay una opción correcta o incorrecto en este tema. Si te gusta la sintaxis de Python, Ruby o Scala, probablemente disfrutarás de omitir puntos y comas. Si prefiere la sintaxis de Java, C # o PHP, probablemente prefiere usar puntos y comas.

La mayoría de la gente escribe JavaScript con punto y coma, por costumbre. Ese fue mi caso hasta que intenté probar escribir código sin punto y coma, después de ver muestras de código de la documentación de Redux. Al principio me sentí un poco extraño, simplemente porque no estaba acostumbrado. Después de sólo un día de escribir código de esta manera no podía verme usando puntos y coma otra vez. Se sentía incómodo e innecesario. Desde mi opinión el código sin punto y coma es más ameno a la vista y es más rápido de tipear.

Yo recomiendo leer la [documentación de ESLint sobre punto y comas](http://eslint.org/docs/rules/semi). Como mencione en está página, si decide escribir código sin punto y coma, hay algunos casos algo extraños en donde se requerira el uso de punto y coma. ESLint puede protegerlo de esos casos con la regla `no-unexpected-multiline`. Configuraremos ESLint para trabajar de forma segura siguiendo el estándar de escribir código sin punto y coma, para eso coloquemos en nuestro `.eslintrc.json` lo siguiente:

```json
{
  "extends": "airbnb",
  "rules": {
    "semi": [2, "never"],
    "no-unexpected-multiline": 2
  }
}
```

🏁 Corre `yarn test`, ahora debe pasar con éxito. Intente agregar un punto y coma innecesario en algún lugar para asegurarse de que la regla esté configurada correctamente.

Soy consciente de que algunos de ustedes querrán seguir utilizando puntos y comas, lo que hará que el código proporcionado en este tutorial sea un inconveniente. Si estás usando este tutorial sólo para aprender, estoy seguro de que seguirá siendo soportable aprender sin punto y coma, hasta volver a usarlos en tus proyectos reales. Si desea utilizar el código proporcionado en este tutorial como una plantilla sin embargo, se requerirá un poco de reescritura, lo que debería ser bastante rápido con ESLint establecido para aplicar puntos y comas para guiarlo a través del proceso. Me disculpo si estás en ese caso.

### ESLint en tu Editor

En este capítulo configurará ESLint en la terminal, lo cual es una excelente manera de detectar errores en tiempo de construcción / antes de hacer pushing, pero también es posible que quiera integrarlo con su IDE para recibir una retroalimentación inmediata. NO use el linting ES6 nativo de su IDE. Configurelo que el binario que utiliza para linting sea el de su carpeta `node_modules` en su lugar. De esta manera puede utilizar todos los config de su proyecto, el preset Airbnb, etc. De lo contrario, sólo obtendrá un linting ES6 genérico.

## Flow

> 💡 **[Flow](https://flowtype.org/)**: Es un verificador de Tipo Estático hecho por Facebook. Detecta tipos inconsistentes en su código. Por ejemplo, le dará un error si intenta usar un string donde debería estar usando un number.

Justo ahora, nuestro código JavaScript es código ES6 valido. Flow puede analizar JavaScript plano y darnos algunas ideas, pero para poder usar todo su potencial, necesitamos agregar anotaciones de tipo en nuestro código, lo que lo hará no estándar. Tenemos que enseñar a Babel y ESLint cuáles son las anotaciones de tipo para que estas herramientas no se asusten al analizar nuestros archivos.

- Corre `yarn add --dev flow-bin babel-preset-flow babel-eslint eslint-plugin-flowtype`

`flow-bin` es el binario para correr Flow en nuestras `scripts` de tareas, `babel-preset-flow` es el preset para que Babel entienda las anotaciones de Flow, `babel-eslint` es un paquete que permite a ESLint *confiar en el analizador de Babel* en lugar del suyo y `eslint-plugin-flowtype` es un plugin de ESLint para verificardar las anotaciones de Flow.

- Actualiza tu archivo `.babelrc` con lo siguiente:

```json
{
  "presets": [
    "env",
    "flow"
  ]
}
```

- Y también tu `.eslintrc.json` a:

```json
{
  "extends": [
    "airbnb",
    "plugin:flowtype/recommended"
  ],
  "plugins": [
    "flowtype"
  ],
  "rules": {
    "semi": [2, "never"],
    "no-unexpected-multiline": 2
  }
}
```

**Nota**: el `plugin:flowtype/recommended` contiene las instrucciones para que ESLint use el analizador de Babel. Si quiere ser más explícito, sientase libre de agregar `"parser": "babel-eslint"` en su `.eslintrc.json`.

Sé que esto es mucho que procesar, así que tómese un minuto para pensar en ello. Todavía estoy asombrado de que incluso ESLint pueda usar el analizador de Babel para entender las anotaciones de Flow. Estas 2 herramientas son realmente increíbles por ser tan modulares.

- Agrega `flow` a tu tarea `test`:

```json
"scripts": {
  "start": "babel-node src",
  "test": "eslint src && flow"
},
```

- Crea un archivo `.flowconfig` en la raiz de tu proyecto que contenga:

```flowconfig
[options]
suppress_comment= \\(.\\|\n\\)*\\flow-disable-next-line
```

Esta es una pequeña utilidad que configuraremos para que hacer que Flow ignore cualquier advertencia detectada en la siguiente línea. Lo usarías así, de forma similar a `eslint-disable`:

```js
// flow-disable-next-line
something.flow(doesnt.like).for.instance()
```

Ya con esto deberíamos tener lista la parte de configuración.

- Agrega anotaciones Flow a `src/dog.js` de la siguiente forma:

```js
// @flow

class Dog {
  name: string

  constructor(name: string) {
    this.name = name
  }

  bark() {
    return `Wah wah, I am ${this.name}`
  }
}

export default Dog
```

El comentario `// @flow` le dice a Flow que queremos que este archivo tenga tipado verificado. De resto, las anotaciones de Flow son tipicamente dos puntos despues de un parametro de función o un nombre de función. Visite la [documentación](https://flowtype.org/docs/quick-reference.html) para más detalles.

- Agrega `// @flow` an la parte superior de tu archivo `index.js`.

`yarn test` ahora debería hacer tanto lint com verificación de tipo en su código.

Hay 2 cosas que quiero que intente:

- En `dog.js`, reemplace `constructor(name: string)` por `constructor(name: number)`, y corra `yarn test`. Debería obetner un error de **Flow** diciendo que estos tipos son incompatibles. Esto quire decir que Flow esta configurado correctamente.

- Ahora reemplace `constructor(name: string)` por `constructor(name:string)`, y corre `yarn test`. Debería aparecer un error de **ESLint** diciendo que las anotaciones Flow deberían tener un espacio después de los dos puntos. Lo que significa que el plugin de Flow para ESLint esta configurado correctamente.

🏁 Si ha obtenido estos 2 errores diferentes, usted tha configurado correctamente con Flow y ESLint! Recuerde que debe volver a colocar el espacio que falta en la anotación de Flow.

### Flow en tu editor

Al igual que con ESLint, debe pasar algún tiempo configurando su editor / IDE para conseguir una retroalimentación inmediata cuando Flow detecte problemas en su código.

## Jest

> 💡 **[Jest](https://facebook.github.io/jest/)**: Es una librería de testing de JavaScript hecha por Facebook. Es muy simple configurar y provee todo lo que necesitaría de una librería de testing lista pra usar. Permitiendole también testear Componentes hecho con React.

- Corre `yarn add --dev jest babel-jest` para instalar Jest y el paquete para usarlo con Babel.

- Agregre lo siguiente a su `.eslintrc.json` en la raiz del objeto para permiterle el uso de funciones Jest tener que importarlos en cada archivo de test:

```json
"env": {
  "jest": true
}
```

- Crea un archivo `src/dog.test.js` que contenga:

```js
import Dog from './dog'

test('Dog.bark', () => {
  const testDog = new Dog('Test')
  expect(testDog.bark()).toBe('Wah wah, I am Test')
})
```

- Agrega `jest` a su script de `test`:

```json
"scripts": {
  "start": "babel-node src",
  "test": "eslint src && flow && jest --coverage"
},
```

La bandera `--coverage` hace que Jest genere datos de cobertura de tus pruebas automaticamente. Esto es útil para ver qué partes de su base de código carecen de pruebas. Escribe estos datos en una carpeta `coverage`.

- Agrega `/coverage/` a tu `.gitignore`

🏁 Corre `yarn test`. Después del linting y verificación de tipo, debería correr laos tests de Jest y mostrar una tabla de cobertura. ¡Todo debería estar en verde!.

## Git Hooks con Husky

> 💡 **[Git Hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)**: Son Scripts que se ejecutan cuando se producen ciertas acciones como un commit o un push.

Bien, ahora tenemos esta tarea de `test` que nos dice si nuestro código se ve bien o no. Vamos a configurar Git Hooks para ejecutar automáticamente esta tarea antes de cada `git commit` y` git push`, lo que nos impedirá hacer push de código malo al repositorio si no pasa la tarea `test`.

[Husky](https://github.com/typicode/husky) es un paquete que hace muy fácil configurar Git Hooks.

- Corre `yarn add --dev husky`

Todo lo que tenemos que hacer es crear dos nuevas tareas en `scripts`,` precommit` y `prepush`:

```json
"scripts": {
  "start": "babel-node src",
  "test": "eslint src && flow && jest --coverage",
  "precommit": "yarn test",
  "prepush": "yarn test"
},
```

🏁 Si ahora intenta hacer commit o push de su código, debería automaticamente correr la tarea `test`.

**Nota**: Si está haciendo push justo después de un commit, puede utilizar `git push --no-verify` para evitar ejecutar todas las pruebas de nuevo.

Siguiente sección: [03 - Express, Nodemon, PM2](03-express-nodemon-pm2.md#readme)

Regresar a la [sección anterior](01-node-yarn-package-json.md#readme) o a la [tabla de contenidos](https://github.com/verekia/js-stack-from-scratch#table-of-contents).