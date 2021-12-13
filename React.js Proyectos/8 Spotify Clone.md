# Spotify Clone

Basado en el [video](https://www.youtube.com/watch?v=3xrko3GpYoU) de Sonny Sangha

### Spotify API

El servicio de API de Spotify que usaremos nos permite acceder a los temas y controlar la reproducción **en la medida que tengamos Spotify abierto en algún dispositivo**. 

Al loguearnos nos entregan un access token cuyo tiempo de duración es de una hora, luego del cual tendremos que solicitar otro con el *refresh token*.

Para interactuar con la API de Spotify utilizaremos el paquete `spotify-web-api-node`

Debemos dirigirnos a https://developer.spotify.com/dashboard/ y presionar Log In. A continuación debemos hacer click en Create an App. Luego de esto le asignamos un nombre y una descripción y obtendremos el 

### Primeros pasos

* Crear proyecto de Next.js con Tailwind CSS

```shell
npx create-next-app -e with-tailwindcss spotify-clone
```



* Creación componentes funcionales con la extensión de ES7 snippets `_rfce` (guión bajo al inicio)




### Heroicons

Para los íconos utilizaremos heroicons.com 

```
npm install @heroicons/react
```

Luego importamos el ícono deseado de este modo:
  ```
  import {HomeIcon} from '@heroicons/react/outline'
  ```

  Para utilizarlo debemos especificar el alto y ancho cosa que hacemos usando clases de Tailwind:

```
<HomeIcon className="w-5 h-5"/>
```



### Tailwind 

* Instalar la extensión Tailwind CSS IntelliSense de Brad Cornes

* La clase `space-x-2` nos permite lograr un espaciado horizontal de los elementos hijos, podríamos usarlo también acompañado de `flex items-center `.

* Tailwind JIT nos permite crear *utility classes on the fly* por ejemplo si queremos un borde personalizado podemos hacerlo con `border-t-[0.1px]`



### Conceptos Sueltos

Node version manager: nvm nos permite utilizar una versión determinada de Node.



### NextAuth

Para la autenticación utilizaremos NextAuth que es una solución para implementar autenticación en aplicaciones de Next.js.

```
npm install next-auth
```

Debemos crear un archivo `[...nextauth].js` en `pages/api/auth`

Utilizamos el código indicado en la documentación sólo que utilizamos como provider a Spotify en lugar de Github

```
import NextAuth from "next-auth"
import SpotifyProvider from "next-auth/providers/spotify"

export default NextAuth({
  // Configure one or more authentication providers
  providers: [
    SpotifyProvider({
      clientId: process.env.NEXT_PUBLIC_CLIENT_ID,
      clientSecret: process.env.NEXT_PUBLIC_CLIENT_SECRET,
      authorization: 
    }),
    // ...add more providers here
  ],
})
```



Creamos una serie de variables de entorno en `.env.local`

```
NEXTAUTH_URL=http://localhost:3000
NEXT_PUBLIC_CLIENT_SECRET=
NEXT_PUBLIC_CLIENT_ID=
JWT_SECRET=some_super_secret_value
```



> La variable `NEXTAUTH_URL` cuando hagamos el deployment la cambiaremos a la dirección entregada por Vercel o nuestro hosting propio.
>
> Tanto `NEXT_PUBLIC_CLIENT_SECRET` como `NEXT_PUBLIC_CLIENT_ID` las obtenemos de  developers.spotify.com
>
> Utilizamos variables del tipo `NEXT_PUBLIC_...` ya que son valores que necesitaremos también en el cliente.
>
> **Cuando cambiemos variables de entorno debemos cerrar el servidor y volver a abrirlo**. Veremos un mensaje indicándonos que realizó esto: **Loaded env from (...) .env.local**



En la raíz del proyecto creamos un directorio `lib` donde colocamos las utilidades que vamos a necesitar. 

Instalamos spotify-web-api-node con

```
npm install spotify-web-api-node
```

Creamos en ese directorio un archivo `spotify.js` importamos este paquete e indicamos los scopes de los permisos que solicitaremos al hacer el login.



```jsx
import SpotifyWebApi from 'spotify-web-api-node';

const scopes = [
  'user-read-email',
  'playlist-read-private',
  'playlist-read-collaborative',
  'streaming',
  'user-read-private',
  'user-library-read',
  'user-top-read',
  // 'user-library-modify',
  'user-read-playback-state',
  'user-modify-playback-state',
  'user-read-currently-playing',
  'user-read-recently-played',
  'user-follow-read',
].join(",");

const params = {
  scope: scopes,
};

const queryStringParams = new URLSearchParams(params).toString();

const LOGIN_URL = `https://accounts.spotify.com/authorize?${queryStringParams}`;

```

> En primer lugar creamos un array con el scope y lo convertimos a un string separado por comas. Luego con `const queryStringParams= new URLSearchParams(params).toString()` convertimos el string en:
> ```js
'scope=user-read-email%2Cplaylist-read-private%2Cplaylist-read-collaborative%2Cstreaming%2Cuser-read-private%2Cuser-library-read%2Cuser-top-read%2Cuser-read-playback-state%2Cuser-modify-playback-state%2Cuser-read-currently-playing%2Cuser-read-recently-played%2Cuser-follow-read'
```

