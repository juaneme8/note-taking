# `getInitialProps`
`getInitialProps` nos permite tener SSR y además realizar una obtención inicial de datos. Se trata de una función`async` que realizará el fetch de datos y luego retornará las props al componente en cuestión.

A partir de la versión Next.js 9.3 se recomienda utilizar `getStaticProps` o `getServerSideProps`

```jsx
function Page({ stars }) {
  return <div>Next stars: {stars}</div>
}

Page.getInitialProps = async (ctx) => {
  const res = await fetch('https://api.github.com/repos/vercel/next.js')
  const json = await res.json()
  return { stars: json.stargazers_count }
}

export default Page
```

Cuando ingresamos a una ruta determinada, Next.js se fija si el componente tiene el método `getInitialProps` y en ese caso ejecuta ese código en el servidor antes de hacer el pre-rendering (al ser asíncrono esperará hasta que esté disponible el resultado)  y el valor retornado por este método lo recibe el componente via props. 

Importante: si bien en la carga inicial de la página el código se ejecutará en el servidor únicamente, si luego navegamos a otra ruta (usando `next/link` o `next/router`) que tiene `getInitialProps` este código será ejecutado del lado del cliente pues debo garantizar el funcionamiento como una SPA.

Si nos fijamos en el código fuente veremos que a pesar de que tenemos renderizado el contenido devuelto en html, también tenemos un `<script>` con los datos retornados en `getInitialProps`. Este paso se conoce como **hidratación** (el html estático pasa a ser dinámico, es decir a tener vida) porque a pesar de que ya tenemos renderizada la página en el servidor, para que sea capaz el cliente de renderizar exactamente lo mismo.

`getInitialProps` sólo funciona en componentes de tipo página.

`getInitialProps` recibe un argumento llamado `context` con las propiedades `pathname`, `query` entre otras.