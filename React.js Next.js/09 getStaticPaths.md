# `getStaticPaths`

Cuando construyamos nuestra aplicación para **producción** si trabajamos sólo con **rutas estáticas** Next.js generará un sitio estático basado en los componentes que tendremos dentro de `pages` por lo que tendremos archivos HTML y bundles JavaScript, por ejemplo `index.html`, `about.html`, `ninjas.html`, etc. 

Sin embargo, si  tenemos además **rutas dinámicas** como por ejemplo una página de detalles de usuarios, productos o algo similar, como Next.js necesita pre-renderizar deberá generar una página y asignarle una ruta para cada elemento pero para ello necesitaría saber qué datos tenemos disponibles y esta información proviene de una **fuente externa** (una API).

Es por ello que en esa ruta dinámica supongamos `[id].js` utilizamos `getStaticPaths` en la cual vamos a hacer el *fetch* para obtener los datos externos gracias a los cuales conoceremos los **ids** con los cuales Next.js creará las páginas y rutas.

Así como `getStaticProps` retorna un objeto `props` con `getStaticPaths` debemos retornar un objeto `paths` que sea un array de objetos con la propiedad `params` que a su vez también es un objeto con la propiedad `id`.
Como estamos retornando como segundo parámetro a la propiedad `fallback: false` esto significa que si un usuario ingresa a una ruta que no existe mostraremos la página 404.

```jsx
export const getStaticPaths = async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/users');
  const data = await res.json();

  // map data to an array of path objects with params (id)
  const paths = data.map(ninja => {
    return {
      params: { id: ninja.id.toString() }
    }
  })

  return {
    paths,
    fallback: false
  }
}

const Details = () => {
  return (
    <div>
      <h1>Details Page</h1>
    </div>
  );
}

export default Details;
```

> Notar que utilizamos `ninja.id.toString()` pues de lo contrario será un entero y queremos que sea un string.



Gracias a esto cuando armemos nuestra aplicación para producción Next.js se fijará en este array de objetos y armará una página HTML y una ruta usando los parámetros que le especificamos (el `id` en nuestro caso).

## Obtener detalles de página con `getStaticProps`
Para obtener los datos de cada una de estas páginas de detalles utilizaremos `getStaticProps` y esta función se ejecutará tantas veces como elementos tenga el array retornado por `getStaticPaths`. `getStaticProps` recibe como parámetro `context` y en `context.params.id` tendremos el valor del id para obtener los datos de esa página en particular.

```jsx
export const getStaticPaths = async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/users');
  const data = await res.json();

  // map data to an array of path objects with params (id)
  const paths = data.map(ninja => {
    return {
      params: { id: ninja.id.toString() }
    }
  })

  return {
    paths,
    fallback: false
  }
}

export const getStaticProps = async (context) => {
  const id = context.params.id;
  const res = await fetch('https://jsonplaceholder.typicode.com/users/' + id);
  const data = await res.json();

  return {
    props: { ninja: data }
  }
}

const Details = ({ ninja }) => {
  return (
    <div>
      <h1>{ ninja.name }</h1>
      <p>{ ninja.email }</p>
      <p>{ ninja.website }</p>
      <p>{ ninja.address.city }</p>
    </div>
  );
}

export default Details;
```

Cuando hacemos `npm run build` nos creará una carpeta `.next` y si navegamos dentro de ella a `pages/ninjas` veremos que nos ha creado una página para cada usuario. 