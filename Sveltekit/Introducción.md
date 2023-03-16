# SvelteKit

:link: Basado en la [playlist](https://www.youtube.com/watch?v=UOMLvxfrTCA&list=PLC3y8-rFHvwjifDNQYYWI6i06D7PjF0Ua&ab_channel=Codevolution) de Codevolution. **COMPLETO VIDEO 6**

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



## Archivos

### :scroll:`package.json`

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

### :scroll:`package-lock.json`

`package-lock.json` Asegura consistencia de las dependencias instaladas.

### :scroll:`.npmrc`

El archivo `.npmrc` es un archivo de configuración usado por npm y mediante la opción `engine-strict=true` nos aseguramos la compatibilidad de las dependencias con los `engines` especificados en el `package.json.` Por ejemplo si un paquete especifica `"engines": {"node": ">=12.0.0 <14.0.0"}` en el `package.json` con `engine-strict=true` nos aseguramos que las dependencias del paquete también cumplan este requerimiento. Se considera una buena práctica ya que puede evitar potenciales issues en el futuro.

### :scroll:`svelte.config.js`

`svelte.config.js` es el archivo de configuración de Svelte.

Exporta un objeto `config` que será usado por otras herramientas que integremos con Svelte. Inicialmente tenemos solo una configuración para `adapter` que son pequeños plugins que toman la aplicación buildeada como entrada y generan una salida para deployment.

### :scroll:`vite.config.js`

`vite.config.js` es el archivo de configuración de vite

### :scroll:`.eslintrc.cjs`

`.eslintrc.cjs` es el archivo de configuración de ESLint

### :scroll:`.estlintignore`

`.estlintignore` es el archivo de configuración donde indicamos los archivos en los que queremos ignorar el linteo.

### :scroll:`.prettierc`

`.prettierr` es el archivo de configuración de Prettier

### :scroll:`.prettierignore`

`.prettierignore` es el archivo de configuración donde indicamos los archivos en los que queremos ignorar el formateo.



## Carpetas

### :file_folder: `.svelte-kit`

Esta carpeta es generada automáticamente cuando ejecutamos dev o build scripts. Esta 



### :file_folder: `static`

Los archivos de esta carpeta como su nombre lo indican son servidos estáticamente como `favicon.png` o `robots.txt`



### :file_folder: `src`

La carpeta `src` contiene `app.html` y una carpeta `routes` que será responsable del ruteo de la aplicación. Inicialmente de `routes` tenemos  `+page.svelte` que será servido cuando visitemos localhost:5173.

En `app.svelte` se conoce como el page template y nos encontramos con tres placeholders:

* `%sveltekit.assets%` representando a los archivos estáticos
* `%sveltekit.head%` representando a los elementos de tipo link y scripts requeridos por la página.
* `%sveltekit.body%` representando al markup para la página a renderizar, que será el contenido de `+page.svelte`



## Routing

Svelte se caracteriza por tener un mecanismo de ruteo basado en el file-system. Esto significa que los paths en la URL serán definidos mediante archivos y carpetas.

Esto no significa que cada archivo en nuestra aplicación responda a una ruta sino que es aca donde toman importancia las **convenciones** que mencionamos que hay que seguir en SvelteKit para lograr esta aplicación de alta performance y lista para producción.



### Routing conventions

Para que un archivo esté disponible como ruta debemos seguir las siguientes convenciones.

* Deben estar en una carpeta `routes` dentro de `src`.
* Los path segments de la URL una son determinados por carpetas.
* Los archivos que corresponden a una ruta deben llamarse `+page.svelte`.



Es por eso que para el contenido que queremos mostrar al navegar a localhost:5173 debemos crear una carpeta `routes` y dentro un archivo `+page.svelte`

Para mostrar una página **/about** debemos crear una carpeta `about` y dentro de ella un archivo `+page.svelte`



### 404 Default Page

Si ingresamos una URL que no puede ser mapeada con ningún archivo (non-matching route) dentro de `routes`, como por ejemplo /dashboard veremos un 404 Not Found entregado automáticamente por SvelteKit.



### Nested Routes

Queremos tener la capacidad de trabajar con rutas anidadas, de modo que el usuario pueda ingresar a:

* **/blog**

* **/blog/first**

* **/blog/second**

En ese caso debemos crear el directorio `blog` y dentro un `+page.svelte`, luego un directorio `first` y dentro un `+page.svelte` y por último un directorio `second` y dentro un `+page.svelte`.

```
- routes
  - blog
    - +page.svelte
    - first
      - +page.svelte
    - second
      - +page.svelte
```



### Dynamic Routes

Queremos contar con rutas dinámicas de modo que el usuario pueda ingresar a:

* **/products** donde verá una lista de los productos
* **/products/1** donde encontrará detalles sobre un producto en particular



En `routes` creamos una carpeta `products` y en ella `+page.svelte` para la página donde veremos el listado.

```
<h1>Products List</h1>
<h2>Product 1</h2>
<h2>Product 2</h2>
<h2>Product 3</h2>
```



Con lo visto hasta ahora podríamos pensar en una solución creando una carpeta para cada producto y dentro un archivo `+page.svelte`.

```
- routes
  - products
    - 1
      - +page.svelte
    - 2
      - +page.svelte
    - 3
      - +page.svelte
```



Sin embargo en caso de tener una cantidad grande de productos esto no sería eficiente por lo tanto creamos un segmento dinámico a través de una carpeta `[productId]` y luego creamos un archivo `+page.svelte`:

```
- routes
  - products
    - [productId]
      - +page.svelte
```



Luego en `+page.svelte` tenemos:

```
<h1>Details about product</h1>
```

Este mismo contenido veremos si navegamos a **/products/1**, **/products/99**, etc.

Para extraer el id actual

```vue
<script>
	import { page } from '$app/stores'
	const productId = $page.params.productId;
</script>

<h1>Details about product {productId}</h1>
```

En el readable store `page` tenemos acceso a datos de la página actual como ser los parámetros de la URL y con el signo `$` en `$page.params.productId;` indicamos que se trata de un store.



### Nested Dynamic Routes

En aplicaciones complejas es común tener más de un segmento dinámico en las rutas.

Por ejemplo podríamos desear tener:

* /products/1
* /products/1/review/1

```
- routes
  - products
    - [productId]
      - +page.svelte
      - [reviewId]
        - +page.svelte
```

