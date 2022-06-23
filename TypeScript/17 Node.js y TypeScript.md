# Node.js y TypeScript

> :link: Basado en el [video](https://youtu.be/ZpY5KdGQvwI) de midudev - [Repositorio](https://github.com/midudev/express-typescript)
>
> :link: Correspondencia con [capítulo de Fullstack Open](https://fullstackopen.com/es/part9/escribiendo_la_aplicacion_express).
>
> Trabajaremos en el repositorio `md-node-ts` pero la idea no será mantenerlo a futuro si hay grandes cambios de versiones ya que bastaría con estas notas.



```bash
mkdir md-node-ts
cd md-node-ts
npm init -y
```





## Comentario Inicial

A la hora de trabajar con TypeScript debemos tener presente que funciona de manera estática, por lo que si en tiempo de ejecución le pasamos un tipo distinto al esperado no será capaz de detectarlo. Esto significa que tendremos que realizar las validaciones manualmente. 



## Estructura del Proyecto

Creamos una carpeta `src` en la cual creamos el código fuente comenzando por `index.ts`, luego el código compilado estará en la carpeta `build` donde tendremos en este caso `index.js`.



## Instalación Dependencias

## Linter

Dado que este es un proyecto real, que está destinado a crecer con el tiempo, usaremos eslint desde el principio:



### Opción 1

```sh
npm i -D eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

Creamos *.eslintrc* con el siguiente contenido:

```json
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking"
  ],
  "plugins": ["@typescript-eslint"],
  "env": {
    "browser": true,
    "es6": true
  },
  "rules": {
    "@typescript-eslint/semi": ["error"],
    "@typescript-eslint/explicit-function-return-type": 0,
    "@typescript-eslint/no-unused-vars": [
        "error", { "argsIgnorePattern": "^_" }
    ],
     "@typescript-eslint/no-explicit-any": 1,
    "no-case-declarations": 0
  },
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "./tsconfig.json"
  }
}
```

En `package.json` agregamos un script `"lint":"eslint --ext .ts ."`

### Opción 2

Si queremos instalar una única dependencia podemos usar `ts-standard` que es una guía de estilos de TypeScript basada en StandardJS.

```
npm i ts-standard -D
```

* En `package.json` agregamos un script `"lint":"ts-standard"`

* En `package.json` agregamos:

```
"eslintConfig": {
	"parserOptions": {
    	"project": "./tsconfig.json"
  	},
  	"extends":["./node_modules/ts-standard/eslintrc.json"]
}
```

> :warning: Es probable que necesitemos reiniciar el editor para visualizar los errores en la parte inferior.

### `typescript`

Instalamos TypeScript como dependencia de desarrollo.

```
npm i typescript -D
```



### `express`

```
npm i express -E
```

> Notar que con `-E` estamos utilizando la versión exacta dado que queremos ir actualizando las dependencias de manera manual.



### `@types/express`

Al comenzar a utilizar usar Express y hacer una ruta nos dirá **parameter `req` implicitly has 'any' type.**.

Hay paquetes que vienen con los tipos incluidos y otros que no, en esos casos podemos usar la web :link: [DefinitilyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) para buscar cuáles debemos utilizar.

En este caso tenemos que usar `@types/express` y queremos que sean dependencias de desarrollo. 

```
npm i @types/express -D
```

:rotating_light: **Importante:** Si a la hora de utilizar express usamos `require` veremos que el compilador interpreta todo lo relacionado con express para que sea del tipo *any*. Mientras que si usamos `import` el editor conoce los tipos reales. **La declaración de importación que se utilizará depende del método de exportación utilizado en el paquete importado**.  :eyeglasses: Una buena regla general es intentar importar un módulo utilizando primero la declaración *import*. Si *import* no funciona, pruebe con un método combinado: *import ... = require('...')*.



:link: [PackagePhobia](https://packagephobia.com/) es una pagina que nos muestra cuanto pesa cada paquete.



### `ts-node-dev`

Podríamos usar *nodemon* con *ts-node*, pero `ts-node-dev` hace exactamente lo mismo que ellos. Estará atento a los cambios que hagamos y efectue la compilación de typescript a la vez que ejecuta el proceso node del archivo indicado.

> Si no estuviéramos trabajando en desarrollo podríamos usar `ts-node`

```
npm i ts-node-dev -D
```



> Cuando usamos solamente ts-node se encarga de hacer una compilación en tiempo real y ejecutar el código inmediatamente.



## Análisis `tsconfig.json`

El archivo [tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) contiene todas sus configuraciones principales sobre cómo desea que TypeScript funcione en su proyecto. Puede definir qué tan estrictamente desea que se inspeccione el código, qué archivos incluir y excluir (*node_modules* está excluido de forma predeterminada) y dónde se deben colocar los archivos compilados, etc.

> En [esta página](https://www.staging-typescript.org/tsconfig) encontramos más información sobre cada una de las configuraciones.

El Bootcamp FullStack Open recomienda la siguiente configuración:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "outDir": "./build/",
    "module": "commonjs",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "esModuleInterop": true
  }
}
```

> En `"rootDir"` no colocamos src y lo dejamos comentado tal cual está dado que en algunos casos vamos a querer acceder a archivos también por fuera de esa carpeta, por ejemplo `package.json`. 

Repasemos cada configuración:

* La configuración *target* le dice al compilador qué versión de ECMAScript usar para el JavaScript generado. ES6 es compatible con la mayoría de los navegadores y es una opción buena y bastante segura.

* *outDir* indica dónde debe colocarse el código compilado.

* *module* le dice al compilador que queremos usar los módulos de *commonjs* en el código compilado. Esto significa que podemos usar *require* en lugar de *import*, que no es compatible con versiones anteriores de Node.js, como la versión 10.

* *strict* es en realidad una abreviatura de varias opciones independientes: *noImplicitAny, noImplicitThis, alwaysStrict, strictBindCallApply, strictNullChecks, strictFunctionTypes y strictPropertyInitialization*. Estos guían nuestro estilo de codificación para usar las funciones de TypeScript de manera más estricta. 

  De esta lista el más importante es [noImplicitAny](https://www.staging-typescript.org/tsconfig#noImplicitAny). Impide establecer implícitamente el tipo *any* es decir que todas las variables deben tener un tipo definido. En aquellos casos aislados donde en serio no sepamos el tipo de variable tendremos que indicarlo explícitamente en el código.

* *noUnusedLocals* evita tener variables locales sin usar.

* *noUnusedParameters* arroja un error si una función tiene parámetros sin usar.

* *noFallthroughCasesInSwitch* asegura que en un *switch case* cada caso termina con una declaración *return* o *break*.

* *esModuleInterop* permite la interoperabilidad entre los módulos commonJS y ES.

  

Como tenemos la configuración `noUnusedParameters` en `true` si tenemos por ejemplo:

```js
app.get('/ping',(req,res)=> {
	res.send('pong')
});
```

Nos indicará que no hemos utilizado `req`, para evitar eso podemos renombrarlo a `_` o `_req` (lo cual es útil cuando estamos ignorando a más de uno). En aquellos endpoints donde queramos usarlo dejamos `req`.



## eslint

Cuando utilizamos Express veremos que a pesar de tener configurado *noImplicitAny* en `tsconfig.json` tenemos elementos con el tipo implicito *any* y no obtenemos quejas del compilador. La razón para que esto suceda es que `req.query`  y `req.body`son explícitamente tipados como any.



Si quisiéramos evitar que los desarrolladores utilicen *any* podemos usar *eslint* para administrar nuestro código. Instalemos eslint y sus extensiones de typescript:

> ```sh
> npm install --save-dev eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser
> ```

Configuraremos eslint para [no permitir any explicito](https://github.com/typescript-eslint/typescript-eslint/blob/master/packages/eslint-plugin/docs/rules/no-explicit-any.md). Escriba las siguientes reglas en *.eslintrc*:

```json
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 11,
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
  "rules": {
    "@typescript-eslint/no-explicit-any": 2
  }
}
```



[@typescript-eslint](https://github.com/typescript-eslint/typescript-eslint) tiene un montón de reglas específicas eslint de TypeScript, pero también se puede utilizar todas las reglas básicas eslint en proyectos de TypeScript. Por ahora, probablemente deberíamos ir con la **configuración recomendada** y modificar las reglas a medida que avanzamos cada vez que encontramos algo que queremos que se comporte de manera diferente.

Entonces usaremos el siguiente `\.eslintrc`

```
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking"
  ],
  "plugins": ["@typescript-eslint"],
  "env": {
    "node": true,
    "es6": true
  },
  "rules": {
    "@typescript-eslint/semi": ["error"],
    "@typescript-eslint/no-explicit-any": 2,
    "@typescript-eslint/explicit-function-return-type": 0,
    "@typescript-eslint/no-unused-vars": ["error", { "argsIgnorePattern": "^_" }],
    "no-case-declarations": 0
  },
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "./tsconfig.json"
  }
}
```



En situacioens puede que nos resulte beneficioso utilizar el tipo *any explícito* cuando manejemos por ejemplo datos en el cuerpo de una solicitud HTTP POST. Nuestra configuración de eslint evita esto, pero puede desarmar esta regla para una línea en particular insertando el siguiente comentario como la línea anterior:

```js
// eslint-disable-next-line @typescript-eslint/no-explicit-any
```



## Creación de Scripts

En `package.json` como es habitual colocaremos todos los scripts que vamos a necesitar.



### Script de Compilado

Utilizaremos el compilador oficial de TypeScript que viene con el paquete npm de *typescript*. El compilador oficial genera y empaqueta archivos JavaScript a partir de los archivos .ts para que la *versión de producción* construida ya no contenga ningún código TypeScript. Este es el resultado exacto al que aspiramos, ya que TypeScript en sí no es ejecutable por navegadores o Node.



El primer script que queremos configurar es para ejecutar `tsc` (o `tsc --init` si le pasamos ese parámetro de manera apropiada).

```json
"tsc":"tsc",
```

* La primera vez vamos a ejecutar `npm run tsc -- --init` utilizamos el `--` para indicar que el parámetro `--init` queremos que le llegue a `tsc` y no al `npm run`. En este momento nos creará un `tsconfig.json` con un comentario sobre cada propiedad del JSON.

  

* Las sucesivas veces ejecutaremos `npm run tsc` directamente cada vez que efectuemos cambios.

> Notar que a partir de ese momento aparecerá el código en la carpeta de salida (por ejemplo `build`) cosa que no sucede cuando trabajabamos en desarrollo con `ts-node-dev`.

### Script de Linteo

```json
"lint": "eslint --ext .ts ."
```

Actualmente, si ejecutamos eslint, también interpretará los archivos en el directorio `build` producto de la *compilación*. No queremos eso, ya que el código es generado por el compilador. Podemos evitar esto creando un archivo `.eslintignore`.



### Script de Desarrollo

```json
"dev":"ts-node-dev src/index.ts"
```



### Script de Arranque

Además del mencionado, debemos crear un script de arranque.

```json
"start":"node build/index.js",
```



## Primeros Pasos

En `src` creamos un archivo `index.ts` con el siguiente contenido:

```js
import express from 'express';

const app = express();

app.use(express.json());

const PORT = 3000;

app.get('/ping', (_req, res) => {
  res.send('pong');
});

app.listen(PORT, () => {
  console.log(`Server is listening on port ${PORT}`);
});

```



## Creación de Router

Colocaremos todos los routers, módulos que se encargan de manejar un conjunto de recursos específicos como *diaries*, bajo el directorio *src/routes*.



En este caso será un diario de viaje y serán accesibles en `/api/diaries` , por ello en `src` creamos la carpeta `routes` y dentro un archivo `diaries.ts`. 



```js
import express from 'express'

const router = express.Router();

router.get('/', (_req, res) => {
  res.send('Fetching all diaries');
})

export default router;
```



Luego en `index.ts`

```js
app.use('/api/diaries', diaryRouter)
```

Podemos usar **ES modules** y luego será compilado a **CommonJS**.



## Tipos

Como regla general debemos tratar de hacer la menor cantidad de tipos y reutilizarlos lo más que sea posible.

Creamos un archivo en  en `src` llamado `types.ts` donde definimos todos los tipos. 

```tsx
export type Weather = 'sunny' | 'rainy' | 'cloudy' | 'windy' | 'stormy'

export type Visibility = 'great' | 'good' | 'ok' | 'poor';

export interface DiaryEntry {
  id: number,
  date: string,
  weather: Weather,
  visibility: Visibility,
  comment: string,
}
```



## Datos

Creamos un directorio `data` y en el un archivo `diaries.json` con un array de objetos estáticos pero en el futuro podrán ser  obtenidos a partir de un fetch a una API.



## Servicios

No escribiremos el código para las manipulaciones de datos reales en el router. En su lugar, crearemos un *service* que se encargue de la manipulación de datos. Es una práctica bastante común separar la "lógica empresarial o de negocios" del código del router en módulos denominados *service*. 

Entonces para aislar la logica de las rutas creamos una carpeta `services`:

* Creamos un directorio `services` y en el un archivo `diaryService.ts` luego allí importamos el JSON y exportamos los métodos necesarios para hacer un GET y a futuro un POST.

> Para poder importar un JSON debemos agregar la propiedad`"resolveJsonModule": true` en `tsconfig.json`

* En el router podemos importar los métodos uno a uno con `import {getEntries} from '../services/diaryServices'` o importarlos a todos `import * as diaryServices from '../services/diaryServices'`. Esto último sería similar a si exportáramos con `export default {getEntries, addEntry}` que importaríamos con `import diaryServices from '../services/diaryServices'`

## :warning:FAQ

:warning: Por alguna razón, VSCode tiende a quejarse de que no puede encontrar el archivo *../../data/diaries.json* del servicio a pesar de que el archivo existe. Eso es un error en el editor y desaparece cuando se reinicia el editor.

:warning: Con `"typescript:"^4.7.2""` obtengo un error **Error: Debug Failure. False expression: Non-string value passed to `ts.resolveTypeReferenceDirective`** que haciendo el downgrade a `"typescript": "4.6.4"` desaparece.





Si luego en el servicio indicamos que los datos serán de tipo `DiaryEntry[]`:

```tsx
import diaryData from '../../data/diaries.json'
import { DiaryEntry } from '../../types';

const diaries: Array<DiaryEntry> = diaryData;
```

Obtendremos un error porque TypeScript ha inferido que `weather` es un string.

Podemos solucionar el problema haciendo una aserción de tipo. Si afirmamos que el tipo de la variable `diaryData` es `Array<DiaryEntry>` con la palabra clave `as`, todo debería funcionar:



```
const diaries: Array<DiaryEntry> = diaryData as Array<DiaryEntry>;
```

Nunca deberíamos usar la aserción de tipo a menos que no haya otra forma de proceder.



En nuestro caso, podríamos cambiar la forma en que exportamos nuestros datos y dado que no podemos usar typings en un archivo JSON, deberíamos convertir el archivo json en un archivo ts que exporta los datos:

```tsx
import { DiaryEntry } from "../types";

const diaryData:Array<DiaryEntry>= [
  {
      "id": 1,
      "date": "2017-01-01",
      "weather": "rainy",
      "visibility": "poor",
      "comment": "Pretty scary flight, I'm glad I'm alive"
  },
]

export default diaryData;
```



## Módulos de Node y JSON

:warning: Copiado de FullStackOpen: [Enlace](https://fullstackopen.com/es/part9/escribiendo_la_aplicacion_express#modulos-de-node-y-json)

Es importante tomar nota de un problema que puede surgir al usar la opción tsconfig [resolveJsonModule](https://www.typescriptlang.org/en/tsconfig#resolveJsonModule):

```json
{
  "compilerOptions": {
    // ...
    "resolveJsonModule": true  
  }
}
```

Cuando trabajamos con *file modules*, node respetará el siguiente orden a la hora de evaluar los archivos de distintas extensiones:

```sh
 ["js", "json", "node"]
```

En nuestro caso como estamos usando *ts-node* y *ts-node-dev*, estos amplían la lista de posibles extensiones a:

```json
 ["js", "json", "node", "ts", "tsx"]
```

> La validez de los archivos *.js*, *.json* y *.node* como módulos en Typecript depende de la configuración del entorno, incluidas las opciones *tsconfig* como *allowJs* y *resolveJsonModule*.

Considere una estructura de carpeta plana que contiene archivos:

```sh
  ├── myModule.json
  └── myModule.ts
```

En typescript, con la opción *resolveJsonModule* establecida en true, el archivo *myModule.json* se convierte en un válido. Ahora, imagine un escenario en el que deseamos utilizar el archivo *myModule.ts*:

```js
import myModule from "./myModule";
```

Mirando de cerca el orden de las extensiones de módulo de node:

```sh
 ["js", "json", "node", "ts", "tsx"]
```

Notamos que la extensión de archivo *.json* tiene prioridad sobre *.ts*, por lo que se importará *myModule.json* y no *myModule.ts*.

**Para evitar errores, se recomienda que dentro de un directorio plano, cada archivo con una extensión de módulo de node válida tenga un nombre de archivo único.**



## Tipos de Utilidades

TypeScript incluye varios tipos de utilidades para facilitar transformaciones de tipos. Encontraremos más información al respecto en la [documentación](https://www.typescriptlang.org/docs/handbook/utility-types.html).

**La premisa siempre es crear la menor cantidad de tipos y reutilizarlos siempre que sea posible**, de esta manera tendremos menos cosas que mantener y además una única fuente de verdad.

 

### Utilidad `Pick`

En ocasiones tenemos un tipo y queremos usarlo para crear un nuevo tipo con algunos de sus campos. Esto puede ser útil por ejemplo cuando el tipo original contiene información sensible y queremos crear uno nuevo sin esos datos confidenciales o cuando estamos creando un nuevo elemento de una lista y el `id` lo generaremos por otro medio.

Suponemos que tenemos el tipo `DiaryEntry`

```tsx
export interface DiaryEntry {
  id: number
  date: string
  weather: Weather
  visibility: Visibility
  comment: string
}
```

Queremos crear el tipo `NonSensitiveInfoDiaryEntry` igual al anterior pero sin el campo `comment` y para ello utilizamos la utilidad `Pick` indicando los campos del origen que queremos que tenga.

```tsx
export type NonSensitiveDiaryEntry = Pick<DiaryEntry, 'id' | 'date' | 'weather' | 'visibility'>
```



### Utilidad `Omit`

Como queremos excluir solo un campo podemos utilizar el tipo de utilidad `Omit` que podemos usar para declarar qué campos excluir:

```tsx
export type NonSensitiveDiaryEntry = Omit<DiaryEntry, 'comment'>
```



## Implementación

Entonces el código en el servicio nos quedará:

```tsx
import diaries from '../../data/diaries';
import { NonSensitiveDiaryEntry, DiaryEntry } from '../types';

const getEntries = (): DiaryEntry[] => {
  return diaries;
};

const getNonSensitiveEntries = (): NonSensitiveDiaryEntry[] => {
  return diaries;
};

const addEntry = () => {
  return null;
};

export default {
  getEntries,
  addEntry,
  getNonSensitiveEntries
};
```

En `getNonSensitiveEntries` estamos devolviendo las entradas completas del diario, ¡y no se da ningún error a pesar del typing! Esto sucede porque **TypeScript solo verifica si tenemos todos los campos obligatorios o no, pero los campos en exceso no están prohibidos**. En nuestro caso esto significa que *no está prohibido* devolver un objeto de tipo `DiaryEntry[]`, pero si intentáramos acceder al campo `comment`, no sería posible porque estaríamos accediendo a un campo que TypeScript desconoce incluso aunque existe.

Debido a que TypeScript no modifica los datos reales, sino solo su tipo, debemos excluir los campos nosotros mismos:

```tsx
const getNonSensitiveEntries = (): NonSensitiveDiaryEntry [] => {
  return diaries.map(({ id, date, weather, visibility }) => ({
    id,
    date,
    weather,
    visibility,
  }));
};
```



Suponiendo que tenemos un método `findById` que recibe un id procedente de un parámetro del endpoint `diaryService.findById(+req.params.id)` y retorna el elemento con ese `id`.

:star2: Notar que usamos el *unary operator* para convertirlo a número, ya que el parámetro recibido es un string.



```tsx
export const findById = (id: number): NonSensitiveInfoDiaryEntry | undefined => {
  const entry = diaries.find(d => d.id === id)

  return entry
}
```

Como estamos usando `filter()` y en caso de no encontrar ningún elemento devolverá `undefined`. Cuando invoquemos este método tendremos que usar chaining operator para referenciar los campos. Es decir tendremos ` const diary = diaryServices.findById(+req.params.id)` y el propio autocomplete nos ayudará a usar chaining operator al referenciar los campos como vemos a continuación:`console.log(diary?.weather)`.

En el tipo de dato devuelto hemos indicado también que es posible que devolvamos `undefined`: `NonSensitiveInfoDiaryEntry | undefined`



Sin embargo, en la implementación anterior estamos devolviendo el campo `coment`, para filtrarlo:

```tsx
export const findById = (id: number): NonSensitiveInfoDiaryEntry | undefined => {
  const entry = diaries.find(d => d.id === id)
  if (entry != null) {
    const { comment, ...restOfDiary } = entry
    return restOfDiary
  }

  return undefined
}
```

:star2: Con `const { comment, ...restOfDiary } = entry` nos aseguramos tener en `restOfDiary` todo menos `comment`.

:star2: El ` return undefined` lo colocamos para evitar el error propio del `noImplicitReturn` que tenemos en la configuración.



## `id` incremental en Array

Si sabemos que los elementos de una array tendrán un id que se irá incrementando por una unidad, antes de pushear un nuevo elemento podemos definir dicho valor de distintas formas.

```
const id = diaries.length + 1;
```

```
const id = Math.max(...diaries.map(d => d.id)) + 1;
```



## Validaciones

Como con TypeScript realizamos verificaciones estáticas, debemos validar igualmente que los tipos de datos recibidos sean los correctos y recién ahí agregar elementos nuevos.

Como sabemos existen librerías como **express-validator** para tal fin pero lo haremos manualmente.

## Utilidades

Creamos un archivo `utils.ts` donde almacenamos las funciones que reutilizaremos.



## Importancia de `enum`

En ocasiones tendremos campos que sólo podrán tomar determinados valores, por ejemplo si recibimos un string y queremos validar que sea de uno de los valores esperados.

Si tenemos definido un tipo de la siguiente forma:

```tsx
export type Weather = 'sunny' | 'rainy' | 'cloudy' | 'windy' | 'stormy'
```

Luego nos veríamos obligados a preguntar si el string es alguno de esos valores, con lo cual estaríamos duplicando código. Como **queremos tener una única fuente de  verdad** no queremos repetir las palabras contenidas en el tipo. En ese caso podemos recurrir a los `enum`.

```tsx
export enum Weather = {
	Sunny = 'sunny',
	Rainy = 'rainy',
	Cloudy = 'cloudy',
    Windy = 'windy',
    Stormy = 'stormy',
} 
```

>  Notar las diferencias que un enum tiene inicialmente comparado con un objeto: `=` en lugar de `:` al definir las propiedades.

La diferencia radica en que esta estructura de datos la tendremos disponible en run-time, cuando se compile el código de TypeScript podremos acceder a un **objeto**con esta misma información.

```tsx
const isWeather = (string: any):boolean => {
	return Object.values(weather).includes(string)
}
```

Recordemos que `Object.values()` retorna un array con los valores de las propiedades, por lo que tendremos: `[ 'sunny', 'rainy', 'cloudy', 'windy', 'stormy' ]`

:warning: Como este código se convierte a JavaScript no podemos llamar al archivo`types.d.ts` porque no son sólo definiciones,por lo que obtendríamos errores. En ese caso podríamos separar en dos archivos por un lado las definiciones en`types.d.ts` y por otro lado los enums en `enums.ts`



Las validaciones completas nos quedarán como vemos a continuación.

```tsx
import { NewDiaryEntry } from './types'
import { Weather, Visibility } from './enums'

const parseComment = (commentFromRequest: any): string => {
  if (!isString(commentFromRequest)) {
    throw new Error('Incorrect or missing comment')
  }

  return commentFromRequest
}

const parseDate = (dateFromRequest: any): string => {
  if (!isString(dateFromRequest) || !isDate(dateFromRequest)) {
    throw new Error('Incorrect or missing date')
  }

  return dateFromRequest
}

const parseWeather = (weatherFromRequest: any): Weather => {
  if (!isString(weatherFromRequest) || !isWeather(weatherFromRequest)) {
    throw new Error('Incorrect or missing Weather')
  }

  return weatherFromRequest
}

const parseVisibility = (visibilityFromRequest: any): Visibility => {
  if (!isString(visibilityFromRequest) || !isVisibility(visibilityFromRequest)) {
    throw new Error('Incorrect or missing Visibility')
  }

  return visibilityFromRequest
}

const isWeather = (param: any): boolean => {
  return Object.values(Weather).includes(param)
}

const isString = (string: string): boolean => {
  return typeof string === 'string'
}

const isDate = (date: string): boolean => {
  return Boolean(Date.parse(date))
}

const isVisibility = (param: any): boolean => {
  return Object.values(Visibility).includes(param)
}

const toNewDiaryEntry = (object: any): NewDiaryEntry => {
  const newEntry: NewDiaryEntry = {
    comment: parseComment(object.comment),
    date: parseDate(object.date),
    weather: parseWeather(object.weather),
    visibility: parseVisibility(object.visibility)
  }

  return newEntry
}

export default toNewDiaryEntry
```

A la hora de hacer un post en el router, llamaremos a `toNewDiaryEntry` que impulsará todas esas verificaciones, como lo hacemos dentro de un `try` los errores que levantemos en la validación serán devueltos al usuario junto con un status code de 400.

```tsx
router.post('/', (req, res) => {
  try {
    const newDiaryEntry = toNewDiaryEntry(req.body)

    const addedDiaryEntry = diaryServices.addDiary(newDiaryEntry)

    res.json(addedDiaryEntry)
  } catch (e) {
    res.status(400).send(e.message)
  }
})
```



## Validación de Campos

En ocasiones recibimos un parámetro por parte del usuario por ejemplo en un POST por lo que inicialmente será de tipo `any` y queremos chequear que sea del tipo esperado.

Por ejemplo para validar el campo *comment*, debemos verificar que exista y asegurarnos de que sea del tipo *string*.

La función debería verse así:

```js
const parseComment = (comment: any): string => {
  if (!comment || !isString(comment)) {
    throw new Error('Incorrect or missing comment: ' + comment);
  }

  return comment;
}
```

La función obtiene un parámetro de tipo *any* y lo devuelve como tipo *string* si existe y es del tipo correcto.

La función de validación de string se ve así

```js
const isString = (text: any): text is string => {
  return typeof text === 'string' || text instanceof String;
};
```

La función es un [tipo de protección](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates). Eso significa que es una función que devuelve un booleano *y* que tiene un *predicado* de tipo como tipo de retorno. En nuestro caso, el tipo de predicado es

```js
text is string
```

La forma general de un predicado de tipo es *parameterName is Type* donde el *parameterName* es el nombre del parámetro de función y *Type* es el tipo de destino.

:warning: Si la función de protección de tipos devuelve verdadero, el compilador de TypeScript sabe que la variable probada tiene el tipo que se definió en el predicado de tipo.

Antes de que se llame al tipo de protección, el tipo real de la variable *comment* no se conoce:

![fullstack content](https://fullstackopen.com/static/e13ea2afa7dd8df1628a901d42a1b4e2/5a190/28e.png)



Pero después de la llamada, si el código pasa de la excepción (es decir, el tipo guard devuelto verdadero), el compilador sabe que *comment* es del tipo *string*:

![fullstack content](https://fullstackopen.com/static/18cf1ac37d9cee7ac1eb22a58e268045/5a190/29e.png)
