# SvelteKit

:link: Basado en la [playlist](https://www.youtube.com/watch?v=UOMLvxfrTCA&list=PLC3y8-rFHvwjifDNQYYWI6i06D7PjF0Ua&ab_channel=Codevolution) de Codevolution. **COMPLETO VIDEO 2**

## ¿Qué es Svelte?

Svelte es una herramienta para crear aplicaciones web rápidas y de manera similar a React o Vue simplifica la creación de UIs interactivas.

A diferencia de React o Vue, que realizan la mayor parte de su trabajo en el navegador, Svelte convierte tu aplicación en JavaScript optimizado en tiempo de compilación. En lugar de utilizar un DOM Virtual, actualiza eficientemente el DOM cuando se produce un cambio en el estado.

Svelte es un *component framework* encargado de crear la UI (view layer) por lo que para lograr una aplicación lista para producción tendremos que tomar otras decisiones (routing, server side rendering, authentication) 

## ¿Qué es SvelteKit?

SvelteKit es un *app framework* que utiliza Svelte y cuenta con funcionalidades que nos permiten lograr aplicaciones de alta performance listas para producción. Sin la necesidad de instalar otros paquetes podremos contar con routing, authentication, server side rendering, bundle optimization, etc. Sin embargo, debemos tener presente que tendremos que seguir una serie de convenciones y opiniones para implementar estas funcionalidades.



## Características SvelteKit

1. File based routing. No es necesaria la instalación de una biblioteca para el routing.
2. Pre-rendering. Genera HTML para cada página de antemano en lugar de hacerlo con JavaScript del lado del cliente. Esto se traduce en mejoras de la performance y de SEO.
3. API Routes. Decimos que SvelteKit es un full-stack framework ya que es posible crear APIs que consumimos mediante la UI.
4. Data fetching. Simplifica la obtención de data y compartirla en el árbol de componentes.
5. Optimized production build system. Sin necesidad de configuraciones.
6. De acuerdo al State of JS 2022 tiene el porcentaje más alto de retention ratio (porcentaje de usuarios que tienen pensado volver a usar la biblioteca)



## Getting started

Creamos carpeta `md-sveltekit` y una vez adentro de ella ejecutamos:

```
npm create svelte@latest hello-world
```

Accederemos a un CLI interactivo

◇ Which Svelte app template? 
**Skeleton project**: consiste en el scaffolding básico.

◇ Add type checking with TypeScript?
**No**

◇ Select additional options (use arrow keys/space bar)
**✔ ESLint**
**✔ Prettier

```
cd hello-world
npm i
git init && git add -A && git commit -m "Initial commit"  npm npm run dev -- --open
```

> A la hora de ejecutar `run dev -- --open` con los primeros `--` le estamos diciendo que el argumento queremos que se lo pase al script `dev` (y no al comando `run`). Con la opción `--open` le indicamos a Vite que abra el proyecto en el navegador una vez que el servidor inicie.



# Folder Structure

## `package.json`

Si analizamos las `devDependencies` nso encontramos con lo siguiente:

```
"devDependencies": {
		"@sveltejs/adapter-auto": "^2.0.0",
		"@sveltejs/kit": "^1.5.0",
		"eslint": "^8.28.0",
		"eslint-config-prettier": "^8.5.0",
		"eslint-plugin-svelte3": "^4.0.0",
		"prettier": "^2.8.0",
		"prettier-plugin-svelte": "^2.8.1",
		"svelte": "^3.54.0",
		"vite": "^4.0.0"
	},
```

* `@sveltejs/kit` y `svelte` paquetes principales.

* `vite` es la build tool
*  `eslint` linting 
* `prettier` code formatting.



Si analizamos los scripts nos encontramos con:

```
	"scripts": {
		"dev": "vite dev",
		"build": "vite build",
		"preview": "vite preview",
		"lint": "prettier --plugin-search-dir . --check . && eslint .",
		"format": "prettier --plugin-search-dir . --write ."
	},
```

* Con `npm run dev` iniciamos la aplicación en desarrollo con instant loading.

* Con `npm run build` compila la aplicación para producción.

* Con `npm run preview` inicia la aplicación compilada para producción (es necesario ejecutar build antes)

* Con `npm run lint` lintea todos los archivos.

* Con `npm run format` formatea todos los archivos.



## `package-lock.json`

`package-lock.json` Asegura consistencia de las dependencias instaladas.

## `.npmrc`

El archivo `.npmrc` es un archivo de configuración usado por npm y mediante la opción `engine-strict=true` nos aseguramos la compatibilidad de las dependencias con los `engines` especificados en el `package.json.` Por ejemplo si un paquete especifica `"engines": {"node": ">=12.0.0 <14.0.0"}` en el `package.json` con `engine-strict=true` nos aseguramos que las dependencias del paquete también cumplan este requerimiento. Se considera una buena práctica ya que puede evitar potenciales issues en el futuro.



## `svelte.config.js`

`svelte.config.js` es el archivo de configuración de Svelte.

Exporta un objeto `config` que será usado por otras herramientas que integremos con Svelte. Inicialmente tenemos solo una configuración para `adapter` que son pequeños plugins que toman la aplicación buildeada como entrada y generan una salida para deployment.



## `vite.config.js`

`vite.config.js` es el archivo de configuración de vite



## `.eslintrc.cjs`

`.eslintrc.cjs` es el archivo de configuración de ESLint

## `.estlintignore`

`.estlintignore` es el archivo de configuración donde indicamos los archivos en los que queremos ignorar el linteo.

## `.prettierr`

`.prettierr` es el archivo de configuración de Prettier

## `.prettierignore`

`.prettierignore` es el archivo de configuración donde indicamos los archivos en los que queremos ignorar el formateo.



## Carpetas

`.svelte-kit`

Esta carpeta es generada automáticamente cuando ejecutamos dev o build scripts.



https://youtu.be/iqm7Sv9VykI?t=310|