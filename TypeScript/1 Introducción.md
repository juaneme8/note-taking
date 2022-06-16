# TypeScript
> Basado en la [playlist](https://www.youtube.com/watch?v=2pZmKW9-I_k&list=PL4cUxeGkcC9gUgr39Q_yD6v-bSyMwKPUI&ab_channel=TheNetNinja) de The Net Ninja - [Repositorio](https://github.com/iamshaunjp/typescript-tutorial)
> Los archivos de este proyecto están en `md-typescript`

# Introducción
TypeScript es un **lenguaje de programación** creado por Microsoft para el desarrollo de aplicaciones a gran escala construído encima de JavaScript y se lo conoce como un **superset de JavaScript** que extiende a ese lenguaje con nuevas características y sintaxis. Cualquier código de JavaScript es código TypeScript, por lo que pueden convivir en un mismo proyecto.

TypeScript ofrece características como mejores herramientas en tiempo de desarrollo, análisis de código estático, verificación de tipos en tiempo de compilación y documentación a nivel de código.

Una herramienta interesante es el siguiente [TypeScript Playground](https://www.typescriptlang.org/play) donde podremos ejecutar código TS y probar en la medida que vamos aprendiendo.

Los navegadores por default no entienden TypeScript por lo que tendremos que **compilar** dicho código para convertirlo en JavaScript. 

> Tradicionalmente, *compilar* significa que el código se transforma de un formato legible por humanos a un formato legible por máquina. En TypeScript, el código fuente legible por humanos se transforma en otro código fuente legible por humanos, por lo que el término correcto en realidad sería *transpilar*.

* Una de las características de TypeScript es que nos permite usar **strict types** (a diferencia de JavaScript que usa dynamic types) es decir que añade un sistema de tipos por lo que si determinamos que una variable es de un determinado tipo (por ejemplo `number`) luego esta no podrá ser otro tipo. De esta manera nos obliga a escribir un **código más limpio y menos propenso a errores** y además **se simplifica el proceso de debugging**.

* Podremos utilizar **modern features** como *arrow functions*, `let`, `const`, *destructuring* que quizás no son totalmente soportadas de manera directa en los navegadores. Esto es gracias a que el código no default luego será compilado a una versión antigua de JavaScript que sí es soportada por todos los navegadores (de manera similar a lo que realiza Babel).

* Nos permite **analizar el código de manera estática** que no sólo advertirá sobre el uso incorrecto de tipos sino también otros errores como escribir mal el nombre de una variable, etc.
* Tenemos una **ventaja en términos de documentación**, ya que gracias al uso de tipos a partir de la declaración podremos conocer qué argumentos consume y qué datos devuelve.
* Los IDE pueden proporcionar **intellisense más específico**.

* Incluye características extra como ser **generics**, **interfaces**, **tuples**, etc.

# Instalación
Debemos instalar un compilador que lleve a cabo la tarea de compilar el código TypeScript de manera de convertirlo en código JavaScript que será lo que finalmente vamos a deployar.

```bash
npm install -g typescript 
```
> Otra opción es instalarlo como dependencia de desarrollo con `npm install typescript --save-dev`. Es una buena práctica utilizar esta opción ya que así cualquier persona que clone nuestro repositorio podrá tener noción de las dependencias utilizadas.



## Compilar `.ts`
Al trabajar con TypeScript los archivos deben tener la extensión `.ts` (`tsx` al trabajar con React) , supongamos que creamos el archivo `sandbox.ts` , debemos ejecutar `tsc sandbox.ts` para compilarlo y obtendremos `sandbox.js` 

En cambio si quisiéramos que el archivo de salida tuviera otro nombre podemos hacerlo hacerlo con: `tsc sandbox.ts output.js`

Cada vez que hagamos un cambio en el archivo será necesario volver a compilar, para evitar tener que hacer esto manualmente podemos ejecutar `tsc sandbox.ts -w`. De estar manera luego de hacer un cambio bastará con guardar el archivo para que sea recompilado automáticamente.


## `tsconfig.json`
Hasta el momento trabajamos con un archivo de entrada en TypeScript y un archivo de salida en JavaScript. En proyectos reales tendremos muchos archivos de ambos tipos y vamos a querer dividirlos en distintas carpetas de modo que no se mezclen.

Normalmente tendremos una estructura como la siguiente:
```
+-- public
|	+-- index.html
|	+-- sandbox.js
|	+-- style.css
+-- src
|   +-- sandbox.ts
```
Donde **public** es la carpeta que será subida al servidor web mientras que **src** es la carpeta con la cual trabajamos en desarrollo. 

Una vez adoptada esta estructura queremos que cuando se compilen los archivos `.ts` ubicados en **src** los archivos resultantes JavaScript sean ubicados en **public**.

Mediante el comando `tsc --init` inicializamos un archivo de configuración `tsconfig.json`:

1. * Donde dice `"//rootDir:" "./` debemos descomentarlo y poner:
`"rootDir": "./src",`

2. * Donde dice `"//outDir:" "./` debemos descomentarlo y poner:
`"outDir": "./public",`

A partir de ahora para compilar debemos ejecutar simplemente `tsc` y si queremos configurar un **watch** podemos ejecutar `tsc -w`  con lo cual estará atento a los cambios que se realicen en **todos los archivos** de `src` y los compilará automáticamente a `public`.

**NOTA:** Si creamos un archivo fuera de la carpeta `src` también nos lo compilará a la carpeta `public`. Como este comportamiento no es el deseado, debemos agregar la siguiente propiedad `tsconfig.json`
```json
{
	"compilerOptions": {
		//(...)
		//(...)
		//(...)
	},
	"include": ["src"]
}
```



## TypeScript en Producción

El código TypeScript no es ejecutable por sí mismo, pero primero debe compilarse en JavaScript ejecutable. Las anotaciones de tipos, interfaces, alias de tipos y otras construcciones del sistema de tipos se eliminan del código y el resultado es JavaScript puro y listo para ejecutar.

En un entorno de producción,durante el paso de compilación, todo el código TypeScript se compila en JavaScript en una carpeta separada, y luego ejecuta el código desde esa carpeta. En un entorno de desarrollo, a menudo es más útil hacer uso de la compilación en tiempo real y la recarga automática para poder ver los cambios resultantes más rápido.

 [ts-node](https://github.com/TypeStrong/ts-node) de npm . Compila y ejecuta el archivo TypeScript especificado inmediatamente, por lo que no es necesario un paso de compilación por separado.



## Extensión de Visual Code

Utilizaremos la extensión [typescript hero](https://marketplace.visualstudio.com/items?itemName=rbbit.typescript-hero).

Una de las mejores cosas de la compatibilidad con el editor de TypeScript es que no es necesario que ni siquiera ejecutes el código para ver los problemas. El plugin de VSCode es tan eficiente que le informa inmediatamente cuando intenta utilizar un tipo incorrecto.