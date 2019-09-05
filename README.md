# ES6 Modules

## ¿Para qué sirven?

- Los módulos nos permiten exportar e importar diferentes partes de código, entre diferentes archivos, lo cual nos permite modularizar el código
- Un módulo no es más que un archivo (en este caso JS) que importamos/exportamos desde o hacia otro archivo para utilizarlo
- _Definición cheta:_ un módulo es una unidad independiente (autocontenida) de código reutilizable, que podemos exportar para luego importarla y utilizarla en otro archivo (ó módulo)
- _Otra:_ los módulos nos permiten _encapsular_ funcionalidad y exponerla a otros archivos JS, como si se tratase de _librerías_ (biliotecas)

## Motivación (o sea... ¿por qué usarlos?)

- **Para organizar mejor nuestro código.**

![](https://i.ytimg.com/vi/0FHEeG_uq5Y/maxresdefault.jpg)

- Igual, si, **código más organizado => código más legible, simple, conciso, más fácil de razonar => más mantenible** ✨

![](https://media.makeameme.org/created/clean-code-clean.jpg)

### Otros problemas que resuelven los módulos

- Duplicación de código
- Namespacing (evitar ls colisión de nombres de variables, funciones, clases, etc)
- Dependency Tree

## Antes de ES6 Modules

- Cargábamos los diferentes `script` tags a mano en el HTML
  - Si los cargábamos en orden incorrecto (según las dependencias), no funcionaba
  - Genera problemas de performance/usabilidad: hasta que no se termina de cargar un `script`, no se carga el siguiente y así, porque es _render-blocking_
  - _tl;dr_ Hacer esto a mano es bastante molesto, perdemos mucho tiempo y casi siempre _sale mal_ (la palabra técnica para eso es _error-prone_ 😁)
- Antes de _ES6 Modules_, No teníamos una forma nativa (y standard) de cómo usar módulos en JS 
- Más tarde fueron apareciendo diferentes soluciones de terceros (_CommonJS, AMD, UMD, Browserify, etc_) y era medio un complejo por temas de compatibilidad, etc
- Aparecen los _package bundlers/module loaders_ que nos simplifican un poco la vida y hacen el trabajo sucio por nosotros, pero agregamos más herramientas y capas de complejidad a nuestro código

## ¿Qué podemos exportar? (e importar)

- variables/constantes
- objetos
- funciones
- clases

## Soporte

Recíen a mediados del 2018 esta feature comenzó a tener más [soporte](https://caniuse.com/#feat=es6-module), por lo que en algunos browsers posiblemente tengamos que usar herramientas (aka _package bundlers_) como [Webpack](https://webpack.js.org/) o [Parcel](https://parceljs.org/), para compilar nuestros módulos de ES6 a una sintaxis que el browser entienda.

## Uso

- **Named export:** Lo usamos cuando queremos indicar explícitamente qué exportamos y con qué nombre. Necesitamos usar `{}` en el `import` y llamarlo con el mismo nombre con el que fue exportado: Si el nombre no matchea con ningún `export`, va a importar el `default`

```js
export const package = {};

import { package } from './module-name.js';
```

```js
// Podemos exportar múltiples valores
const a = 1;
const b = 2;
const c = 3;

export { a, b, c };

// Y después importar sólo los que vamos a usar (con destructuring!)
import { a, b } from './module-name.js'

// En el caso de ser necesario, podemos renombrar los imports, suando 'as'
import { a as one } from './module-name.js'
```

- **Default export:** Lo usamos generalmente cuando exportamos un único valor. Sólo puede haber un _default export_ por módulo. No necesitamos usar `{}` en el `import` y podemos ponerle el nombre que querramos

```js
export default const package = {};

import package from './module-name.js';
```

- ✨ **Nota:** podemos usar _named_ y _default exports_ en el mismo módulo

- **Wildcard export:** Lo usamos si queremos importar _todo_ lo que otro módulo exporta

```js
const a = 1;
const b = 2;
const c = 3;

export { a, b, c };

// Importamos a, b y c
import * from './module-name.js'
```

- **Absolute path:** También podemos usar _paths absolutos_ para referenciar módulos que se encuentran en otro dominio

```js
import toUpperCase from 'https://git.io/fjjGt'
```

## Usando ES6 modules en el browser

- Tenemos que agregar el atributo `type="module"` a nuestros tags `script` para que el browser los cargue como _ES6 Modules_
- Ojo con el tema de [_CORS_](https://www.youtube.com/watch?v=1maCPA28eCo), necesitamos levantar los archivos en un server. Algunas opciones: [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer), [http-server](https://www.npmjs.com/package/http-server)
- Para browsers sin soporte, podemos definir un `script` con al atributo `nomodule` como fallback

```js
<script type="module" src="module.js"></script>
<script nomodule src="fallback.js"></script>
```

## Node

- Node utiliza _CommonJS_ como sistema de módulos desde hace mucho tiempo

```js
const package = require('module-name');
```

- Gracias a esta feature de ES6, podemos estandarizar y unificar la forma de usarlos. En la versión 12, el [soporte para ES6 modules](https://blog.logrocket.com/es-modules-in-node-js-12-from-experimental-to-release/) mejoró mucho

## Tips

- Poner los _imports_ siempre al inicio del archivo JS
- Si el archivo exporta una sola cosa, usar `export default`
- Los _Named imports_ llevan `{}`, los _default_ no
- Si el _path_ no es absoluto, no olvidarnos de usar `./` o `/` antes del nombre del módulo
- No olvidarnos de ponerle la extensión `.js` al módulo que importamos

```js
// Esto no funciona ❌
import { something } from './moduleName';

// Esto si ✅
import { something } from './moduleName.js';
```

