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

ESLint works with *rules*, and there are [many of them](http://eslint.org/docs/rules/). Instead of configuring the rules we want for our code ourselves, we will use the config created by Airbnb. This config uses a few plugins, so we need to install those as well.

Check out Airbnb's most recent [instructions](https://www.npmjs.com/package/eslint-config-airbnb) to install the config package and all its dependencies correctly. As of 2017-02-03, they recommend using the following command in your terminal:

```sh
npm info eslint-config-airbnb@latest peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs yarn add --dev eslint-config-airbnb@latest
```

It should install everything you need and add `eslint-config-airbnb`, `eslint-plugin-import`, `eslint-plugin-jsx-a11y`, and `eslint-plugin-react` to your `package.json` file automatically.

**Note**: I've replaced `npm install` by `yarn add` in this command. Also, this won't work on Windows, so take a look at the `package.json` file of this repository and just install all the ESLint-related dependencies manually using `yarn add --dev packagename@^#.#.#` with `#.#.#` being the versions given in `package.json` for each package.

- Create an `.eslintrc.json` file at the root of your project, just like we did for Babel, and write the following to it:

```json
{
  "extends": "airbnb"
}
```

We'll create an NPM/Yarn script to run ESLint. Let's install the `eslint` package to be able to use the `eslint` CLI:

- Run `yarn add --dev eslint`

Update the `scripts` of your `package.json` to include a new `test` task:

```json
"scripts": {
  "start": "babel-node src",
  "test": "eslint src"
},
```

Here we just tell ESLint that want to lint all JavaScript files under the `src` folder.

We will use this standard `test` task to run a chain of all the commands that validate our code, whether it's linting, type checking, or unit testing.

- Run `yarn test`, and you should see a whole bunch of errors for missing semicolons, and a warning for using `console.log()` in `index.js`. Add `/* eslint-disable no-console */` at the top of our `index.js` file to allow the use of `console` in this file.

**Note**: If you're on Windows, make sure you configure your editor and Git to use Unix LF line endings and not Windows CRLF. If your project is only used in Windows environments, you can add `"linebreak-style": [2, "windows"]` in ESLint's `rules` array (see the example below) to enforce CRLF instead.

### Semicolons

Alright, this is probably the most heated debate in the JavaScript community, let's talk about it for a minute. JavaScript has this thing called Automatic Semicolon Insertion, which allows you to write your code with or without semicolons. It really comes down to personal preference and there is no right and wrong on this topic. If you like the syntax of Python, Ruby, or Scala, you will probably enjoy omitting semicolons. If you prefer the syntax of Java, C#, or PHP, you will probably prefer using semicolons.

Most people write JavaScript with semicolons, out of habit. That was my case until I tried going semicolon-less after seeing code samples from the Redux documentation. At first it felt a bit weird, simply because I was not used to it. After just one day of writing code this way I could not see myself going back to using semicolons at all. They felt so cumbersome and unnecessary. A semicolon-less code is easier on the eyes in my opinion, and is faster to type.

I recommend reading the [ESLint documentation about semicolons](http://eslint.org/docs/rules/semi). As mentioned in this page, if you're going semicolon-less, there are some rather rare cases where semicolons are required. ESLint can protect you from such cases with the `no-unexpected-multiline` rule. Let's set up ESLint to safely go semicolon-less in `.eslintrc.json`:

```json
{
  "extends": "airbnb",
  "rules": {
    "semi": [2, "never"],
    "no-unexpected-multiline": 2
  }
}
```

🏁 Run `yarn test`, and it should now pass successfully. Try adding an unnecessary semicolon somewhere to make sure the rule is set up correctly.

I am aware that some of you will want to keep using semicolons, which will make the code provided in this tutorial inconvenient. If you are using this tutorial just for learning, I'm sure it will remain bearable to learn without semicolons, until going back to using them on your real projects. If you want to use the code provided in this tutorial as a boilerplate though, it will require a bit of rewriting, which should be pretty quick with ESLint set to enforce semicolons to guide you through the process. I apologize if you're in such case.

### ESLint in your editor

This chapter set you up with ESLint in the terminal, which is great for catching errors at build time / before pushing, but you also probably want it integrated to your IDE for immediate feedback. Do NOT use your IDE's native ES6 linting. Configure it so the binary it uses for linting is the one in your `node_modules` folder instead. This way it can use all of your project's config, the Airbnb preset, etc. Otherwise you will just get some generic ES6 linting.

## Flow

> 💡 **[Flow](https://flowtype.org/)**: A static type checker by Facebook. It detects inconsistent types in your code. For instance, it will give you an error if you try to use a string where should be using a number.

Right now, our JavaScript code is valid ES6 code. Flow can analyze plain JavaScript to give us some insights, but in order to use its full power, we need to add type annotations in our code, which will make it non-standard. We need to teach Babel and ESLint what those type annotations are in order for these tools to not freak out when parsing our files.

- Run `yarn add --dev flow-bin babel-preset-flow babel-eslint eslint-plugin-flowtype`

`flow-bin` is the binary to run Flow in our `scripts` tasks, `babel-preset-flow` is the preset for Babel to understand Flow annotations, `babel-eslint` is a package to enable ESLint *to rely on Babel's parser* instead of its own, and `eslint-plugin-flowtype` is an ESLint plugin to lint Flow annotations. Phew.

- Update your `.babelrc` file like so:

```json
{
  "presets": [
    "env",
    "flow"
  ]
}
```

- And update `.eslintrc.json` as well:

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

**Note**: The `plugin:flowtype/recommended` contains the instruction for ESLint to use Babel's parser. If you want to be more explicit, feel free to add `"parser": "babel-eslint"` in `.eslintrc.json`.

I know this is a lot to take in, so take a minute to think about it. I'm still amazed that it is even possible for ESLint to use Babel's parser to understand Flow annotations. These 2 tools are really incredible for being so modular.

- Chain `flow` to your `test` task:

```json
"scripts": {
  "start": "babel-node src",
  "test": "eslint src && flow"
},
```

- Create a `.flowconfig` file at the root of your project containing:

```flowconfig
[options]
suppress_comment= \\(.\\|\n\\)*\\flow-disable-next-line
```

This is a little utility that we set up to make Flow ignore any warning detected on the next line. You would use it like this, similarly to `eslint-disable`:

```js
// flow-disable-next-line
something.flow(doesnt.like).for.instance()
```

Alright, we should be all set for the configuration part.

- Add Flow annotations to `src/dog.js` like so:

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

The `// @flow` comment tells Flow that we want this file to be type-checked. For the rest, Flow annotations are typically a colon after a function parameter or a function name. Check out the [documentation](https://flowtype.org/docs/quick-reference.html) for more details.

- Add `// @flow` at the top of `index.js` as well.

`yarn test` should now both lint and type-check your code fine.

There are 2 things that I want you to try:

- In `dog.js`, replace `constructor(name: string)` by `constructor(name: number)`, and run `yarn test`. You should get a **Flow** error telling you that those types are incompatible. That means Flow is set up correctly.

- Now replace `constructor(name: string)` by `constructor(name:string)`, and run `yarn test`. You should get an **ESLint** error telling you that Flow annotations should have a space after the colon. That means the Flow plugin for ESLint is set up correctly.

🏁 If you got the 2 different errors working, you are all set with Flow and ESLint! Remember to put the missing space back in the Flow annotation.

### Flow in your editor

Just like with ESLint, you should spend some time configuring your editor / IDE to give you immediate feedback when Flow detects issues in your code.

## Jest

> 💡 **[Jest](https://facebook.github.io/jest/)**: A JavaScript testing library by Facebook. It is very simple to set up and provides everything you would need from a testing library right out of the box. It can also test React components.

- Run `yarn add --dev jest babel-jest` to install Jest and the package to make it use Babel.

- Add the following to your `.eslintrc.json` at the root of the object to allow the use of Jest's functions without having to import them in every test file:

```json
"env": {
  "jest": true
}
```

- Create a `src/dog.test.js` file containing:

```js
import Dog from './dog'

test('Dog.bark', () => {
  const testDog = new Dog('Test')
  expect(testDog.bark()).toBe('Wah wah, I am Test')
})
```

- Add `jest` to your `test` script:

```json
"scripts": {
  "start": "babel-node src",
  "test": "eslint src && flow && jest --coverage"
},
```

The `--coverage` flag makes Jest generate coverage data for your tests automatically. This is useful to see which parts of your codebase lack testing. It writes this data into a `coverage` folder.

- Add `/coverage/` to your `.gitignore`

🏁 Run `yarn test`. After linting and type checking, it should run Jest tests and show a coverage table. Everything should be green!

## Git Hooks with Husky

> 💡 **[Git Hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)**: Scripts that are run when certain actions like a commit or a push occur.

Okay so we now have this neat `test` task that tells us if our code looks good or not. We're going to set up Git Hooks to automatically run this task before every `git commit` and `git push`, which will prevent us from pushing bad code to the repository if it doesn't pass the `test` task.

[Husky](https://github.com/typicode/husky) is a package that makes this very easy to set up Git Hooks.

- Run `yarn add --dev husky`

All we have to do is to create two new tasks in `scripts`, `precommit` and `prepush`:

```json
"scripts": {
  "start": "babel-node src",
  "test": "eslint src && flow && jest --coverage",
  "precommit": "yarn test",
  "prepush": "yarn test"
},
```

🏁 If you now try to commit or push your code, it should automatically run the `test` task.

**Note**: If you are pushing right after a commit, you can use `git push --no-verify` to avoid running all the tests again.

Next section: [03 - Express, Nodemon, PM2](03-express-nodemon-pm2.md#readme)

Back to the [previous section](01-node-yarn-package-json.md#readme) or the [table of contents](https://github.com/verekia/js-stack-from-scratch#table-of-contents).
