# Pre-rendering

Una de las características más destacadas de Next.js es el pre-rendering. El pre-rendering nos permite que los componentes de React sean renderizados con los datos necesarios en HTML en el servidor y luego sean enviados al navegador. **A partir de ese momento la página se comporta como una SPA** y el routing lo podemos manejar del lado cliente en el navegador.  

Las aplicaciones React creadas con create-react-app utilizan **client side rendering**, es decir que los componentes React son renderizados en HTML en el navegador. Si bien en pantalla y en las dev tools veremos los distintos elementos HTML, si abrimos el código fuente sólo veremos un `<div id="root"></div>`. 

En las aplicaciones React creadas con Next.js en cambio, utilizan pre-rendering por lo que si analizamos el código fuente veremos todos los elementos HTML. Esto sucede debido a que Next.js genera el HTML para todas las páginas de manera anticipada en lugar de que esto deba ser realizado del lado servidor.

En aplicaciones CRA cuando se sirve la página sólo tenemos ese `div` vacío y luego cuando se carga y ejecuta el código JavaScript en el navegador son creados y montados los elementos HTML en el DOM. Este proceso se conoce como **hydration** o hidratación. En aplicaciones Next.js cuando se sirve la página tenemos los elementos HTML y los datos y gracias a JavaScript logramos que la página sea interactiva.



> * Algo equivalente a analizar el código fuente es dirigirnos a Network y analizar si la respuesta tiene o no los elementos HTML.
> * Si en una aplicación creada con CRA deshabilitamos JavaScript veremos una página en blanco ya que se construye en el cliente. Sin embargo, si utilizamos Next.js como hacemos un pre-rendering, podremos verla sin tener JavaScript habilitado.  
> * Cuando utilizamos `useEffect()` es el navegador quien realiza el fetch de los datos (inicialmente recibimos una página en blanco lo cual podemos constatar en Network yendo a Preview), mientras que en Next.js gracias a `getStaticProps` y `getServerSideProps` obtendremos los datos en el servidor por lo que recibimos la página con información.

Next.js nos ofrece SSR *(server side rendering)* , SSG *(static site generation)* y ISR (*Incremental static regeneration*).



## Ventajas Pre-rendering

El pre-rending nos aporta dos ventajas importantes:

* Mejoras en términos de  performance

Con aplicaciones creadas con **create react app** el usuario verá una pantalla en blanco durante unos segundos al actualizar la página (mientras se ejecuta el código JavaScript, hace el fetching de datos de una API externa y luego renderiza la UI), mientras que con Next.js verá el contenido de manera inmediata. 

Decimos que Next.js tiene un **FCP** (*first contentful paint*) más rápido. En este [artículo](https://web.dev/user-centric-performance-metrics/) encontramos más información al respecto. 



* Mejoras en términos de SEO (search engine optimization)

  Esto es importante en sitios como blogs y e-commerce (no afectando a sitios que para acceder al contenido es necesario loguearse). Gracias al pre-renderizado el motor de búsqueda o *search index* podrá acceder al contenido, mientras que en aplicaciones realizadas con CRA sólo podrá acceder a un div con el `id="root"`. Si bien Google mejoró el proceso de *crawling* en páginas donde el contenido es generado dinámicamente, es mejor el caso en que ya tenemos el contenido generado.



# SSR, SSG y ISR

Con Next.js podemos realizar SSR, SSG, ISR.

Podemos ver más información en esta [página](https://www.netlify.com/blog/2020/12/02/next.js-should-i-use-ssr-or-ssg/) 

## Server Side Rendering

SSR es cuando los componentes de React son renderizados en páginas HTML en el servidor, luego de un request del navegador es decir  **at run-time**. 
Tenemos la **ventaja** de que los datos estarán siempre actualizados.



## Static Site Generation

Static Generation es un tipo de pre-rendering donde las páginas HTML son generadas *at build-time*.

## Inscremental Static Regeneration

ISR es una implementación híbrida que nos permite actualizar contenido estático sin la necesidad de hacer *rebuild* de todo el sitio cuando cambiamos algo. Es una alternativa ideal para aprovechar las ventajas de las páginas estáticas y a la vez escalar a millones de páginas.



> Resumen de [Guía de ISR por Lee Robinson](https://www.smashingmagazine.com/2021/04/incremental-static-regeneration-nextjs/). 
>
> Trabajar con SSG, nos aporta sitios estáticos pre-renderizados que pueden ser subidos a CDN y disponibles globalmente en poco tiempo y además tenemos contenido que puede ser accedido muy rápido e indexado también rápidamente a los crawlers. Sin embargo tiene algunas desventajas. En sitios estáticos de gran escala, podrá llevar horas esperar el *build* del sitio. En caso de duplicar la cantidad de páginas también duplicaría el tiempo requerido. Es por esto que fue necesaria una solución híbrida como ISR. 
>
> Considerando el caso de un equipo donde el código está desacoplado del contenido ya que usan un Headless CMS, el cambio de un precio en un artículo demandaría el *rebuild* de todo el sitio lo cual podría llevar horas. ISR apunta a interpretar donde fueron los cambios y realizar una actualización incremental sin la necesidad de hacer un *rebuild* entero.



## Tipos de Pre-rendering

Next.js soporta dos tipos de pre-rendering que son Static Generation y Server Side Rendering.

> 

### Static Generation

SSG es cuando los componentes de React son renderizados en páginas HTML junto con todos los datos que constituyen el contenido, en el momento de desplegar la aplicación, es decir **at build-time**. Tenemos la ventaja de que el TTFB (*time to first byte*) es más rápido.

Este es el método recomendado siempre que sea posible ya que la página puede ser construida una vez, cacheada por un CDN y servida al cliente de manera casi instantánea. Esto como dijimos representa una mejora en términos de performance y SEO siendo ideal para blogs, sitios de e-commerce, páginas de documentación, etc.

Next.js por default hace el pre-renderizado de todas las páginas de neustra aplicación, por lo que el contenido HTML de cada página será generado estáticamente cuando hacemos el build de la aplicación. Es por esto que incluso en la aplicación demo de Next.js cuando vemos el código fuente ya tenemos todos los elementos HTML.

En **producción** vamos a deployar el build optimizado que se genera al ejecutar el comando build. En ese momento las páginas son pre-renderizadas (solo una vez y reutilizado ante cada request). Mientras que en **desarrollo** las páginas son pre-renderizadas ante cada request para asegurarnos mostrar el contenido actualizado cada vez.  



#### Static Generation con Fetching de datos

Este proceso consiste en generar el HTML luego de obtener datos de una API externa, una base de datos local o mismo del filesystem.



##### `getStaticProps`

En aplicaciones React donde utilizamos por ejemplo **create-react-app** hacemos el fetch de datos de una API en el  `useEffect` hook es decir que e request lo estamos haciendo en el navegador. Sin embargo al utilizar **Next.js** como los componentes ya están pre-renderizados cuando llegan al navegador por lo que idealmente vamos queremos obtener los datos antes para que ya estén en el template. 

Es posible hacer exportando una función async especial provista por Next.js llamada `getStaticProps` que se ejecuta antes de que se renderice el componente, obtiene los datos mediante el `fetch` y luego debe retornar un objeto que se los pasa via `props` al componente que luego será renderizado.

En `pages` creamos el archivo `users.js` y queremos generar estáticamente la página con el contenido de los usuarios que obtendremos de una API externa.

> Como fake API es posible utilizar https://jsonplaceholder.typicode.com/ y en particular el endpoint https://jsonplaceholder.typicode.com/users

```jsx
export const getStaticProps = async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/users');
  const data = await res.json();

  console.log(data); //esto lo veremos en la terminal y no en la consola del navegador
    
  return {
    props: { users: data }
  }
}

const UserList = ({ users }) => {
  console.log(users)

  return (
    <div>
      <h1>All Users</h1>
      {users.map(user => (
        <div key={user.id}>
        	<h3>{ user.name }</h3>
        </div>
      ))}
    </div>
  );
}
 
export default UserList;
```



Si analizamos el código fuente veremos que tanto los elementos HTML como los datos de los usuarios están presentes en él.



> Una implementación más organizada para producción sería crear un componente `User` que recibe como props el objeto `user`. Para ello deberíamos crear una carpeta `components` pero no queremos acceder a las características de de `pages` (file-system routing o acceso a `getStaticProps`), debemos crearla fuera esta carpeta. Por lo tanto creamos la carpeta en la raíz y dentro de ella el archivo `user.js`. 
>
> Este tipo de componentes reciben el nombre de *presentation components*, en contraposición de los **page components** que son aquellos que queremos exponer como una ruta.
>
> En aplicaciones react se suele utilizas para este tipo de componentes pero en Next.js se suele usar kebab-case.



Algunas particularidades de `getStaticProps`

*  Se ejecuta en el servidor  *at build-time* (esto en producción, mientras que en desarrollo se ejecuta ante cada request) y no en el navegador. Es más, el código escrito dentro de `getStaticProps` ni siquiera será incluído en el JS bundle que es enviado al navegador.

  > Es por eso que el `console.log(data);` lo vemos en la terminal y no en la consola del navegador. 

* Es posible escribir código de lado servidor como ser consultar a una base datos o acceder al file system usando el módulo `fs`. Como estamos ejecutando código en el servidor, estaremos trabajando en un **entorno seguro**. Esto significa que podemos conectarnos a la DB usando credenciales sin que sean visibles para el usuario o utilizar API keys que no llegarán al navegador. 

* Sólo es posible usarla en una página y no en componentes normales.

* Debe retornar un objeto con una propiedad `props` que también es un objeto.

  ```jsx
  return {
      props: { users: data }
    }
  ```



#### Static Generation Builds

Hasta ahora analizamos Static Generation en modo desarrollo, vamos a ver la salida que obtenemos al hacer el build optimizado para produción. 

En primer lugar borramos la carpeta `.next` que se genera cuando ejecutamos `npm run dev`. Luego ejecutamos `npm run build` y la carpeta de salida también será `.next` pero con distinto contenido.



##### Salida de la terminal

En la terminal veremos información acerca de cada ruta de nuestra aplicación:

| Page | Size | First Load JS |
| :--: | :--: | :-----------: |

**Page** se refiere a la ruta
**Size** al peso de los assets descargados al navegar la página del lado cliente.
**First Load JS** peso de los assets descargados del servidor al visitar la página.



Luego veremos un apartado **First Load JS shared by all** donde veremos el código que es descargado independientemente de la ruta que estemos:

* `chunks/framwork.923004.js`: código de `node_modules` como ser React.
* `chunks/main.477735.js` código relacionado con las páginas y componentes.
* `chunks/pages/_app.2767dd.js` código de `_app.js` que es un componente que envuelve todas las páginas de la aplicación.
* `chunks/webpack.972781.js`: runtime código de webpack
* `css/01db3197b29cb52779ce.css`: consiste en el css de `global.css`.

> La columna de **First Load JS** será pintada de colores verde, rojo o amarillo de acuerdo a la performance arrojada por el tamaño de estos archivos.



Luego veremos información del tipo de pre-rendering efectuado en cada página que hemos creado, pudiendo caer en las siguientes categorías:

* (Server): server-side renders at runtime (uses `getInitialProps` or `getServerSideProps`)
* (Static): automatically rendered as static HTML (uses no initial props).
* (SSG): automatically generated as static HTML + JSON (uses `getStaticProps`)
* (ISR): incremental static regeneration (uses revalidate in `getStaticProps`)



Las páginas en las que utilizamos `getStaticProps`  para hacer fetching de datos externos (nuestro ejemplo `/users`) caerán en la tercera categoría SSG mientras que aquellas páginas en las que no, caerán en la segunda categoría.

Además veremos que Next.js nos crea múltiples archivos de JavaScript en lo que se conoce como **code splitting** y veremos la estrategia también de **hash assets** para solucionar el tema de caché.

:warning: Se recomienda realizar curso optimización web

Si dentro del navegador vamos a Network en las DevTools veremos que Next.js nos hace el **code splitting** y cada página tiene su chunk específico `2d57ec00e72a16...` además también veremos otros paquetes que son reutilizados por todas las páginas como `webpack-c2126667a5f965...` o `framework.c6faa22799416a6...`. Con esto notamos que que Next.js ya ha realizado optimizaciones de manera automática.

> En modo de **desarrollo** también veremos este proceso ya que cada página tiene su propio *javascript bundle* que obtenemos cuando la navegamos por primera vez.  

Si hacemos click derecho en "Ver código fuente" (o bien en Response en Network) veremos que el contenido principal de la página ya viene incluido en el HTML de la respuesta. Esto significa que no hay *client side rendering* y todo lo que estamos viendo ya viene del servidor. **Esto es muy importante en términos de SEO** (search engine optmization) como ya todo viene prerenderizado es mucho más amigable para los motores de búsqueda y también a la hora de compartir la página en redes sociales ya que la información estará disponible.



##### Salida en carpeta `.next`

En la carpeta `.next` nos encontramos con varias carpetas, nos centraremos en `server` y `static`. 

En carpeta`server/pages` veremos distintos archivos y unos de ellos son `users.html` y `users.json`, esto se corresponde con lo visto anteriormente donde nos indicaba que la ruta `/users` tenía SSG con HTML + JSON. En cambio `index.html` , `404.html` como vimos en el output de la terminal al ser una páginas estáticas sólo tenemos un archivo HTML.

También en `server` veremos una serie de archivos `.js` que no serán enviados al navegador y son una transformación de las páginas y componentes de la aplicación. La hidratación en cambio se produce con los archivos de la carpeta `static/chunks/pages` donde tenemos archivos de javascript que serán enviados al navegador y tendrán el código para **hidratar** la página y hacerla interactiva. También veremos otros archivos que no tienen relación con el código escrito `framework-92300432...`, `main-47...`, `polyfills-a5...`, `webpack...`



##### Ejecutar la aplicación

Luego de ejecutar `npm run build` con el comando`npm start` podremos servir al navegador los archivos de la aplicación recién construida. 

A partir de ahora nos planteamos dos escenarios. El primero será suponiendo que no tenemos ningún `Link` en `index.js` que nos permita navegar a `/users` y el segundo caso suponiendo que sí tenemos ese enlace.

**Caso 1**: Cuando no tenemos ningún `Link` en el `index.js` a `/users` 

>  En el navegador y **con las DevTools abiertas** hacer click derecho en el icono de actualizar y elegir **Vaciar caché y volver a cargar la página de manera forzada**.



Si ingresamos a `localhost:3000/` y vamos a Network veremos los recursos que son descargados. En primer lugar vemos `localhost` que representa el archivo `index.html` que vimos en la carpeta `.next/server/` y el chunk `index.24...` También veremos archivos relacionados con css, framework, webpack, _app, etc pero no vemos nada relacionado con `/users` ya que Next.js sabe que no hay forma de navegar hacia esa ruta desde `/`. 

Si en cambio ingresamos a `/users` descargaremos `users` que representa el `users.html` en `/server` y el chunk `users-35aa9...` para el javascript de lado cliente.



**Caso 2**: Cuando tenemos un`Link` en `index.js` a `/users` 

Supongamos que en `index.js` tenemos un enlace usando el componente `Link` a `/users`

```jsx
<Link href="/users">
	<a>Users</a>	
</Link>
```

Como consecuencia de eso cuando ingresemos a `/` (si hicimos otra vez el build debemos limpiar el caché y recargar de manera forzada nuevamente) veremos que fueron descargados dos chunks que comienzan con `users-35a...` y `users.json`. Gracias a esto cuando naveguemos a `/users` el proceso será instantáneo **sin la necesidad de nuevos requests al servidor** (no estamos descargando `users.html`).



Esta **optimización** se conoce como **Link Prefetching** y es el comportamiento default en pre-rendering en Static Generation y consiste en que para cualquier componente `Link` que tengamos en el viewport (inicialmente o al scrollear) se realizará el pre-fetching incluyendo los datos correspondientes.



El recurso `users.html` sólo es descargado cuando navegamos directamente a `/users`, mientras que si navegamos desde el root descargamos el `users-35...` y `users.json` y con este JavaScript y JSON construimos la UI del lado cliente. Cuando una página con `getStaticProps` es pre-renderizada *at build-time* además del HTML genera un JSON con el resultado de ejecutar `getStaticProps`. Este JSON será usado en el *client side routing* cuando usemos `next/link` o `next/router`.



#### Incremental Static Regeneration (ISR)

#### Fetching con parámetros dinámicos



### Server Side Rendering

#### Server Side Rendering con fetching de datos



## Client Side Data Fetching

### Pre-rendering & Client Side Data Fetching