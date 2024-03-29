# SvelteKit

:link: Basado en la [playlist](https://www.youtube.com/watch?v=UOMLvxfrTCA&list=PLC3y8-rFHvwjifDNQYYWI6i06D7PjF0Ua&ab_channel=Codevolution) de Codevolution. **COMPLETO VIDEO 43**

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

Accederemos a un CLI interactivo:

Need to install the following packages:
  create-svelte@latest
Ok to proceed? (y)



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

```html
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

```html
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

* /products/1 en este caso veríamos detalles sobre el producto 1
* /products/1/review/1 en este caso veríamos la review 1 sobre el producto 1

```
- routes
  - products
    - [productId]
      - +page.svelte
      - reviews
        - [reviewId]
          - +page.svelte
```



Luego en este archivo: 

```
<script>
	import { page } from $app/stores
	const { productId, reviewId } = $page.params;
</script>

<h1>Review {reviewId} of product {productId}</h1>
```

  

### Catch All Routes

Queremos crear un único archivo que sea capaz de catchear todos segmentos de la URL.  Esto puede ser útil cuando necesitamos manejar URLS desconocidas o dinámicas.

Por ejemplo supongamos que tenemos una página de documentación con cierta cantidad de features y a su vez cada feature con una serie de conceptos que deben ser explicados e incluso pueden tener otras rutas dentro.

* /docs/feature1/concept1/example1
* ...
* /docs/feature**n**/concept**n**/example**n**

Un ejemplor real de esto podría ser **/docs/routing/catch-all-routes**

Como todas estas páginas tendrán un layout similar nos interesará tener un único archivo capaz de catchear todas los route segments de la URL. Tener en cuenta  los distintos segmentos de las URL nos resultan beneficiosos en términos de organización y SEO.

Debemos crear una carpeta `docs` y dentro de ella una carpeta `[...slug]` y `+page.svelte`.

```
- routes
  - docs
    - [...slug]
      - +page.svelte
```

Si bien podemos ponerle el nombre que queramos es una especie de convención usar slug.



El archivo `+page.svelte` matcheará con cualquier URL que contenga /docs en su path. Esto significa que lo veremos al visitar docs/feature1 y al visitar docs/feature1/concept1, etc.

```vue
<script>
  import page from '$app/stores'
  console.log($page.params.slug);
</script>

<h1>Docs home page</h1>
```

Si ingresamos a /docs/feature1/concept1/example1 veremos en pantalla el log:`feature1/concept1/example1` es decir un string que contiene todos los segmentos luego de docs. A partir de este string vamos a generar una array utilizando `.split("/")`.

```vue
<script>
  import page from '$app/stores'
  const slugArr = $page.params.slug;
</script>

{#if slugArr.length===1}
<h1>Viewing docs for feature {slugArr[0]}</h1>
{:else if slugArr.length===1}
<h1>Viewing docs for feature {slugArr[0]} and concept {slugArr[1]}</h1>
{/if}
```

Si visitamos /docs/routing/catch-all-routes veremos Viewing docs for feature routing and concept catch-all.routes.



#### Filter Parameters

Otro caso en el cual es podría ser útil el concepto de catch all routes es a la hora de pasar parámetros de filtro.

Supongamos que en una página de una inmobiliaria queremos listar todas las casa pero a su vez filtrar de acuerdo a nuestro presupuesto.

Suponiendo que este sea entre $100000 y $1000000 podríamos usar la URL: **/houses/100000/1000000**

```
- routes
  - houses
    - [...slug]
      - +page.svelte
```

Luego en `+page.svelte` tendríamos:

```
<script>
  import page from '$app/stores'
  const slugArr = $page.params.slug;
</script>

<h1>List of houses between {slugArr[0]} and {slugArr[1]}</h1>
```



### Optional Parameters

Hasta ahora trabajamos con rutas con parámetros dinámicos, al usarlos el segmento de la url es obligatorio para que matchee con la página.

```
- routes
  - products
    - [productId]
      - +page.svelte
```

Lo que significa que para visualizar el contenido de la página tendremos que visitar /products/1. Sin embargo en ocasiones queremos mostrar el contenido de una página aunque no esté el parámetro. 



Supongamos que queremos tener las siguientes rutas:

* `/` al entrar a la raíz queremos mostrar la **versión por defecto** de la página en inglés.
* `/es`
* `/fr`

```
- routes
  - [[lang]]
      - +page.svelte
```

Notar el uso de corchetes dobles `[[]]` que es lo que determina que no sea un parámetro obligatorio sino opcional.

:rotating_light: En ese momento veremos en pantalla un mensaje:

```
The "/" and the "/[[lang]]" routes conflict with each other
```

Esto se debe a que como `lang` es opcional, tenemos dos rutas que matchean por lo queSvelte no sabe a qué archivo debe mapear.

Una forma es eliminar el `+page.svelte` de la raíz pero lo mejor sería crear una carpeta nueva `marketing` y colocar allí dentro al `[[lang]]` y a `+page.svelte`, teniendo en cuenta que tendremos que ingresar como **/marketing**, **/marketing/es**, etc.



Luego en `+page.svelte`:

```vue
<script>
	import { page } from '$app/stores'

	const greetings = {
		en: 'Hello',
		es: 'Hola',
		fr: 'Bonjour'
	}
	const {lang = 'en'} = $page.params;
	const greeting = greetings[lang];
</script>

<h1>{greeting[lang]}</h1>
```

Notar que con `const {lang = 'en'} = $page.params;` estamos seteando un valor por defecto al hacer destructuring.



### Route Navigation

Queremos que la navigación entre las distintas rutas no sea posible sólo modificando la barra de direcciones del navegador sino que queremos que sea posible acceder mediante un elemento de UI como un link o de manera automática luego de cierta acción.



#### Navegación mediante UI

Implementaremos una navegación de una ruta a otra mediante elementos de UI por parte del usuario.

SvelteKit utiliza anchors `<a>` para navegar entre rutas en lugar de utilizar otro componente específico del framework.



##### **Navegación a rutas estáticas**

Queremos implementar la navegación desde `/` hacia `/blog` y `/products`, por lo tanto en `+page.svelte`

```vue
<a href="/blog">Blog</a>
<a href="/products">Products</a>
```

Luego por ejemplo en `/products/+page.svelte` podremos garantizar la naveción al inicio.

```vue
<a href="/">Home</a>

<h1>Products</h1>

<h2>Product 1</h2>
<h2>Product 2</h2>
<h2>Product 3</h2>
```



##### **Navegación a rutas dinámicas**

La navegación a rutas dinámicas es similar a lo visto:

```vue
<a href="/">Home</a>

<h1>Products</h1>

<h2><a href="/products/1">Product 1</a></h2>
<h2><a href="/products/2">Product 2</a></h2>
<h2><a href="/products/2">Product 3</a></h2>
```



En lugar de utilizar un valor hardcodeado suponemos que recibimos por prop la ruta dinámica:

```vue
<script>
	export let productId = 100;
</script>
<a href="/">Home</a>

<h1>Products</h1>

<h2><a href="/products/1">Product 1</a></h2>
<h2><a href="/products/2">Product 2</a></h2>
<h2><a href="/products/2">Product 3</a></h2>
<h2>
  <a href={`/products/${productId}`}Product {productId}</a></h2>
```



#### Navegación programática

En ocasiones vamos a querer que luego de una determinada acción se produzca una navegación programática a otra ruta. 

Para simular esta situación (en la que por ejemplo luego de una acción que demanda cierto tiempo en completarse queremos redireccionar al usuario a otra página) lo haremos con botón de modo que al hacer click en el será redireccionado a /products.

Como queremos colocar el botón en la página de inicio, editamos el `+page.svelte` del directorio raíz.

```vue
<script>
  import { goto } from '$app/navigation'
  
  const handleClick = () => {
  	console.log('Placing your order');
  	goto('/products');
  }
</script>

<button on:click={handleClick}>Place order</button>
```



##### **Replace History**

Si luego de navegar hacemos click en el botón "back" del navegador iremos a la página anterior que en este caso es la de inicio. Esto se debe a hemos hecho push de la ruta en el stack, pero también es posible reemplazar el historial para eso podemos agregar un objeto como segundo argumento.

```
goto('/products', {replaceState: true})
```

Esto traerá como consecuencia que al presionar el botón back no volvamos a esta página sino a la anteriormente visitada.



##### **Funciones `beforeNavigate` y `afterNavigate`**

El módulo `$app/navigation` exporta las funciones `beforeNavigate` y `afterNavigate` que serán útiles cuando queramos mostrar u ocultar un spinner cuando la navegación está en progreso. Estas funciones aceptan un callback como argumento. Estos callback reciben automáticamente un parámetro `navigation`.

```vue
<script>
  import { goto, beforeNavigate, afterNavigate } from '$app/navigation'
  
  const handleClick = () => {
  	console.log('Placing your order');
  	goto('/products');
  }
  
  beforeNavigate((navigation)=> {
  	console.log({before: navigation});
  })
   ((navigation)=> {
  	console.log({after: navigation});
  })
</script>

<button on:click={handleClick}>Place order</button>
```

Cuando hacemos click en el botón "Place order" veremos en pantalla `{before: {...}}` al analizar el objecto `navigation`:

* `from` objecto donde veremos info sobre /
* `to` objeto donde veremos info sobre /products
* `type: "goto"`
* `cancel` función que podremos llamar si queremos cancelar una navegación.



Si ahora hacemos click en el botón "back" se ejecutará el callback de `afterNavigate` por lo que veremos en consola `{after: {...}}` al analizar el objecto `navigation`:

* `from` objecto donde veremos info sobre /products

* `to` objeto donde veremos info sobre /

* `type: "popstate"`

  Notar que no tenemos una función cancel ya que no podemos cancelar una navegación luego de que ha sucedido.



## Route Matchers

Suponiendo que tenemos una ruta `products` con una ruta dinámica `[productId]` como vemos a continuación:

```
- routes
  - products
    - [productId]
      - +page.svelte
```

Esto nos permite navegar a `/products/1`, `/products/100` pero también matcheará con `/products/batman` cosa no deseada.

Para solucionar esto SvelteKit cuenta con **matchers** que son funciones que aceptarán o rechazarán el parámetro recibido. En nuestro caso vamos a querer retornar `true`sólo si el parámetro recibido es un dígito.

En `src` vamos a crear una carpeta `params` y dentro un archivo `integer.js`.

```jsx
export default match(param){
	return /^\d+$/.test(param);
}
```

> La función debe llamarse `match` ya que esto es una convención de SvelteKit a seguir.

Luego para que esto comience a funcionar debemos renombrar la carpeta `[productsId]` que pasará a ser `[productId=integer]` siendo `integer` el nombre de archivo en la carpeta `params`.

Si ahora ingresamos a `/products/batman` veremos un 404.



## Route Layout

Normalmente a la hora de desarrollar aplicaciones tendremos un layout por defecto que queremos mostrar en todas las páginas como ser un header o un footer.

Hasta ahora al navegar de `/` a `/blog` por ejemplo la actual `+page.svelte` era destruida y una nueva tomaba su lugar.

En caso de querer afectar a todas las páginas de la aplicación, creamos un archivo `+layout.svelte` en `routes`.

```vue
<header>Header</header>
<slot />
<footer>Footer</footer>
```

Con `<slot />` cualquier` página individual que mostremos se renderizará en el lugar de ese tag



#### Layouts Anidados

En ocasiones vamos a querer tener layouts que apliquen solo a ciertas partes de nuestra aplicación. Por ejemplo supongamos que en /products/1 además de la info del producto 1 queremos mostrar otros productos sugeridos.

En ese caso dentro de la carpeta `products` y dentro de `[productId=integer]` podríamos crear un archivo `+layout.svelte` y colocar allí el layout que queremos tener acompañado del tag `<slot/>`. 

```
<slot/>
<h3>Featured products</h3>
```



## Layout Groups

Para explicar este concepto asumiremos que estamos trabajando con rutas relacionadas a la autenticación.

```
- routes
  - register
    - +page.svelte
  - login
    - +page.svelte
```

Supongamos que tenemos la rutas `/register` y `/login` que muestran cierto contenido y se nos solicita que ambas rutas tengan un layout en común como ser un mensaje de bienvenida.

La forma más simple sería crear una carpeta `auth` mover las carpetas `register` y `login` dentro suyo y a su vez crear un `+layout.svelte`:

```
- routes
  - auth
    - register
      - +page.svelte
    - login
      - +page.svelte
    - +layout.svelte
```

En `+layout.svelte` bastaría con tener:

```
<h1>Welcome</h1>
<slot />
```

Sin embargo estamos agregando un nuevo segmento `auth` en las url que serán: `/auth/register` y `/auth/login` y puede que esto no sea deseado, es allí donde es útil el concepto de **layout group**. 

Bastará con cambiarle el nombre a la carpeta `auth` por `(auth)` de manera que las rutas estarán juntas y contaremos con el layout pero sin afectar la url.



## Breaking Out of Layouts

En ocasiones vamos a querer que una ruta tenga un layout distinto.

```
- routes
  - +layout.svelte
  - (auth)
    - +layout.svelte
    - password
      - +layout.svelte
      - info
        - +page.svelte
      - forgot
        - +page.svelte
      - reset
        - +page.svelte
```

En `+layout.svelte` establecemos un layout que se manifestaría en todas las páginas:

```
<p>Password layout</p>
<slot />
```

El comportamiento default hereda layouts de elementos de jerarquía superior, en nuestro caso por el nivel de anidamiento veremos el layout del directorio root, el de auth y el de password.

Supongamos que por algun motivo queremos queremos modificar los layouts que hereda cada ruta.

* En `/forgot` no queremos mostrar el password layout pero sí el auth layout y root layout. En ese caso modificamos el archivo `+page.svelte` y lo llamamos `+page@(auth).svelte`. Esto se interpreta como carpeta `auth` y hacia arriba.

* En `/reset` solo queremos ver el root layout, por eso lo renombramos a `+page@.svelte`



Esta misma sintaxis funciona para archivos `+layout.svelte` por lo que podremos hacer que un layout rompa con la jerarquía de layouts que heredaría.



## Otros archivos

Hasta ahora en la carpeta `routes` trabajamos con archivos llamados `+page.svelte` y `+layout.svelte`. Cuando creamos un archivo con otro nombre no serán tratados como rutas sino que les daremos un tratamiento de componentes.

En el root layout `+layout.svelte` hasta ahora tenemos:

```vue
<header>Header</header>
<slot />
<footer>Footer</footer>
```

Vamos a extraer el contenido en dos componentes para ello creamos al mismo nivel dos archivos: `header.svelte` y `footer.svelte`.

```vue
<script>
	import Header from './header.svelte'
  import Footer from './footer.svelte'
</script>

<Header />
<slot />
<Footer />
```

Sin embargo debemos tener que estos componentes no serán mapeados a ninguna ruta.



### Carpeta `lib`

Cuando tenemos componentes específicos a una ruta, creamos los archivos dentro de su carpeta pero si se trata de componentes o módulos que serán requeridos por múltiples rutas es una buena idea colocarlos en una carpeta `lib` en `src`.



### Path alias

Supongamos que movimos los archivos `header.svelte` y `footer.svelte` a la carpeta `lib`.

Luego a la hora de importarlos podremos utilizar un path alias:

```vue
<script>
	import Header from '$lib/header.svelte'
  import Footer from '$lib/footer.svelte'
</script>

<Header />
<slot />
<Footer />
```

Como vemos gracias a esto se nos simplificará el acceso a los archivos evitando tener múltiples `../` pudiendo utilizar path alias.



## API Routes

Hasta ahora hemos aprendido como rutear páginas, pero en SvelteKit también podremos rutear a endpoints de una API. A diferencia de las rutas de páginas donde respondemos con HTML las API Routes nos dan la posibilidad de tener un control total sobre la respuesta. Vamos a poder realizar operaciones CRUD sobre una DB de manera similar a lo que haríamos en una app con Node.js+Express.js pero sin la necesidad de configurar un servidor aparte.

Estas API Routes también son útiles para hacer peticiones a APIs externas. Un caso ideal sería en caso de tener que consultar una API que requiera una private key, ya que al ser server-side-routes no se envían al navegador.

Creamos en `routes` para tener una mejor organización y separación creamos una carpeta  `api` aunque no es una convención obligatoria. 

En este primer ejemplo queremos obtener la respuesta al acceder a `/api` por ello dentro de ella creamos un archivo que por convención debe llamarse `+server.js`.

```
- routes
  - api
    - +server.js
```



```
export function GET(){
  return new Response("Hello from the demo API")
}
```

El nombre de la función matcheando el HTTP verb es otra convención que debemos seguir.

Utilizamos el objeto `Response` standard de JavaScript.

Si navegamos a `/api` veremos la respuesta en pantalla.



### GET Request

Utilizaremos la extensión **Thunder Client** como herramienta para efectuar los requests a nuestras rutas de la API. En una aplicación real la UI será la encargada de hacer los requests.

Utilizaremos datos en memoria representando lo que en una aplicación real sería la data de una DB, para ello creamos un archivo `comments`.js en la carpeta `lib`. 

```js
export const comments = [
  {
    id: 1, 
    text: "This is the first comment",
  },
  {
    id: 2, 
    text: "This is the second comment",
  },
  {
    id: 3, 
    text: "This is the third comment",
  },
];
```



En `api` creamos una carpeta `comments` y en ella un archivo `+server.js`.

```
- routes
  - api
    - comments
      - +server.js
```



```js
import {comments} from '$lib/comments'

export function GET(){
	return new Response(JSON.stringify(comments), {
    headers: {
      'Content-type': 'application/json'
    }
  });
}
```



Como esto es un poco engorroso y  el restono de un JSON desde una API Route es algo habitual, SvelteKit nos lo simplifica con una función llamada `json` para generar estas respuestas:

```js
import { json } from '@sveltejs/kit'
import {comments} from '$lib/comments'

export function GET(){
	return json(comments);
}
```



Podremos probar esta respuesta en el navegador ingresando a http://localhost:5173/api/comments directamente o podemos hacerlo utilizando las extensiones Thunder Client, HTTP Client o externamente a través de Postman.

Si en ese momento nos aparece un mensaje de error **Connection was refused by the server** tendremos que cambiar `localhost` por `[::1]` por lo que pasará a ser:

```
http://[::1]:5173/api/comments
```



### POST Request

```
- routes
  - api
    - comments
      - +server.js
```

```jsx
import { json } from '@sveltejs/kit'
import {comments} from '$lib/comments'

export function GET(){
	return json(comments);
}

export async function POST(requestEvent){
  const {request} = requestEvent;
  const {text} = await request.json();
  
  const newComment = {
  	id: comments.length+1,
  	text
  }
  return json(newComment, {status: 201});
}
```

* En Thunder Client seleccionamos el verbo `POST` y en la dirección colocamos `http://[::1]:5173/api/comments` y luego en Body: `{"text": "New comment"}`. El id será generado automáticamente en el handler.

* Con `await request.json();` extraemos el JSON Body que es enviado como parte de la petición. Notar que utilizamos la variable `text` al hacer destructuring ya que es una de las propiedades de ese objeto `body`.

* Podríamos haber retornado también un objeto de tipo `Response`

  ```
  new Response(JSON.stringify(newComment),{status: 201})
  ```

  

> Si enviamos una request en Thunder Client sin haber creado el handler correspondiente obtendremos un 405 con Method Not Allowed.



## Dynamic API Routes

Hasta ahora para los `GET` y `POST` trabajamos con `/api/comments` tanto para listar todos los elementos como para agregar uno nuevo. En ocasiones vamos a querer referenciar un elemento en particular ya sea para listarlo, editarlo o eliminarlo. 

En nuestro ejemplo lo haremos con `/api/comments/:commentId` siendo este último un **segmento dinámico**.

Debemos partir de la base de que las dynamic API routes son muy similares a las dynamic page routes.

```
- routes
  - api
    - comments
      - +server.js
      - [commentId]
        - +server.js
```
Queremos retonar un comentario cuyo id matchee con el route parameter.
```jsx
import { json } from '@sveltejs/kit'
import {comments} from '$lib/comments'

export function GET(requestEvent){
	const {params} = requestEvent;
  const {commentId} = params;
  const comment = comments.find(
    comment => comment.id===parseInt(commentId)
  )
  
  return json(comment)
}
```

* Usamos `parseInt()` con el parámetro ya que en el array lo almacenamos como número pero el parámetro lo recibimos como string.

### PATCH Request

En ocasiones vamos a querer realizar modificaciones parciales en un elemento en particular y operamos de la misma manera vista en el caso anterior.

En Thunder Client seleccionamos el verbo `PATCH` y en la dirección colocamos `http://[::1]:5173/api/comments/1` y luego en Body: `{"text": "Updated comment"}`

```jsx
import { json } from '@sveltejs/kit'
import {comments} from '$lib/comments'

export async function PATCH(requestEvent){
  const {params, request} = requestEvent;
  const {commentId} = params;
  const {text} = await request.json();
  
  const comment = comments.find(
    comment => comment.id===parseInt(commentId)
  )
	comment.text = text;
  return json(comment)
}
```

En este caso luego de un patch exitoso retornamos un 200.

### DELETE Request

```jsx
import {json} from '@sveltejs/kit'
import {comments} from '$lib/comments'

export async function DELETE(requestEvent){
	const {params} = requestEvent;
	const {commentId} = params;
	
	const deletedComment = comments.find(
    comment => comment.id===parseInt(commentId)
  );
  const index = comments.findIndex(
  	comment => comment.id===parseInt(commentId)
  );
 	comments.splice(index, 1);
	return json(deletedComment)
}
```

Almacenamos `deletedComment` ya que lo necesitamos para retornarlo. Luego buscamos el índice de dicho elemento en el array y por último utilizamos `comments.splice(index, 1);` que mutará el array original.

En Thunder Client seleccionamos el verbo `DELETE` y en la dirección colocamos `http://[::1]:5173/api/comments/1`.



# Loading Data

En aplicaciones productivas en lugar de utilizar contenido estático hardcodeado como hasta ahora vamos a querer realizar la carga de datos provenientes de fuentes externas (DB o API externa).

SvelteKit aporta varias condiciones que simplifican este proceso.

> En esta sección asumimos que estamos en un nuevo proyecto, es por eso que diremos por ejemplo que creamos una carpeta `products` que anteriormente ya utilizamos.

Suponemos en la página de inicio tenemos un enlace que nos lleva a `/products` donde vemos un listado de productos que son cargados de una fuente externa.

```
- routes
  - +page.svelte
  - products
    - +page.svelte
```



En `/routes/+page.svelte`:

```
<h1>Welcome to the home page</h1>
<a href="/products">Products</a>
```

En `/routes/products/+page.svelte` inicialmente tenemos:

```
<h1>List of products</h1>
```



### Generación de API Externa

Para generar la API externa utilizaremos el paquete JSON Server que nos permite crear REST APIs con *zero-config*.

```
npm install json-server
```

En la raíz creamos un archivo `db.json` con un objeto que tiene una propiedad `products` que es un array de objetos.

```json
{
	"products": [
		{
			"id": 1,
			"title": "Product 1",
			"price": 700,
			"description": "Description 1"
		},
		{
			"id": 2,
			"title": "Product 2",
			"price": 1050,
			"description": "Description 2"
		},
		{
			"id": 3,
			"title": "Product 3",
			"price": 2600,
			"description": "Description 3"
		}
	],
}
```

Luego en `package.json` creamos un nuevo script:

```json
"serve-json": "json-server --watch db.json --port 4000"
```

De esta manera creamos un endpoint de la API sirviendo en localhost:4000 la lista de productos. 

Luego ejecutamos `npm run serve-json` luego en el navegador bastará con visitar `localhost:4000/products` y veremos la lista de elementos. Si visitamos `localhost:4000/products/1` veremos la info de ese elemento en particular.



### Fetching de datos

A continuación queremos agregar el fetching de datos de la API creada recientemente.

Debemos crear un archivo `+page.js` (el nombre es una convención de SvelteKit) en la misma jerarquía del `+page.svelte`, como vemos a continuación. 

```
- routes
  - +page.svelte
  - products
    - +page.svelte
    - +page.js
```

Este archivo debe exportar una función llamada `load`y  esta debe retornar un objeto. Siguiendo esta convención el objeto retornado formará parte de las props del `+page.svelte` ubicado al mismo nivel.



#### Retorno simple de `data`



En `+page.js` tenemos:

```jsx
export async function load() {
	const title = "List of available products"
  return {
  	title
  }
}
```

Luego en `+page.svelte` accedemos a este objeto y lo llamamos `data`.

```vue
<script>
	export let data;
</script>

<h1>{data.title}</h1>
```



#### Fetching real

```jsx
export async function load() {
	const title = "List of available products";
  const response = await fetch('http://localhost:4000/products');
  const products = response.json()
  return {
    title,
  	products
  }
}
```



```vue
<script>
	export let data;
	const products = data.products;
</script>

<h1>{data.title}</h1>
{#each products as product}
<div>
	<h2>{product.title}</h2>
	<p>{product.description></p>
	<hr /> 
</div>
{/each}
```

En la consola veremos un warning como el siguiente: **`Loading /login using window.fetch. For best results, use the fetch that is passed to your load function:` [`https://kit.svelte.dev/docs/load#making-fetch-requests`](https://kit.svelte.dev/docs/load#making-fetch-requests)**



#### `loadEvent` y función `fetch`

Nos indica que no debemos usar la función `fetch` nativa sino que debemos usar la que le pasa a `load` a través de `loadEvent` recibido como argumento.

```jsx
export async function load(loadEvent) {
	const {fetch} = loadEvent;
	const title = "List of available products";
  const response = await fetch('http://localhost:4000/products');
  const products = response.json()
  return {
    title,
  	products
  }
}
```

En este caso no hay ninguna ventaja de usar esta función `fetch` de SK frente a la nativa, pero veremos a continuación casos en los que sí obtenemos mejoras.

#### Diferencias funciones `fetch`

A continuación vamos a analizar las diferencias existentes entre la función `fetch` nativa y esta que estamos utilizando y así entender qué fue lo que motivó el warning.

Agregamos a `db.json` la propiedad `postcodes` que servirá un array de tres elementos.

```json
{
	"products": [
		{
			"id": 1,
			"title": "Product 1",
			"price": 700,
			"description": "Description 1"
		},
		{
			"id": 2,
			"title": "Product 2",
			"price": 1050,
			"description": "Description 2"
		},
		{
			"id": 3,
			"title": "Product 3",
			"price": 2600,
			"description": "Description 3"
		}
	],
	"postcodes": [
		{
    	"postcode": "E3 4NA",
    	"post_town": "London",
      "building_name": "Wellington Buildings",
      "line_1": "Flat 1",
      "line_2": "Wellington Buildings",
      "line_3": "Wellington Way",
      "udprn": 7871403,
      "traditional_county": "Greater London",
      "country": "England"
    },
    {
      "postcode": "E3 4NA",
      "post_town": "London",
      "building_name": "Wellington Buildings",
      "line_1": "Flat 2",
      "line_2": "Wellington Buildings",
      "line_3": "Wellington Way",
      "udprn": 7871414,
      "traditional_county": "Greater London",
      "country": "England"
    },
    {
      "postcode": "E3 4NA",
      "post_town": "London",
      "building_name": "Wellington Buildings",
      "line_1": "Flat 3",
      "line_2": "Wellington Buildings",
      "line_3": "Wellington Way",
      "udprn": 7871425,
      "traditional_county": "Greater London",
      "country": "England"
    }
  ]
}
```

Luego de modificar el JSON al visitar`http://localhost:4000/postcodes` accederemos a esta información.

El propósito es autocompletar un select que será utilizado en la UI. Como muchos de estos datos son innecesarios utilizaremos un endpoint de nuestra API `/postcodes` para realizar el fetching a la API externa y devolver la data con el formato deseado.

Para ello creamos una carpeta `api` y dentro una carpeta `postcodes` y allí un archivo `+server.js`.

```
- routes
  - api
  	- postcodes
    	- +server.js
```

En `+server.js` creamos un GET request que realice el fetch a la API externa y devuelva los datos formateados.

```jsx
export async function GET() {
	const response = await fetch('http://localhost:4000/postcodes');
	const postcodes = await response.json();
	const appPostCodes = postcodes.map((postcode) => {
		return {
			buildingName: postcode.building_name,
			line1: postcode.line_1,
			line2: postcode.line_2,
			line3: postcode.line_3,
			town: postcode.post_town,
			country: postcode.country,
			postcode: postcode.postcode
		};
	});
	return new Response(JSON.stringify(appPostCodes), {
		headers: {
			'Content-Type': 'application/json'
		}
	});
}
```



Si visitamos ahora `http://localhost:5173/api/postcodes` accederemos a la información formateada.



Luego creamos la función `load` que retorna la data de la API utilizando el endpoint creado recientemente:

```
- routes
  - address
  	- +page.js
  	- +page.svelte
```

Entonces en `+page.js` para cada postcode retornamos un string de address.

```jsx
export const load = async () => {
	const response = await fetch('http://localhost:5173/api/postcodes');
	const postcodes = await response.json();
	const addressList = postcodes.map((postcode) => {
		return `${postcode.buildingName}, ${postcode.line1} ,${postcode.line2}, ${postcode.line3}, ${postcode.town}, ${postcode.country}, ${postcode.postcode}`;
	});
	return { addressList };
};
```

En `+page.svelte` recibimos esta data como props y la presentamos en pantalla:

```vue
<script>
	let address = '';
	export let data;
	const { addressList } = data;
</script>

<h1>Select your address</h1>

<select bind:value={address}>
	<option value="">Pick an address</option>
	{#each addressList as addressItem}
		<option value={addressItem}>{addressItem}</option>
	{/each}
</select>
```



Nuevamente obtenemos el warning que nos indica: For best results, use the `fetch` that is passed to your `load` function.

Para satisfacer esto modificamos `+page.js`:

```jsx
export const load = async (loadEvent) => {
	const { fetch } = loadEvent;
	const response = await fetch('/api/postcodes');
	const postcodes = await response.json();
	const addressList = postcodes.map((postcode) => {
		return `${postcode.buildingName}, ${postcode.line1} ,${postcode.line2}, ${postcode.line3}, ${postcode.town}, ${postcode.country}, ${postcode.postcode}`;
	});
	return { addressList };
};
```

Este `fetch` de SvelteKit mejora el fetching de datos de dos formas:

* Utilizando el `fetch` nativo estamos realizando una llamada HTTP de nuestro server a nuestro server lo cual es redundante. Esto afectará la performance. El `fetch` de SK directamente llama al handler del GET sin la necesidad de un request adicional.
* Con el `fetch` nativo debemos utilizar la URL completa mientras que con el de SK podemos utilizar **requests relativos**.



## Universal Load Function

La función `load` que creamos en `+page.js` y estuvimos estudiando recibe el nombre de universal load function ya que se ejecuta tanto en el sevidor como en el cliente.

Para demostrar esto primero editamos el `src/app.html` que inicialmente es así:

```html
<body data-sveltekit-preload-data="hover">
		<div style="display: contents">%sveltekit.body%</div>
</body>
```

Sacamos el `data-sveltekit-preload-data="hover"` ya que puede traer confusión aunque luego veremos en detalle el propósito de este atributo.

```html
<body>
		<div style="display: contents">%sveltekit.body%</div>
</body>
```



Si agregamos un `console.log()` en `/src/routes/products/+page.js`

```jsx
export async function load(loadEvent) {
	console.log('Load function called in page.js')
	const {fetch} = loadEvent;
	const title = "List of available products";
  const response = await fetch('http://localhost:4000/products');
  const products = response.json()
  return {
    title,
  	products
  }
}
```

Al refrescar la página `/products` veremos el log tanto en las devtools como en la terminal, lo que demuestra que la función se ejecuta en el servidor y en el navegador.

Tener presente que al realizar *client side navigation* no ejecutamos código en el servidor, es decir que si desde `/` hacemos click en un botón para navegar hacia `/products` no veremos el log en la terminal pero sí en las devtools.



## Server Load Funciton

Recientemente vimos la función `load` definida en `+page.js` encargada de *loading page data* y que se ejecuta tanto en servidor como en el navegador.

En ocasiones vamos a querer ejecutar una data loading function solo en el servidor. Es útil cuando trabajamos con información sensible que no queremos que sea accesible desde el navegador. Por ejemplo si tenemos una private key como parte de la request a la API o si queremos conectarnos directamente a la base de datos para obtener los datos, en ambos casos podríamos estar utilizando variables de entorno privadas que nunca deben ser enviadas al navegador. Una universal load function (ULF) comprometería este principio, en estos casos utilizaremos una server load function (SLF).

Una server load function es similar a la universal load function, en el sentido que es una función llamada `load` que retorna un objeto que es inyectado en el componente como data prop. Sin embargo, la diferencia está en que esta será ejecutada sólo en el navegador.

Creamos un archivo `+page.server.js`:

```
- routes
  - products
    - +page.svelte
    - +page.server.js
```
(para evitar confunsiones borramos el `- +page.js` que contiene la ULF)

Debemos realizar algunos cambios respecto a la función `load` vista anteriormente:

* El argumento pasado a una SLF es distinto del pasado a una ULF por lo tanto en vez de `loadEvent` lo llamamos `serverLoadEvent`.

```jsx
export async function load(serverLoadEvent) {
	console.log('Load function called in page.server.js')
	const {fetch} = serverLoadEvent;
	const title = "List of available products";
  const response = await fetch('http://localhost:4000/products');
  const products = response.json()
  return {
    title,
  	products
  }
}
```

Si navegamos a `/products` no veremos el log en la consola de las dev tools pero sí en la terminal, con lo cual confirmamos que sólo se ejecuta en el servidor (el código ni se caga ni se ejecuta en el servidor).



### Deshabilitar JavaScript

Si desde las Devtools presionamos Ctrl+Shift+P y **Disable JavaScript** si a continuación realizamos una navegación client-side haciendo click en un enlace veremos el contenido de la página cargado correctamente.

Desde la pestaña de Network veremos que se realiza una request a products por lo que la load function se ejecutará en el server, obtendrá la data y la devolverá para mostrar en pantalla (cosa que podremos ver en Preview y Response). 



# Universal vs Server Load Function

Tanto la ULF como la SLF se utilizan para obtener data para una página. Hemos visto que la SLF como su nombre lo indica corre sólo en el servidor y su código nunca alcanza al navegador, lo que la vuelve ideal para retornar datos que requieren de información sensible para obtenerlos (db credentials, private API keys).

La utilidad de una ULF radica en que las SLF sólo pueden devolver valores que sean serializables (JSON, map, set, etc) pero no puede retornar clases o constructores de componentes.

A continuación veremos que con una ULF podremos devolver un componente mientras que con una SLF esto no es posible.

Hasta ahora en  `+page.svelte`  recibimos la `data` y la renderizamos en el HTML:

```vue
<script>
	export let data;
	const products = data.products;
</script>

<h1>{data.title}</h1>
{#each products as product}
<div>
	<h2>{product.title}</h2>
	<p>{product.description></p>
	<hr /> 
</div>
{/each}
```

Queremos extraer el contenido de adentro del `div` a un componente aparte llamado `product.svelte`

```
- routes
  - products
    - +page.svelte
    - +page.server.js
    - product.svelte
```



En `product.svelte`:

```vue
<script>
	export let product;
</script>

<h2>{product.title}</h2>
<p>{product.description></p>
<hr /> 
```



En `+page.svelte` vamos a hardcodear el componente `Product`

```vue
<script>
	import Product from './product.svelte'
	export let data;
	const products = data.products;
</script>

<h1>{data.title}</h1>
{#each products as product}
<div>
	<Product {product}/>
</div>
{/each}
```

Notar que ponemos `product` que es equivalente a `product={product}`


En este momento al visitar /products  en pantalla veremos lo mismo que teníamos anteriormente.

En lugar de hardcodear el componente `Product` con una ULF podemos retornar el componente como parte de la prop `data`.

Creamos un archivo `page.js` (`page.server.js` lo borramos momentáneamente) con la ULF.

```
- routes
  - products
    - +page.svelte
    - +page.js
    - product.svelte
```



`page.js` nos queda así:

```jsx
import Product './product.svelte';

export async function load(loadEvent) {
	console.log('Load function called in page.js')
	const {fetch} = loadEvent;
	const title = "List of available products";
  const response = await fetch('http://localhost:4000/products');
  const products = response.json()
  return {
    title,
  	products,
    Component: Product
  }
}
```

Estamos pasando un constructor `Component` como data a la página. 



Luego en `+page.svelte`

```vue
<script>
	export let data;
	const products = data.products;
	const Component = data.Component;
</script>

<h1>{data.title}</h1>
{#each products as product}
<div>
	<Component {product}/>
</div>
{/each}
```



Intetamos lo mismo con una SLF. Creamos un archivo `+page.server.js`

```jsx
import Product './product.svelte';

export async function load(serverLoadEvent) {
	console.log('Load function called in page.server.js')
	const {fetch} = serverLoadEvent;
	const title = "List of available products";
  const response = await fetch('http://localhost:4000/products');
  const products = response.json()
  return {
    title,
  	products,
    Component: Product
  }
}
```

Habiendo utilizado la SLF al visitar /products veremos un error 500 que nos dirá **GET http://localhost:5173/products** en las DevTools y en la terminal veremos el mensaje **Error: Data returned from 'load' when rendering /products is not serializable*: Cannot stringify a function**



## ULF con SLF

En ocasiones tendremos que retornar data que requiere de información sensible para obtenerla y también queremos retornar info no serializable como constructores de componentes o clases. SvelteKit nos permite utilizar ambas funciones a la vez. Esto significa que podremos obtener la data de una página usando SLF, ULF o ambas.

Los datos retornados por la SLF son provistos a la ULF como parte del argumento `loadEvent`.

Por lo tanto en la SLF `+page.server.js` retornamos `title` y `products`

```jsx
export async function load(serverLoadEvent) {
	console.log('Load function called in page.server.js')
	const {fetch} = serverLoadEvent;
	const title = "List of available products";
  const response = await fetch('http://localhost:4000/products');
  const products = response.json()
  return {
    title,
  	products,
  }
}
```

Luego en la ULF `+page.js` recibimos `data` de la SLF y no es necesaria la lógica de fetching.

```jsx
import Product './product.svelte';

export async function load(loadEvent) {
	console.log('Load function called in page.js')
	const {data} = loadEvent;
	
  return {
    ...data, 
    Component: Product
  }
}
```



## URL Data

Frecuentemente vamos a querer cargar datos específicos de una entidad de acuerdo a los parámetros recibidos en la URL. 

Vamos a hacer uso de JSON Server para proporcionarnos datos de un producto en particular al ingresar a http://localhost:4000/products/1

```
- routes
  - products
    - [productId]
    	- +page.svelte
```

En `+page.svelte` por el momento simplemente tenemos:

```
<h1>Product details</h1>
```

Si ahora visitamos http://localhost:5173/products/1 veremos este contenido.

Ahora queremos cargar la data que será consumida por esta página.

Tomamos la elección de hacerlo en una **SLF** por ello creamos un archivo `+page.server.js`

```
- routes
  - products
    - [productId]
     	- +page.svelte
    	- +page.server.js
```

Realizamos la fetch a la API generada con JSON Server.
```jsx
export async function load(serverLoadEvent) {
	const {fetch, params} = serverLoadEvent;
  const {productId} = params;
  const title = "Product details";
  const response = await fetch(`http://localhost:4000/products/${productId}`);
  const product = response.json()
  return {
    title,
  	product,
  }
}
```

Luego modificamos `+page.svelte` para consumir estos datos:

```vue
<script>
	import Product from './product.svelte'
  export let data;
	const title = data.title;
  const product = data.product;
</script>

<h1>{title}</h1>
<div>
	<h2>{product.title} - ${product.price}</h2>
  <p>{product.description}</p>
</div>


```

Si ahora visitamos http://localhost:5173/products/1 veremos la información correcta.



#### `params`, `url`, `route`

Tanto en las SLF como en la ULF además de `params` contamos con `url` y `route` que podrán resultarnos útiles en alguna ocasión. Para conocer su comportamiento si visitamos http://localhost:5173/products/1 y los logueamos en pantalla veremos:



* `params` como ya vimos nos entrega un objeto con los parámetro de la ruta.

```
{ productId: '1' }
```



* `url` como su nombre lo indica contiene todos los key/value relacionados con la url.

```
URL {
  href: 'http://localhost:5173/products/1',
  origin: 'http://localhost:5173',
  protocol: 'http:',
  username: '',
  password: '',
  host: 'localhost:5173',
  hostname: 'localhost',
  port: '5173',
  pathname: '/products/1',
  search: '',
  searchParams: URLSearchParams {},
  hash: ''
}
```



* `route` nos permite conocer el nombre del directorio de la ruta actual.

```
route
{ id: '/products/[productId]' }
```



# Handle Erorrs

Veremos a continuación como trabajar errores en la función `load`.

Si visitamos http://localhost:5173/products/4 es decir con un `productId` inexistente en nuestro JSON Server veremos en pantalla toods valores `undefined`. Lo que queremos hacer en cambio es mostrarle un mensaje de error (404 product not found) al usuario.

Lo que haremos será trabajar en `+page.server.js` y allí verificar si el `productId` es mayor que 3.

En primer lugar utilizaremos el objeto `Error` y veremos qué sucede:

```jsx
export async function load(serverLoadEvent) {
	const {fetch, params} = serverLoadEvent;
  const { productId } = params;
  if (productId > 3) {
    throw new Error('Product not found')
  }
  const title = "Product details";
  const response = await fetch(`http://localhost:4000/products/${productId}`);
  const product = response.json()
  return {
    title,
  	product,
  }
}
```

Si ahora visitamos http://localhost:5173/products/4 veremos en pantalla un error 500. Esto se debe a que en las load functions debemos manejar los erorres con una `error` que es una helper function de SvelteKit.

```jsx
import { error } from '@sveltejs/kit'
export async function load(serverLoadEvent) {
	const {fetch, params} = serverLoadEvent;
  const { productId } = params;
  if (productId > 3) {
    throw error(404, 'Product not found')
  }
  const title = "Product details";
  const response = await fetch(`http://localhost:4000/products/${productId}`);
  const product = response.json()
  return {
    title,
  	product,
  }
}
```

La función `error` acepta dos argumentos el primero es el status code y el segundo el mensaje de error. 

Ahora veremos en pantalla: 404 - Product not found con la página por defecto de error de SvelteKit. Podemos personalizarla creando un archivo `+error.svelte`.

```
- routes
  - products
    - [productId]
     	- +page.svelte
    	- +page.server.js
    	- +error.svelte
```



En `+error.svelte`

```vue
<script>
	import { page } from '$app/stores'
</script>

<h1>{$page.status}: {$page.error.message}</h1>
```



## Objeto personalizado

Hemos hasta ahora devuelto desde `+page.server.js` como segundo argumento un string  `throw error(404, 'Product not found')` y lo recibimos `+error.svelte` como `$page.error.message`. 

Pero podríamos haber devuelto en lugar de un string un objeto por ejemplo con las siguientes propiedades: `throw error(404, {message: 'Product not found', hint: 'Try a different product'})` que accederíamos con `$page.error.message` y `$page.error.hint`.



# Redirects

Desde la función `load` es posible redireccionar al usuario gracias a otro helper de SvelteKit llamado `redirect`. 

Por ejemplo podríamos querer redireccionarlos si el `productId` es mayor que 3. 

Tener presente que para el caso mencionado donde un usuario ingresa a una ruta con un `productId` inválido es más apropiado mostrar un error que para la redirección pero por simplicidad tomamos el mismo caso. Un caso de uso más apropiado sería si el usuario intenta ingresar a una ruta protegida sin estar logueado y lo redireccionamos a `/login`.

Al trabajar con `redirect` como primer argumento indicamos el status code (podemos usar 307 que representa temporary redirect) y como segundo la ruta a la cual queremos hacer la redirección (podemos usar `/products`)

```jsx
import { redirect } from '@sveltejs/kit'
export async function load(serverLoadEvent) {
	const {fetch, params} = serverLoadEvent;
  const { productId } = params;
  if (productId > 3) {
    throw redirect(307, '/products')
  }
  const title = "Product details";
  const response = await fetch(`http://localhost:4000/products/${productId}`);
  const product = response.json()
  return {
    title,
  	product,
  }
}
```



# Layout Data

Hasta ahora tenemos una página donde mostramos todos los productos y una dónde mostramos los detalles de un producto en particular. En ambos contamos con un `+page.js` o `+page.server.js` con la `load` function con la que proporcionamos los datos necesarios.

```
- routes
  - products
    
    - [productId]
     	- +page.svelte
    	- +page.server.js
    
    - +page.svelte
    - +page.server.js
```

Queremos incorporar en ambas páginas una sección de *featured products*. En primer lugar modificamos el `db.json` de json server agregando la propiedad `featured-products` de modo que al ingresar a http://localhost:4000/featured-products accedamos a este listado.

```json
{
	"products": [
		{
			"id": 1,
			"title": "Product 1",
			"price": 700,
			"description": "Description 1"
		},
		{
			"id": 2,
			"title": "Product 2",
			"price": 1050,
			"description": "Description 2"
		},
		{
			"id": 3,
			"title": "Product 3",
			"price": 2600,
			"description": "Description 3"
		}
	],
	"featured-products": [
		{
			"id": 1,
			"title": "Featured Product 1",
			"price": 700,
			"description": "Description 1"
		},
		{
			"id": 2,
			"title": "Featured Product 2",
			"price": 1050,
			"description": "Description 2"
		},
		{
			"id": 3,
			"title": "Featured Product 3",
			"price": 2600,
			"description": "Description 3"
		}
	],
}
```



Si bien podríamos hacer el fetch en ambas funciones `load` estaríamos violando el principio DRY (do not repeat yourself). Una solución óptima consiste en envolver las dos páginas en un layout y cargar los featured products allí.

Por lo tanto en `products` creamos el archivo `+layout.svelte`

```
- routes
  - products
    
    - [productId]
     	- +page.svelte
    	- +page.server.js
    
    - +page.svelte
    - +page.server.js
    - +layout.svelte
```

Si bien sabemos que en `+layout.svelte` mostraremos los productos destacados los datos se los pasaremos a través de `+layout.js` o `+layout.server.js` siguiendo una convención similar a la vista anteriormente.

Creamos un `+layout.js` que será una ULF que correrá tanto en el servidor como en el navegador:

```
- routes
  - products
    
    - [productId]
     	- +page.svelte
    	- +page.server.js
    
    - +page.svelte
    - +page.server.js
    
    - +layout.svelte
    - +layout.js
```



Por lo tanto en `+layout.js`

```jsx
export async function load(loadEvent){
	const { fetch } = loadEvent;
	const title = 'Featured products';
	const response = await fetch('http://localhost:4000/featured-products')
	const featuredProducts = response.json();
	
	return {title, featuredProducts}
}
```



En `+layout.svelte` consumiremos esta data mediante el objeto retornado como la prop `data`. 

```vue
<script>
	export let data;
	const title = data.title;
	const featuredProducts = data.featuredProducts;
</script>

<slot />
<h2>{title}</h2>
{#each featuredProducts as product}
    <div>
        <h2>${product.title}</h2>
        <p>${product.description}</p>
    </div>
{/each}
```



# Using parent data

En ocasiones vamos a necesitar datos que son cargados más arriba en el árbol de componentes. 

Como ejemplo consideramos el que en el **root layout** vamos a cargar los detalles del usuario y también queremos acceder a lo mismo dentro del layout de products.

<img src="Introducción.assets/image-20230425082959681.png" alt="image-20230425082959681" style="zoom:50%;" />

```
- routes
  - products
    - +layout.svelte
    - +layout.js
    
  
  - +layout.svelte
  - +layout.js
```

En la raíz de `routes` creamos un archivo `+layout.svelte` y uno `+layout.js` de manera similar a lo visto anteriormente.

En `+layout.js` obtenemos los datos del usuario (en este caso no hacemos el fetch de una API externa sino que los hardcodeamos por simplicidad)

```js
export async function load(loadEvent){
	return {
		username: 'juaneme8'
	}
}
```

En `+layout.svelte` consumimos estos datos:

```vue
<script>
	export let data;
	const { username } = data;
</script>

<div>Welcome, {username}</div>
<slot />
```



Queremos mostrar el nombre de usuario en el layout vinculado con el layout de los detalles de producto



En `products/+layout.js` accedemos a parent data a través de la función `parent` 

```js
export async function load(loadEvent){
	const { fetch, parent } = loadEvent;
  const parentData = await parent()
  const { username } = parentData;
	const title = 'Featured products';
	const response = await fetch('http://localhost:4000/featured-products')
	const featuredProducts = response.json();
	
	return {username, title, featuredProducts}
}
```

Luego en `products/+layout.svelte``

```vue
<script>
	export let data;
  const username = data.username;
	const title = data.title;
	const featuredProducts = data.featuredProducts;
</script>


<slot />
<h2>{title} for {username}</h2>
{#each featuredProducts as product}
    <div>
        <h2>${product.title}</h2>
        <p>${product.description}</p>
    </div>
{/each}
```



Las técnicas vistas son válidas:

* Sin importar el nivel de anidamiento que tenga el componente hijo en el árbol del componentes.

* Tanto para ULF como para SLF.





# Using child data

De manera similar a lo visto anteriormente veremos cómo utilizar datos de hijos en componentes padres o ubicados más arriba en el árbol de componentes.

Como ejemplo consideramos un **banner de notificaciones** ubicado en el root layout que esperamos que sea actualizado por la página que esté actualmente siendo renderizada. Por ejemplo en la página de página de todos los productos queremos mostrar una notificación y en la página de detalles de un producto otra notificación. Esto lo haremos trabajando en la función `load` de `+page.js`

En `products/+page.js` incorporamos el campo `notification` al objeto retornado por la función `load`:

```js
import Product './product.svelte';

export async function load(loadEvent) {
	console.log('Load function called in page.js')
  const notification = 'End of season sale!'
	const {data} = loadEvent;
	
  return {
    ...data, 
    Component: Product
    notification,
  }
}
```



Lo mismo hacemos en `products/[productId]/+page.server.js`

```jsx
import { redirect } from '@sveltejs/kit'
export async function load(serverLoadEvent) {
	const {fetch, params} = serverLoadEvent;
  const { productId } = params;
 
  if (productId > 3) {
    throw redirect(307, '/products')
  }
  const title = "Product details";.
  const notification = 'End of season sale! 50% off'
  const response = await fetch(`http://localhost:4000/products/${productId}`);
  const product = response.json()
  return {  
    title,
  	product,
    notification,
  }
}
```



Es posible acceder a estos datos en el layout padre, por lo tanto en `+layout.svelte`

```vue
<script>
	import { page } from '$app/stores'
	export let data;
	const { username } = data;
</script>

{#if $page.data.notification}
	<p>{$page.data.notification}</p>
{/if}

<div>Welcome, {username}</div>
<slot />
```



Un caso de uso práctico es para configurar el título de una página en la aplicación.

```vue
<script>
	import { page } from '$app/stores'
	export let data;
	const { username } = data;
</script>

<svelte:head>
	<title>{$page.data.title} || 'Default page name'</title>
</svelte:head>

{#if $page.data.notification}
	<p>{$page.data.notification}</p>
{/if}

<div>Welcome, {username}</div>
<slot />
```



# Promise Unwrapping

Modificamos ahora el JSON Server en `db.json` de manera que incorporemos tres propiedades nuevas:

```json
{
	"most-active-stock": {
		"symbol": "STOCK A",
		"price": 351.0
	},
	"top-gaining-stock": {
		"symbol": "STOCK B",
		"price": 50.0
	},
	"top-losing-stock": {
		"symbol": "STOCK C",
		"price": 100.0
	},
}
```



* `most-active-stock`
* `top-gaining-stock`
* `top-losing-stock`

Estas nos habilitarán tres nuevos endpoints:

* http://localhost:4000/most-active-stock
* http://localhost:4000/top-gaining-stock
* http://localhost:4000/top-gaining-stock



Supongamos que queremos mostrar estos tres valores en una página:

```
- routes
  - stocks
    - +page.svelte
    - +page.js
```



Luego en `+page.js` obtenemos la data realizando tres llamadas a la API.

```jsx
export const load = async (loadEvent) => {
	const {fetch} = loadEvent;
    const mostActiveStockResponse = await fetch('http://localhost:4000/most-active-stock');
    const topGainingStockResponse = await fetch('http://localhost:4000/top-gaining-stock');
    const topLosingStockResponse = await fetch('http://localhost:4000/top-losing-stock');
    
    const mostActiveStock = await mostActiveStockResponse.json();
    const topGainingStock = await topGainingStockResponse.json();
    const topLosingStock = await topLosingStockResponse.json();
    
    return {
        mostActiveStock,
        topGainingStock,
        topLosingStock
    }
}
```



Luego en `+page.svelte` accedemos a estos datos mediante la prop `data`.

```vue
<script>
	export let data;
	const {mostActiveStock,topGainingStock,topLosingStock} = data;
</script>

<span>Most Active Stock - {mostActiveStock.symbol}</span>
<span>Top Gaining Stock - {topGainingStock.symbol}</span>
<span>Top Losing Stock - {topLosingStock.symbol}</span>
```



Es posible realizar una mejora en términos de performance haciendo uso del concepto de **Promise Unwrapping** es una característica de las `load` functions que nos permite evitar el waterfall effect que se causa el tener que hacer `await` de `response.json` una tras otra, ya que tendremos que esperar que cada una de esas promesas se resuelvan antes de resolver la siguiente.

Para optimizarlo lo especificamos como parte del return statement sin el keyword `await` de modo que  SvelteKit se encargará de resolver las promesas en paralelo.

```jsx
export const load = async (loadEvent) => {
	const {fetch} = loadEvent;
    const mostActiveStockResponse = await fetch('http://localhost:4000/most-active-stock');
    const topGainingStockResponse = await fetch('http://localhost:4000/top-gaining-stock');
    const topLosingStockResponse = await fetch('http://localhost:4000/top-losing-stock');

    return {
        mostActiveStock: mostActiveStockResponse.json(),
        topGainingStock: topGainingStockResponse.json(),
        topLosingStock: topLosingStockResponse.json()
    }
}
```



Tener presente que SvelteKit sólo resolverá **top level promises**, esto significa que si tenemos por ejemplo:

```jsx
return {
        mostActiveStock: mostActiveStockResponse.json(),
        topGainingStock: topGainingStockResponse.json(),
        topLosingStock: topLosingStockResponse.json(),
        anotherStock: {
        	nestedStock: response.json()
        }
    }
```

Esto no funcionará.



# Data invalidation

Frecuentemente vamos a estar mostrando datos en pantalla y necesitaremos invalidarlos luego de que ocurra un evento y realizar un nuevo fetch para botener los datos actualizados.

Trabajaremos también con `+page.js` y `+page.svelte` creados en Promise Unwrapping dentro de `stocks`, pero para esta sección migramos ambos archivos a `+layout.js` y `+layout.svelte` respectivamente.

Actualizamos `db.json` agregando la propiedad `stocks`:

```json
{
	"stocks": [
		{
			"symbol": "STOCK D",
			"price": 151.0
		},
		{
			"symbol": "STOCK E",
			"price": 100.0
		},
		{
			"symbol": "STOCK F",
			"price": 240.0
		}
	],
}
```



Ahora en `page.js` hacemos el fetch y retornamos los datos:

```js
export const load = async (loadEvent) => {
	const { fetch, depends } = loadEvent;
	const response = await fetch('http://localhost:4000/stocks');
	const stocks = await response.json();
	return { stocks };
};
```



En `page.svelte` mostramos los elementos en pantalla:

```vue
<script>
	export let data;
</script>

<h1>Actively trading stocks</h1>

{#each data.stocks as stock}
	<h2>{stock.symbol} - ${stock.price}</h2>
{/each}
```



Como estos valores fluctúan constantemente. Para simular esto modificamos los valores de `db.json` e inmediatamente veremos reflejados estos cambios en `http://localhost:4000/stocks` debido al watch mode, pero en la UI seguiremos viendo los valores viejos (stale data). 

Si bien podríamos tener una función que se ejecute cada algunos segundos y actualice los datos, el requerimiento que se nos hace es contar con un botón de refrescar que haga el fetch de los valores actualizados.

Podríamos llamar nuevamente al endpoint /stocks pero esto hace que tengamos orígenes distintos de los datos para el render inicial (load function) y para los renders subsiguientes lo cual complica las cosas. La solución mas simple es llamar a la función `load` que realizará el fetch nuevamente de la API, retornará los datos actualizados para que sean inyectados en la página y la ui será renderizada. Para lograr que SvelteKit ejecute nuevamente la `load` function hacemos uso de la función `invalidate`.

```vue
<script>
	import { invalidate } from '$app/navigation';
	export let data;
  
	function refresh() {
		invalidate('http://localhost:4000/stocks');
	}
</script>

<h1>Actively trading stocks</h1>

{#each data.stocks as stock}
	<h2>{stock.symbol} - ${stock.price}</h2>
{/each}

<button on:click={refresh}>Refresh</button>
```

SvelteKit buscará todas las `load` functions donde se efectúe un request a esta URL y las volverá a ejecutar.

Invalidar datos mediante la URL se vuelve dificil en aquellos casos en los cuales trabajamos con urls que aceptan parámetros, en esos casos podremos utilizar `depends` disponible en `loadEvent`. Lo que haremos con `depends` será pasarle un string que actuará como label para esa función `load`.

Por lo tanto en `+page.js`

```js
export const load = async (loadEvent) => {
	const { fetch, depends } = loadEvent;
	depends('stocks:actively-trading');
	const response = await fetch('http://localhost:4000/stocks');
	const stocks = await response.json();
	return { stocks };
};
```



Luego en `+page.svelte` simplemente podremos tener:

```vue
<script>
	import { invalidate } from '$app/navigation';
	export let data;
  
	function refresh() {
		invalidate('stocks:actively-trading');
	}
</script>

<h1>Actively trading stocks</h1>

{#each data.stocks as stock}
	<h2>{stock.symbol} - ${stock.price}</h2>
{/each}

<button on:click={refresh}>Refresh</button>
```



Tener presente que solo invalidaremos `load` functions que matcheen con la URL que le pasemos o la etiqueta. Por ejemplo si quisiéramos que al presionar refresh también se actualice la información provista por el fetch de  `+layout.js` (el que originalmente teníamos en `+page.js` ). 

```jsx
export const load = async (loadEvent) => {
	const {fetch} = loadEvent;
    const mostActiveStockResponse = await fetch('http://localhost:4000/most-active-stock');
    const topGainingStockResponse = await fetch('http://localhost:4000/top-gaining-stock');
    const topLosingStockResponse = await fetch('http://localhost:4000/top-losing-stock');
    
    const mostActiveStock = await mostActiveStockResponse.json();
    const topGainingStock = await topGainingStockResponse.json();
    const topLosingStock = await topLosingStockResponse.json();
    
    return {
        mostActiveStock,
        topGainingStock,
        topLosingStock
    }
}
```

Si bien podríamos agregar aquí un `depends` e invalidar también eso existe otra forma. 



Una forma más simple en cambio es utilizar `invalidateAll`, por lo que actualizando `+page.svelte`

```vue
<script>
	import { invalidateAll } from '$app/navigation';
	export let data;
  
	function refresh() {
		invalidateAll();
	}
</script>

<h1>Actively trading stocks</h1>

{#each data.stocks as stock}
	<h2>{stock.symbol} - ${stock.price}</h2>
{/each}

<button on:click={refresh}>Refresh</button>
```

Como consecuencia de esto al presionar Refresh invalidaremos todas las load functions activas.



# Link Options

Hemos visto que en SvelteKit utilizamos el anchor tag para navegar entre las distintas rutas de nuestra aplicación. Bastará con que el `href` no sea un sitio externo sino una ruta de nuestra aplicación y navegaremos a esa página. Importaremos el código del componente y luego llamaremos a las `load` functions que necesita para obtener datos.

En `/routes/+page.svelte` suponemos que tenemos:

```html
<h1>Welcome to the home page</h1>
<a href="/products">Products</a>
```



En las DevTools veremos en Network todas las requests que se efectúan cuando hacemos click en el botón para navegar por ejemplo a `/products`:

`__data.json` (data de JSON server), `+page.svelte`,`+page.js`,`+layout.svelte`,`+layout.js`, `product.svelte`, `featured-products` 

Veremos que es posible personalizar el fetching de código y de datos.

 

## Preload Data

Hemos visto que al hacer click en un botón para navegar a otra página, se carga tanto el código como los datos del server y se renderiza la página en el navegador. SvelteKit proporciona atributos que nos permiten hacer un preload de los datos y del código.

Para hacer un **preload de los datos y el código** podemos modificar `+page.svelte` de la siguiente forma:

```html
<h1>Welcome to the home page</h1>
<a href="/products" data-sveltekit-preload-data="hover">Products</a>
```

Al hacer hover (o touch start event en mobile) sobre el link veremos que se disparan todas las requests que anteriormente veíamos cuando hacíamos click en el link. Esto trae como consecuencia que al hacer click la carga de la página sea instantánea.

Si bien esto puede ocasionar mejoras en la performance, en ocasiones puede no ser deseado. Hemos dado por sentado que el usuario va a hacer click en el link pero puede que no lo haga o lo que es peor que los datos cambien y necesiten ser actualizados. 

En ese caso en lugar de hover utilizamos `tap` que se dispara cuando el usuario hace click aún sin soltar el mouse.

```html
<h1>Welcome to the home page</h1>
<a href="/products" data-sveltekit-preload-data="tap">Products</a>
```



Si tenemos múltiples links que queremos hacer el preload no tenemos que agregar este atributo a todos ellos sino que podíamos ponerlo una única vez en el `body`.

Recordemos que al estudiar la ULF justamente quitamos `data-sveltekit-preload-data="hover"` del `body`. Ahora lo volvemos a agregar:

```html
<body data-sveltekit-preload-data="hover">
		<div style="display: contents">%sveltekit.body%</div>
</body>
```

Como consecuencia de esto bastará con hacer hover sobre cualquier elemento link para que se efectúe el preloading.

* Podemos agregar este atributo en un elemento que envuelva a varios links (un `div` por ejemplo)
* Podemos agregar el atributo al tag `html` pero quitarlo de aquellos links que no queremos que se haga el preload (esto será útil si nos interesa tener preload en un gran porcentaje de nuestros links y no tenerlo en uno pequeño).

```html
<h1>Welcome to the home page</h1>
<a href="/products" data-sveltekit-preload-data="off">Products</a>
```



## Preload Code

El preload de data y código es una técnica interesante desde el punto de vista de UX ya que logramos que la próxima ruta cargue instantáneamente para el usuario. Sin embargo, esto no es deseable en todos los escenarios. Si tenemos datos que se actualizan permanentemente hacer el preload ante un hover del link nos generará stale data si el usuario decide navegar luego de unos segundos. Podríamos hacer un preload ante un tap pero en estos casos lo mejor es hacer un preload del código únicamente (y no data) ante un hover.

Eliminamos el atributo del `body` y trabajamos sobre la página de inicio, ya vimos que si tenemos lo siguiente:

```html
<h1>Welcome to the home page</h1>
<a href="/products" data-sveltekit-preload-data="hover">Products</a>
```

Se realiza el preload de los datos (`__data.json`) y del código (`+page...`,`+layout...`) cosa que observamos en las devtools.

Para evitar trabajar con data obsoleta (staleness data) precargamos sólo el código y no los datos. Esto lo hacemos utilizando el atributo `data-sveltekit-preload-code`:

```html
<h1>Welcome to the home page</h1>
<a href="/products" data-sveltekit-preload-code="hover">Products</a>
```

Al hacer click sobre el link los datos serán fetcheados y no serán stale.



Los valores posibles de `data-sveltekit-preload-code` son: 

* `hover`, `tap` (ambos idénticos a lo visto anteriormente), 

* `eager` con este valor los links serán precargados directamente sin la necesidad de hacer click o hover.

* `viewport` los links serán precargados cuando entren al viewport. Si tenemos un elemento fuera de la pantalla y scrolleamos hacia el, cuando aparezca en el viewport se realizará el preload.



Debemos tener presente que el preload de código es un prerrequisito del preload de data, por lo que si tenemos ``data-sveltekit-preload-data` con valor `hover` o `tap` implicará que  `data-sveltekit-preload-code` también tiene estos valores. 



## Otros atributos

Además de `data-sveltekit-preload-data` y `data-sveltekit-preload-code` existen otros dos atributos que no son tan usados: `data-sveltekit-reload` y `data-sveltekit-noscroll`



### `data-sveltekit-reload`

Cuando utilizamos elementos anchor tags (`<a>`) el comportamiento por defecto es el estudiado donde SvelteKit los controlará posibilitándonos la client side navegation.

```
<a href="/products">Products</a>
```

Esto significa que cuando hacemos click en el enlace navegaremos sin la necesidad de que la página recargue. Si por algún motivo necesitamos que la página se recargue (full page navigation) podemos hacerlo de esta forma:

```
<a href="/products" data-sveltekit-reload>Products</a>
```



### `data-sveltekit-noscroll`

En ocasiones vamos a querer conservar el estado de scroll actual al navegar a otra ruta. El comportamiento por defecto al navegar a rutas dentro de la aplicación SvelteKit  consiste en posicionarnos en la parte superior de la pantalla scrolleando respecto de la posición anteiror en caso de ser necesario. 

Si queremos deshabilitar esta opción podemos hacerlo modificando `routes/+page.svelte` de esta forma:

```vue
<a href="/products" data-sveltekit-noscrol>Products</a>

<style>
  .tall{
    height: 100vh;
  }
</style>
```

Nos apoyamos en CSS para hacer notorios estos cambios.



# Preload Programmatically

En la sección anterior vimos cómo personalizar el comportamiento de los links usando atributos, pero como también es posible la navegación programática también podemos hacer preload de data y código.

En `routes/+page.svelte` supongamos que tenemos:

```vue
<script>
import { goto } from '$app/navigation'
</script>

<button on:click={() => {
goto('/products')
}}>Go to products</button>
```

Lo primero que podemos observar es que al hacer hover del botón no realizamos pre-load de data ni de código.

Al hacer click en el botón navegamos a la página de Products y en las devtools podremos ver qué es ahi cuando obtenemos los datos y el código.

```vue  
<script>
import { goto, preloadData } from '$app/navigation'
</script>

<button 
        on:mouseover={async () =>{await preloadData('/products')}}
  			on:focus={async () =>{await preloadData('/products')}}
        on:click={() => {goto('/products')}}>
  Go to products
</button>
```

Al agregar `on:mouseover` también debemos agregar `on:focus` para garantizar la accesibilidad.

Si pasamos el mouse por encima del botón podremos ver el preloading de data y código de manera similar a lo que sucede en un link.



* Si queremos imitar el comportamiento de tap que lograbamos con `data-sveltekit-preload-data="tap"` podemos utilizar `on:mousedown`:

  ```vue
  <script>
  import { goto, preloadData } from '$app/navigation'
  </script>
  
  <button 
          on:mousedown={async () =>{await preloadData('/products')}}
    			on:focus={async () =>{await preloadData('/products')}}
          on:click={() => {goto('/products')}}>
    Go to products
  </button>
  ```

  



* Si queremos hacer el preload de código en lugar de data podemos importar `preloadCode`

```vue
<script>
import { goto, preloadCode } from '$app/navigation'
</script>

<button 
        on:mouseover={async () =>{await preloadCode('/products')}}
  			on:focus={async () =>{await preloadCode('/products')}}
        on:click={() => {goto('/products')}}>
  Go to products
</button>
```

Al pasar el mouse por encima del botón no veremos en las devtools en network el archivo `__data.json`.



# Page Options

Por defecto SvelteKit renderiza un componente en el servidor antes de mandarlo al cliente como HTML y luego va a renderizar el componente otra vez en el navegador para hacerlo interactivo en un proceso que se conoce como **hydration**. Por ejemplo un botón podría haber sido cargado pero no sería clickeable, la interactividad se la proporciona la hidratación.

Este comportamiento de primero renderizar en el servidor y luego hacerlo en el navegador para hacerlo interactivo puede controlarse usando algunas opciones de página:

* SSR: server side rendering

* CSR: client side rendering

  

* Prerender: Renderizar antes de enviar al navegador, típicamente en build time.



## SSR y CSR

Las page options SSR y CSR pueden ser específicadas en `+page.js` o `+page.server.js`

Por ejemplo en `products/+page.js` :

```js
import Product './product.svelte';

export async function load(loadEvent) {
	console.log('Load function called in page.js')
  const notification = 'End of season sale!'
	const {data} = loadEvent;
	
  return {
    ...data, 
    Component: Product
    notification,
  }
}

export const ssr = true;
export const csr = true;
```

Debemos tener presente que `true` es el valor por defecto para ambos por lo que no notaremos ninguna diferencia. 

Por tratarse de una ULF veremos el `console.log()` tanto en la terminal como en el navegador.

Si observamos en el panel de Network veremos que el HTML es generado en el servidor ya que si previsualizamos la respuesta de `products` lo veremos incluído allí. Esto es bueno para performance y SEO.



## CSR only

A continuación buscaremos que el código no corra en el servidor y sólamente lo haga en el navegador.

```js
import Product './product.svelte';

export async function load(loadEvent) {
	console.log('Load function called in page.js')
  const notification = 'End of season sale!'
	const {data} = loadEvent;
	
  return {
    ...data, 
    Component: Product
    notification,
  }
}

export const ssr = false;
export const csr = true;
```

Veremos que el log sólamente en el navegador y si analizamos la pestaña Preview de lo recibido en Network de `products` veremos que está en blanco (no contiene el HTML) y en Response directamente veremos que tenemos un `div`. Se carga JavaScript y luego se ejecuta para montar los elementos HTML en el DOM. Un caso de uso para "CSR only page option" es cuando estamos armando la sección admin de nuestra aplicación, donde no necesariamente necesitamos SEO y probablemente ahorraremos costos de server ya que no tenemos que generar el HTML para cada request.



## SSR only

Consideramos el caso de SSR

```js
import Product './product.svelte';

export async function load(loadEvent) {
	console.log('Load function called in page.js')
  const notification = 'End of season sale!'
	const {data} = loadEvent;
	
  return {
    ...data, 
    Component: Product
    notification,
  }
}

export const ssr = true;
export const csr = false;
```

En este momento podremos ver que el log aparece en la terminal pero no en la consola de las devtools. En la response preview veremos todo el contenido presente que lo convierte en una opción viable cuando el SEO sea importante. Sin embargo si la página requiere JavaScript tendremos que tener ambos en `true`. Un caso de uso podría ser una página de noticias donde sólo se muestra contenido con importancia de SEO pero no de interactividad.



## Both in `false`

Si colocamos `ssr = false` y `csr = false` veremos la página en blanco ya que el código no corre ni en el servidor ni en el navegador.



## Prerender

Renderiza todo el HTML de páginas en *buildtime* en vez de hacerlo en *runtime*. Esto tiene los mismos beneficios que las páginas server-rendered pero como ventaja adicional evita tener que recalcular la página para cada visitante y escala con bajo costo cuando el número de visitantes crece. Como desventaja el buildtime es más costoso y sólo puede ser actualizada mediante el build y deploy de una nueva versión de la aplicación. Un caso de uso podría ser para blogs, ecommerce, documentación o páginas de marketing. En estos casos creamos el HTML una vez, lo cacheamos en un CDN y se lo servimos al cliente de manera casi instantánea mejorando así la performance

### Análisis `.svelte-kit`

```
- .svelte-kit
  - generated
```

Dentro de la carpeta `.svelte-kit` nos encontramos una carpeta `generated` que es creada cada vez que corremos la aplicación y contiene todo el código necesario para correr en desarrollo.



Cuando ejecutamos `npm run build` veremos una carpeta `output` que dentro tiene carpetas `server` (contiene todo el código que corre en el servidor) y `client` (con todo el código que debe ser entregado al navegador)

```
- .svelte-kit
  - output
  	- server
  	- client
```

Como en la página Home no tenemos ningun elemento dinámico que dependa del usuario es un buen candidato para prerender at buildtime en lugar de emplear ssr el HTML ante cada request.



Para agregar esta *page option* nuevamente debemos hacerlo en `+page.js` o `+page.server.js`

En `routes/+page.js`

```jsx
export const load = async () => {
	console.log('home page universal load function called')
}

export const prerender = true;
```



En primer lugar debemos debemos buildear con `npm run build` (veremos el log de la ULF).

Ahora veremos en `.svelte-kit` que en `output` además de `server` y `client` tenemos `prerendered`

```
- .svelte-kit
  - output
  	- server
  	- client
  	- prerendered
  		- pages
  			- index.html
```

En `index.html` podremos ver el HTML de la página home.



A continuación servimos la aplicación con `npm run preview`. Al visitar la página veremos el contenido pero esta vez el HTML no fue conputado cuando hicimos el request sino que fue en buildtime. Esto lo podemos confirmar ante la ausencia del log de la load function en la terminal.

La hidratación sucederá en el navegador y esto lo confirmamos mediante el log que aparece en la consola. Si no lo queremos podemos deshabiltar CSR.





## Pre-rendering API routes

Crearemos un API endpoint sin una utilidad práctica real sino a los fines de presentar el tema.

Creamos una carpeta `current-time` y en ella `+server.js` como vemos a continuación:

```
- routes
  - api
  	- current-time
  		- +server.js
```



```js
export async function GET(){
	console.log('current-time GET handler invoked');
    return new Respone(new Date().toLocaleTimeString());
}
```



Ejecutamos la aplicación con `npm run dev` y veremos que al navegar a http://localhost:5173/api/current-time  (url de development) veremos la hora actual y si refrescamos vemos nuevamente la hora actualizada.

Incorporamos ahora el pre-render

```js
export async function GET(){
	console.log('current-time GET handler invoked');
    return new Respone(new Date().toLocaleTimeString());
}

export const prerender = true;
```



Hacemos el build de la aplicación `npm run build` (veremos el log de esta API route).

En `.svelte-kit` veremos:

```
- .svelte-kit
  - output
  	- server
  	- client
  	- prerendered
  		- api
  			- current-time
```

En el archivo `current-time` veremos la hora de cuando fue invocado el GET durante el build.

Luego cuando servimos `npm run preview` veremos que al ingresar a la http://localhost:4173/api/current-time  (notar que es otra url)  la hora presentada será la misma que vimos en la carpeta `output` aunque refresquemos las veces que sea.

De manera similar si tuviéramos una función fetch que genera un request a una API externa, esta sería guardada en la carpeta `output` y sería reutilizado ante cada request a ese endpoint. Cualquier cambio en la data externa no se vería reflejado en nuestra página.



Un punto importante del pre-rendering de API routes es que heredan valores por defecto de las páginas que fetchean datos de ellas.

Quitamos el pre-render en `server.js`:

```js
export async function GET(){
	console.log('current-time GET handler invoked');
    return new Respone(new Date().toLocaleTimeString());
}

```



Luego en `+page.js` dentro de la ULF del componente inicial:

```js
export const load = async ({fetch}) => {
	console.log('home page universal load function called');
    const response = await fetch('/api/current-time');
    const currentTime = await response.text();
    return {currentTime};
}

export const prerender = true;
exprot const csr = false;
```



En `+page.svelte` obtenemos la prop `data`:

```
<script>
	export let data;
</script>

<h1>Welcome to Sveltekit @ {data.currentTime}</h1>
```

