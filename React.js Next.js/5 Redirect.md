# Redirect
En ocasiones queremos redireccionar a un usuario hacia alguna página por algún motivo.
Esto puede ser útil por ejemplo luego de que el usuario envía un formulario.

A continuación se muestra un redireccionamiento o unos segundos después de mostrar la página 404. Utilizamos el hook `const router = useRouter()` de next y luego en el `useEffect` utilizamos un timer con `setTimeOut` y con `router.push('/')` hacemos el redireccionamiento a `/`. 

Si quisiéramos navegar hacia atrás en la historia podríamos utilizar `router.go(-1)` o bien `router.go(1)` si quisiéramos navegar hacia adelante.

```jsx
import Link from 'next/link'
import { useEffect } from 'react'
import { useRouter } from 'next/router'

const NotFound = () => {
  const router = useRouter()

  useEffect(() => {
    setTimeout(() => {
      // router.go(-1)
      // router.go(1)
      router.push('/')
    }, 3000)
  }, [])

  return (
    <div className="not-found">
      <h1>Ooops...</h1>
      <h2>That page cannot be found :(</h2>
      <p>Going back to the <Link href="/"><a>Homepage</a></Link> is 3 seconds...</p>
    </div>
  );
}
 
export default NotFound;
```