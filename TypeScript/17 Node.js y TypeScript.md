# Node.js y TypeScript

> :link: Basado en el [video](https://youtu.be/ZpY5KdGQvwI) de midudev - [Repositorio](https://github.com/midudev/express-typescript)
>
> :link: Correspondencia con [capítulo de Fullstack Open](https://fullstackopen.com/es/part9/escribiendo_la_aplicacion_express).
>
> Trabajaremos en el repositorio `md-node-ts` pero la idea no será mantenerlo a futuro si hay grandes cambios de versiones ya que bastaría con estas notas.



```
mkdir md-node-ts
```

```
npm init -y
```



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



:link: [PackagePhobia](https://packagephobia.com/) es una pagina que nos muestra cuanto pesa cada paquete.



### `ts-node-dev`

Podríamos usar *nodemon* con *ts-node*, pero `ts-node-dev` hace exactamente lo mismo y podemos continuar usándolo. Estará atento a los cambios que hagamos y efectue la compilación de typescript a la vez que ejecuta el proceso node del archivo indicado.

> Si no estuviéramos trabajando en desarrollo podríamos usar `ts-node`

```
npm i ts-node-dev -D
```



## Creación de Scripts

En `package.json` como es habitual colocaremos todos los scripts que vamos a necesitar.



### Script de Compilado

El primer script que queremos configurar es para ejecutar `tsc` (o `tsc --init` si le pasamos ese parámetro de manera apropiada).

```json
"tsc":"tsc",
```

* La primera vez vamos a ejecutar `npm run tsc -- --init` utilizamos el `--` para indicar que el parámetro `--init` queremos que le llegue a `tsc` y no al `npm run`. 

* Las sucesivas veces ejecutaremos `npm run tsc` directamente cada vez que efectuemos cambios.

En este momento nos creará un `tsconfig.json` el significado de cada propiedad del JSON comentado.



### Script de Linteo

```json
"lint": "eslint --ext .ts ."

```

Actualmente, si ejecutamos eslint, también interpretará los archivos en el directorio build producto de la *compilación*. No queremos eso, ya que el código es generado por el compilador. Podemos evitar esto creando un archivo *.eslintignore*.

### Script de Arranque

Además del mencionado, debemos crear un script de arranque.

```json
"start":"node build/index.js",
```



### Script de Desarrollo

```json
"dev":"ts-node-dev src/index.ts"
```



## Analisis `tsconfig.json`

El bootcamp FSOpen recomienda la siguiente configuración:

```json
{
  "compilerOptions": {
    "target": "ES6",
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

Repasemos cada configuración:

* La configuración *target* le dice al compilador qué versión de ECMAScript usar para el JavaScript generado. ES6 es compatible con la mayoría de los navegadores y es una opción buena y bastante segura.

* *outDir* indica dónde debe colocarse el código compilado.

* *module* le dice al compilador que queremos usar los módulos de *commonjs* en el código compilado. Esto significa que podemos usar *require* en lugar de *import*, que no es compatible con versiones anteriores de Node.js, como la versión 10.

* *strict* es en realidad una abreviatura de varias opciones independientes: *noImplicitAny, noImplicitThis, alwaysStrict, strictBindCallApply, strictNullChecks, strictFunctionTypes y strictPropertyInitialization*. Estos guían nuestro estilo de codificación para usar las funciones de TypeScript de manera más estricta. Para nosotros quizás el más importante sea el que ya hemos visto [noImplicitAny](https://www.staging-typescript.org/tsconfig#noImplicitAny). Impide establecer implícitamente el tipo *any*, lo que puede suceder si no escribe los parámetros de una función, por ejemplo. 

* *noUnusedLocals* evita tener variables locales sin usar.

*  *noUnusedParameters* arroja un error si una función tiene parámetros sin usar.

* *noFallthroughCasesInSwitch* asegura que en un *switch case* cada caso termina con una declaración *return* o *break*.

* *esModuleInterop* permite la interoperabilidad entre los módulos commonJS y ES.

  

Como tenemos la configuración `noUnusedParameters` en `true` si tenemos por ejemplo:

```js
app.get('/ping',(req,res)=> {
	res.send('pong')
});
```

Nos indicará que no hemos utilizado `req`, para evitar eso podemos renombrarlo a `_` o `_req` (lo cual es útil cuando estamos ignorando a más de uno). En aquellos endpoints donde queramos usarlo dejamos `req`.



## Primeros Pasos

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

Podemos solucionar el problema haciendo una aserción de tipo. Si afirmamos que el tipo de la variable *diaryData* es *DiaryEntry* con la palabra clave *as*, todo debería funcionar:



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

De acuerdo con la documentación del nodo para los [módulos de archivo](https://nodejs.org/api/modules.html#modules_file_modules), node intentará resolver los módulos en orden de extensiones:

```sh
 ["js", "json", "node"]
```

Además de eso, de forma predeterminada, *ts-node* y *ts-node-dev* amplían la lista de posibles extensiones de módulo de node a:

```json
 ["js", "json", "node", "ts", "tsx"]
```

> **NB**: La validez de los archivos *.js*, *.json* y *.node* como módulos en Typecript depende de la configuración del entorno, incluidas las opciones *tsconfig* como *allowJs* y *resolveJsonModule*.

Considere una estructura de carpeta plana que contiene archivos:

```sh
  ├── myModule.json
  └── myModule.ts
```

En typescript, con la opción *resolveJsonModule* establecida en true, el archivo *myModule.json* se convierte en un módulo de nodo válido. Ahora, imagine un escenario en el que deseamos utilizar el archivo *myModule.ts*:

```js
import myModule from "./myModule";
```

Mirando de cerca el orden de las extensiones de módulo de node:

```sh
 ["js", "json", "node", "ts", "tsx"]
```

Notamos que la extensión de archivo *.json* tiene prioridad sobre *.ts*, por lo que se importará *myModule.json* y no *myModule.ts*.

Para evitar errores, se recomienda que dentro de un directorio plano, cada archivo con una extensión de módulo de node válida tenga un nombre de archivo único.



## Tipos de Utilidades

TypeScript incluye varios tipos de utilidades para facilitar transformaciones de tipos. Encontraremos más información al respecto en la [documentación](https://www.typescriptlang.org/docs/handbook/utility-types.html).

**La premisa siempre es crear la menor cantidad de tipos y reutilizarlos siempre que sea posible**, de esta manera tendremos menos cosas que mantener y además una única fuente de verdad.

 

### Utilidad `Pick`

En ocasiones tenemos un tipo y queremos usarlo para crear un nuevo tipo con algunos de sus campos. Esto puede ser útil por ejemplo cuando el tipo original contiene información sensible y queremos crear uno nuevo sin esos datos confidenciales. 

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

 

Otra forma sería declarar un tipo completamente nuevo para *NonSensitiveDiaryEntry*:

```js
export type NonSensitiveDiaryEntry = Omit<DiaryEntry, 'comment'>;
```



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

En `getNonSensitiveEntries` estamos devolviendo las entradas completas del diario, ¡y no se da ningún error a pesar del typing! Esto sucede porque TypeScript solo verifica si tenemos todos los campos obligatorios o no, pero los campos en exceso no están prohibidos. En nuestro caso esto significa que *no está prohibido* devolver un objeto de tipo `DiaryEntry[]`, pero si intentáramos acceder al campo `comment`, no sería posible porque estaríamos accediendo a un campo que TypeScript desconoce incluso aunque existe.

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

> Notar que usamos el *unary operator* para convertirlo a número, ya que el parámetro recibido es un string.



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

