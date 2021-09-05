# TypeScript
> Basado en la [playlist](https://www.youtube.com/watch?v=2pZmKW9-I_k&list=PL4cUxeGkcC9gUgr39Q_yD6v-bSyMwKPUI&ab_channel=TheNetNinja) de The Net Ninja - [Rrepositorio](https://github.com/iamshaunjp/typescript-tutorial)
> Los archivos de este proyecto están en `md-typescript`

# Introducción
TypeScript es un **lenguaje de programación** creado por Microsoft construido encima de JavaScript y se lo conoce como un **superset de JavaScript** que extiende a ese lenguaje con nuevas características y sintaxis. Cualquier código de JavaScript es código TypeScript, por lo que pueden convivir en un mismo proyecto

Los navegadores por default no entienden TypeScript por lo que tendremos que **compilar** dicho código para convertirlo en JavaScript. 

Una de las características de TypeScript es que nos permite usar **strict types** (a diferencia de JavaScript que usa dynamic types) es decir que añade un sistema de tipos por lo que si determinamos que una variable es de un determinado tipo (por ejemplo `number`) luego esta no podrá ser otro tipo. De esta manera nos obliga a escribir un **código más limpio y menos propenso a errores** y además **se simplifica el proceso de debugging**.

Otra característica es que podremos utilizar **modern features** como *arrow functions*, `let`, `const`, *destructuring* que quizás no son totalmente soportadas de manera directa en los navegadores. Esto es gracias a que el código no default luego será compilado a una versión antigua de JavaScript que sí es soportada por todos los navegadores (de manera similar a lo que realiza Babel).

Por último TypeScript incluye características extra como ser **generics**, **interfaces**, **tuples**, etc.

# Instalación
Por más que trabajemos con TypeScript a la hora de deployar el código deberá ser JavaScript puro por lo que necesitaremos un compilador.

```bash
npm install -g typescript 
```
Otra opción es instalarlo como dependencia de desarrollo con `npm install typescript --save-dev`

# Compilar `.ts`
Al trabajar con TypeScript los archivos deben tener la extensión `.ts` (`tsx` al trabajar con React) , supongamos que creamos el archivo `sandbox.ts`  de modo que para compilarlo y obtener `sandbox.js` debemos ejecutar `tsc sandbox.ts`

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

A partir de ahora para compilar debemos ejecutar simplemente `tsc` y si queremos configurar un **watch** podemos ejecutar `tsc -w`  con lo cual estará atento a los cambios que se realicen en **todos los archivos** de `src` y los compilará automáticamente.

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