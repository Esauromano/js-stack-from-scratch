# 02 - Babel, ES6, ESLint, Flow, Jest, and Husky

El código para este capítulo esta disponible [aquí](https://github.com/verekia/js-stack-walkthrough/tree/master/02-babel-es6-eslint-flow-jest-husky).

Vamos a usar la sintaxis de ES6, la cual aporta una gran mejora en comparación con la sintaxis "antigua" de ES5. No obstante, todos los navegadores y entornos JS comprenden bien ES5, pero no ES6. Es aquí donde una herramienta llamada Babel ¡viene al rescate!

## Babel

> 💡 **[Babel](https://babeljs.io/)**  es un compilador que transforma el código ES6 (y otras cosas como las sintaxis JSX de React) en código ES5. Es muy modular y puede ser usado en miles de [entornos](https://babeljs.io/docs/setup/) diferentes. Es por mucho el compilador ES5 preferido por la comunidad de React.

- Mueve tu `index.js` a una nueva carpeta llamada `src`. Aquí es donde escribiras tu código ES6. Reemplaza el código anterior relacionado a `color` en `index.js` por:

```js
const str = 'ES6'
console.log(`Hello ${str}`)
```

Estamos usando  un *template string* aquí, una nueva característica de ES6 que nos permite inyectar variables dentro de strings sin concatenar, usando `${}`. Note que los template strings son creados usando **backquotes**.

- Ejecuta `yarn add --dev babel-cli` para instalar la interfaz de comandos(CLI) para Babel.

La CLI de Babel incluye [dos ejecutables](https://babeljs.io/docs/usage/cli/): `babel`, el cual compila los archivos ES6 a nuevos archivos ES5, y `babel-node`, el cual puedes usar para reemplazar al binario de `node` y ejecutar archivos ES6 directamente sobre la marcha. `babel-node` es excelente para desarrollo pero es muy pesado y no esta orientado para su uso en producción. En este capítulo vamos a usar `babel-node` para preparar nuestro entorno de desarrrollo y en el siguiente usaremos `babel` para construir los archivos ES5 para producción.

- En el `package.json`, en tu script `start`, reemplaza `node .` por `babel-node src` (`index.js` es el archivo por defecto que Node buscará, por eso podemos omitir `index.js`).

Si ejecutas `yarn start` ahora, debería imprimir la salida correcta, pero Babel no esta haciendo nada actualmente. Eso se debe a que, no le dimos ninguna información sobre cuales transformaciones queremos aplicar. La razón de que haya impreso la salida correcta es porque Node nativamente entiende ES6 sin ayuda de Babel. En cambio algunos navegadores o viejas versiones de Node no podrán ejecutarlo con éxito.

- Ejecuta `yarn add --dev babel-preset-env` para instalar un paquete predefinido de Babel llamado `env`, que posee las configuraciones para las características más recientes de ECMAScript soportadas por Babel.

- Crea un archivo `.babelrc` en la raíz de tu proyecto, el cual es un archivo JSON para tu configuración de Babel. Escribe lo siguiente para hacer que Babel utilice la configuración `env`:

```json
{
  "presets": [
    "env"
  ]
}
```

🏁 `yarn start` debería funcionar todavía, pero está haciendo algo ahora. No podemos saber si realmente es así, ya que estamos usando `babel-node` para interpretar el código ES6 sobre la marcha. Pronto tendrás una prueba de que tu código ES6 se transforma cuando llegas a la sección [Sintaxis Módulos ES6](#sintaxis-modulos-es6) de este capítulo.

## ES6

> 💡 **[ES6](http://es6-features.org/)**: Es la mejora más significativa del lenguaje JavaScript. Hay muchas nuevas características ES6 para listarlas todas aquí, pero el código ES6 tipicamente usa clases con `class`, `const` and `let`, template strings, y funciones flecha (`(text) => { console.log(text) }`).

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

Esto no sería nada nuevo si has tenido la oportunidad de usar Programación Orientada a Objetos(POO) en otro lenguaje. Sin embargo, es algo relativamente nuevo en JavaScript. La clase es expuesta al mundo entero con la asignación a `module.exports`.

En `src/index.js`, escribe lo siguiente:

```js
const Dog = require('./dog')

const toby = new Dog('Toby')

console.log(toby.bark())
```

Como puedes apreciar, a diferencia del paquete hecho por la comunidad `color` que usamos anteriormente, en el caso que requiramos uno de nuestros archivos, usaremos `./` en el `require()`.

🏁 Ejecuta `yarn start` y debería imprimir "Wah wah, I am Toby".

### Sintaxis modulos ES6

Aquí simplemente remplazaremos `const Dog = require('./dog')` por `import Dog from './dog'`, esta es la nueva sintaxis de modules de ES6 (En contraposición a la sintaxis de módulos "CommonJS"). Actualmente, no es soportada de forma nativa por NodeJS, asi que esto prueba que Babel procesa esos archivos ES6 correctamente.

En `dog.js`, también remplazaremos `module.exports = Dog` por `export default Dog`

🏁 `yarn start` todavía debería imprimir "Wah wah, I am Toby".

## ESLint

> 💡 **[ESLint](http://eslint.org)** es un linter para código ES6. Un linter nos ofrece recomendaciones sobre el formato del código, asegurando un estilo consistente en tu código y el código que compartes con tus compañeros. También es una excelente manera de aprender sobre JavaScript al cometer errores que ESLint ira encontrando.

ESLint trabaja con *reglas(rules)*, y hay [muchas de ellas](http://eslint.org/docs/rules/). No obstante, en vez de configurar las reglas que queramos para nuestro código, usaremos la configuración creada por Airbnb. Esta configuración usa algunos plugins que necesitamos instalar para que funcionen correctamente.

Consulte las [instrucciones](https://www.npmjs.com/package/eslint-config-airbnb) más recientes de Airbnb para instalar el paquete de configuración y sus respectivas dependencias correctamente. A partir de 2017-02-03, recomiendan el uso del siguiente comando en su terminal:

```sh
npm info eslint-config-airbnb@latest peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs yarn add --dev eslint-config-airbnb@latest
```

Con esto se debería instalar todo lo necesario y agregar `eslint-config-airbnb`, `eslint-plugin-import`, `eslint-plugin-jsx-a11y`, y `eslint-plugin-react` a su archivo`package.json` automáticamente.

**Nota**: He reemplazado `npm install` por `yarn add` en este comando. Adicionalmente,  esto no funcionará en Windows, por tanto revise el archivo `package.json` de este repositorio e instale todas las dependencias relacionadas con ESLint manualemnte usando `yarn add --dev packagename@^#.#.#` donde `#.#.#` sea la versión encontrada en el `package.json` para cada paquete.

- Crea un archivo `.eslintrc.json` en la raiz de tu proyecto, tal cual como hicimos para Babel, y escribe lo siguiente en el:

```json
{
  "extends": "airbnb"
}
```

Crearemos un script NPM/Yarn para ejecutar ESLint. Instalemos el paquete `eslint` para poder usar la CLI de `eslint`:

- Ejecuta `yarn add --dev eslint`

Actualiza los `scripts` de tu `package.json` para incluir una nueva tarea llamada `test`:

```json
"scripts": {
  "start": "babel-node src",
  "test": "eslint src"
},
```

Esto le dirá a ESLint que queremos revisar todos los archivos JavaScript dentro de la carpeta `src`.

Usaremos la tarea `test` estándar para correr todos los comandos que validarán nuestro código, ya sea para hacer linting, verificar tipado, o pruebas unitarias.

- Ejecuta `yarn test`, y deberías ver un montón de errores por puntos y comas faltantes, y advertencias por utilizar `console.log()` en `index.js`. Agrega `/* eslint-disable no-console */` en la parte superior del archivo `index.js` para permitir el uso de `console` en este archivo.

**Nota**: Si estas en Windows, asegúrate de configurar tu editor y Git para utilizar terminaciones de línea Unix LF y no Windows CRLF. Si tu proyecto es solo utilizado en entornos Windows, puedes agregar `"linebreak-style": [2, "windows"]` en el array de `rules` de ESLint (Vea el ejemplo a continuación) para forzar CRLF en su lugar.

### Punto y Comas

Bien, este es probablemente el debate más acalorado en la comunidad de JavaScript, vamos a hablar de ello por un minuto. JavaScript tiene esta cosa llamada Inserción Automática de Punto y Comas, que te permite escribir código con o sin puntos y comas. Realmente se reduce a la preferencia personal y no hay una opción correcta o incorrecta en este tema. Si te gusta la sintaxis de Python, Ruby o Scala, probablemente disfrutarás de omitir puntos y comas. Si prefieres la sintaxis de Java, C # o PHP, probablemente prefieras usar puntos y comas.

La mayoría de la gente escribe JavaScript con puntos y comas, por costumbre. Ese fue mi caso hasta que intenté probar escribir código sin puntos y comas, después de ver muestras de código de la documentación de Redux. Al principio me sentí un poco extraño, simplemente porque no estaba acostumbrado. Después de sólo un día de escribir código de esta manera no podía verme usando puntos y comas otra vez. Se sentía incómodo e innecesario. Desde mi opinión el código sin puntos y comas es más ameno a la vista y es más rápido de tipear.

Yo recomiendo leer la [documentación de ESLint sobre puntos y comas](http://eslint.org/docs/rules/semi). Como mencione en está página, si decides escribir código sin puntos y comas, hay algunos casos extraños en donde se requerira de su uso. ESLint puede protegerte de esos casos con la regla `no-unexpected-multiline`. Configuraremos ESLint para trabajar de forma segura siguiendo el estándar de escribir código sin puntos y comas, para eso coloquemos en nuestro `.eslintrc.json` lo siguiente:

```json
{
  "extends": "airbnb",
  "rules": {
    "semi": [2, "never"],
    "no-unexpected-multiline": 2
  }
}
```

🏁 Ejecuta `yarn test`, ahora debe pasar con éxito. Intenta agregar un punto y coma innecesario en algún lugar para asegurarse de que la regla esté configurada correctamente.

Soy consciente de que algunos de ustedes querrán seguir utilizando puntos y comas, lo que hará que el código proporcionado en este tutorial sea un inconveniente. Si estás usando este tutorial sólo para aprender, estoy seguro de que seguirá siendo soportable aprender sin punto y coma, hasta volver a usarlos en tus proyectos reales. Si desea utilizar el código proporcionado en este tutorial como una plantilla sin embargo, se requerirá un poco de reescritura, lo que debería ser bastante rápido con ESLint establecido para aplicar puntos y comas para guiarlo a través del proceso. Me disculpo si estás en ese caso.

### Compat

[Compat](https://github.com/amilajack/eslint-plugin-compat) es un plugin de ESLint que te advierte si estas utilizando algunas APIs de Javascript que no están disponibles en los navegadores que quieres soportar. Utiliza [Browserslist](https://github.com/ai/browserslist), que está basado en [Can I Use](http://caniuse.com/).

- Ejecuta `yarn add --dev eslint-plugin-compat`

- Agrega lo siguiente a tu `package.json`, para indicar que queremos dar soporte a navegadores con mas del 1% de la cuota de mercado:

```json
"browserslist": ["> 1%"],
```

- Edita tu archivo `.eslintrc.json`:

```json
{
  "extends": "airbnb",
  "plugins": [
    "compat"
  ],
  "rules": {
    "semi": [2, "never"],
    "no-unexpected-multiline": 2,
    "compat/compat": 2
  }
}
```

Puedes probar el plugin utilizando `navigator.serviceWorker` o con una instancia de `fetch` en tu código, lo que deberá generar una advertencia en ESLint.

### ESLint en tu editor

En este capítulo configurarás ESLint en la terminal, lo cual es una excelente manera de detectar errores en tiempo de construcción / antes de hacer pushing, pero también es posible que quiera integrarlo con su IDE para recibir una retroalimentación inmediata. NO use el linting ES6 nativo de su IDE. Configurelo para que el binario que utilice para linting sea el de su carpeta `node_modules`. De esta manera puede utilizar todos los config de su proyecto, el preset Airbnb, etc. De lo contrario, sólo obtendrá un linting ES6 genérico.

## Flow

> 💡 **[Flow](https://flowtype.org/)**: Es un verificador de Tipo Estático hecho por Facebook. Detecta tipos inconsistentes en tu código. Por ejemplo, te dará un error si intentas usar un string donde debería estar usando un número.

Justo ahora, nuestro código JavaScript es código ES6 válido. Flow puede analizar JavaScript plano y darnos algunas ideas, pero para poder usar todo su potencial, necesitamos agregar anotaciones de tipo en nuestro código, lo que lo hará no estándar. Tenemos que enseñar a Babel y ESLint qué son las anotaciones de tipo para que estas herramientas no se asusten al analizar nuestros archivos.

- Ejecuta `yarn add --dev flow-bin babel-preset-flow babel-eslint eslint-plugin-flowtype`

`flow-bin` es el binario para ejecutar Flow en nuestras `scripts` de tareas, `babel-preset-flow` es el preset para que Babel entienda las anotaciones de Flow, `babel-eslint` es un paquete que permite a ESLint *confiar en el parser de Babel* en lugar del suyo y `eslint-plugin-flowtype` es un plugin de ESLint para verificar las anotaciones de Flow.

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
    "flowtype",
    "compat"
  ],
  "rules": {
    "semi": [2, "never"],
    "no-unexpected-multiline": 2,
    "compat/compat": 2
  }
}
```

**Nota**: el `plugin:flowtype/recommended` contiene las instrucciones para que ESLint use el parser de Babel. Si quieres ser más explícito, sientete libre de agregar `"parser": "babel-eslint"` en su `.eslintrc.json`.

Sé que esto es mucho que procesar, así que tómate un minuto para pensar en ello. Todavía estoy asombrado de que incluso ESLint pueda usar el parser de Babel para entender las anotaciones de Flow. Estas 2 herramientas son realmente increíbles por ser tan modulares.

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

El comentario `// @flow` le dice a Flow que queremos que este archivo tenga verificaión de tipos. Para el resto, las anotaciones de Flow son tipicamente dos puntos después de un parámetro de función o un nombre de función. Visita la [documentación](https://flowtype.org/docs/quick-reference.html) para más detalles.

- Agrega `// @flow` en la parte superior de tu archivo `index.js`.

`yarn test` ahora debería hacer tanto lint como verificación de tipo en su código.

Hay 2 cosas que quiero que intentes:

- En `dog.js`, remplaza `constructor(name: string)` por `constructor(name: number)`, y ejecuta `yarn test`. Deberías obtener un error de **Flow** diciéndote que estos tipos son incompatibles. Esto quire decir que Flow esta configurado correctamente.

- Ahora remplaza `constructor(name: string)` por `constructor(name:string)`, y ejecuta `yarn test`. Debería aparecer un error de **ESLint** diciéndote que las anotaciones Flow deberían tener un espacio después de los dos puntos. Lo que significa que el plugin de Flow para ESLint esta configurado correctamente.

🏁 Si has obtenido estos 2 errores diferentes, has configurado todo correctamente con Flow y ESLint! Recuerda que debes volver a colocar el espacio que falta en la anotación de Flow.

### Flow en tu editor

Al igual que con ESLint, deberás pasar algún tiempo configurando tu editor / IDE para conseguir una retroalimentación inmediata cuando Flow detecte problemas en su código.

## Jest

> 💡 **[Jest](https://facebook.github.io/jest/)**: Es una librería de pruebas(testing) de JavaScript hecha por Facebook. Es muy simple de configurar y provee todo lo que necesitas de una librería de testing lista pra usar. Permitiéndole también testear componentes hechos con React.

- Ejecuta `yarn add --dev jest babel-jest` para instalar Jest y el paquete para usarlo con Babel.

- Agrega lo siguiente a tu `.eslintrc.json` en la raiz del objeto para permitir el uso de funciones de Jest sin tener que importarlos en cada archivo de test:

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

- Agrega `jest` a tu script de `test`:

```json
"scripts": {
  "start": "babel-node src",
  "test": "eslint src && flow && jest --coverage"
},
```

La bandera `--coverage` hace que Jest genere datos de cobertura de tus pruebas automáticamente. Esto es útil para ver qué partes de su base de código carecen de pruebas. Escribe estos datos en un directorio `coverage`.

- Agrega `/coverage/` a tu `.gitignore`

🏁 Ejecuta `yarn test`. Después del linting y la verificación de tipos, debería ejecutar los tests de Jest y mostrar un reporte. ¡Todo debería estar en verde!.

## Git Hooks con Husky

> 💡 **[Git Hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)**: Son Scripts que se ejecutan cuando se producen ciertas acciones como un commit o un push.

Bien, ahora tenemos esta tarea de `test` que nos dice si nuestro código se ve bien o no. Vamos a configurar Git Hooks para ejecutar automáticamente esta tarea antes de cada `git commit` y` git push`, lo que nos impedirá hacer push de código malo al repositorio si no pasa la tarea `test`.

[Husky](https://github.com/typicode/husky) es un paquete que hace muy fácil configurar Git Hooks.

- Ejecuta `yarn add --dev husky`

Todo lo que tenemos que hacer es crear dos nuevas tareas en `scripts`,` precommit` y `prepush`:

```json
"scripts": {
  "start": "babel-node src",
  "test": "eslint src && flow && jest --coverage",
  "precommit": "yarn test",
  "prepush": "yarn test"
},
```

🏁 Si ahora intentas hacer commit o push de tu código, debería automaticamente ejecutar la tarea `test`.

Si esto no funciona, es posible que `yarn add --dev husky` no haya instalado los Git Hooks apropiadamente. Nunca he tenido este problema, pero a algunos les sucede. Si es tu caso, ejecuta `yarn add --dev husky --force`, y si te es posible escribe un comentario para describir tu situación [en este issue](https://github.com/typicode/husky/issues/84).

**Nota**: Si estás haciendo push justo después de un commit, puede utilizar `git push --no-verify` para evitar ejecutar todas las pruebas de nuevo.

Siguiente sección: [03 - Express, Nodemon, PM2](03-express-nodemon-pm2.md#readme)

Regresar a la [sección anterior](01-node-yarn-package-json.md#readme) o a la [tabla de contenidos](https://github.com/JMEspiz/js-stack-from-scratch#table-of-contents).
