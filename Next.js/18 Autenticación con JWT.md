# JWT y Rutas Protegidas

> :link: Basado en el [video de Fazt](https://youtu.be/zBbqrcvdJjQ) - [Repositorio](https://github.com/FaztWeb/nextjs-jwt-cookie)

```
npx create-next-app next-login-app
```

El propósito de este apartado es entender cómo generar un token, devolverlo al navegador mediante una cookie y de esta manera proteger rutas.

Si bien a modo de ejemplo utilizaremos un backend provisto por Next.js podríamos utilizar una REST API realizada por ejemplo con Node.js.

* En `pages` creamos un archivo `login.jsx` y en él un componente `LoginPage` con un formulario formado por un input de email y uno de password. Al presionar un botón haremos submit de ese formulario generando un POST a `/api/login`.

* En `api` creamos una carpeta `auth` y en ella un archivo `login.js`.



## Dependencias Proyecto

```
npm i axios jsonwebtoken jose cookie  
```

* axios
* jsonwebtoken
* jose
* cookie



## Página Principal

En `pages/index.js` mostraremos la página princiupal. Esta ruta luego será protegida.



## Página de Dashboard

En `pages/dashboard.js` mostraremos dashboard. Esta ruta también será protegida.



## Página de Login

En `pages/login.js` colocamos un formulario básico de ingreso de email y contraseña. Al momento de presionar el botón enviar realizamos un POST con dicha información a `/api/auth/login`.

```jsx
import axios from 'axios';
import React, { useState } from 'react'

function LoginPage() {
  const [credentials, setCredentials] = useState({
    email: '',
    password:''
  })

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const res = await axios.post('/api/auth/login', credentials)
      console.log(res.data)
    }
    catch (error) {
      if (error.response) {
        console.log(error.response.data)
      }
      else {
        console.error(error)
      }
    }
  }

  const handleChange = (e) => {
    setCredentials({...credentials, [e.target.name]:e.target.value})
  }

  return (
    <form onSubmit={handleSubmit}>
      <input name="email" type="email" placeholder="Email" onChange={handleChange} />
      <input name="password" type="password" placeholder="Password" onChange={handleChange} />
      <button type="submit">Login</button>
    </form>
  )
}

export default LoginPage
```



# Login en Backend

En `pages/auth/login.js` 



## Verificación de Datos

Una vez que el usuario ingresa los datos en el formulario. Los recibimos en el backend y lo primero que hacemos será chequear que estos sean correctos.

* En una situación real nos fijaremos en la base de datos que la contraseña y el correo sean los esperados.

* En caso de que los datos sean incorrecto enviamos un 401 y el mensaje de error apropiado. 

```jsx
import { sign } from 'jsonwebtoken'

export default function loginHandler(req, res) {
  const { email, password } = req.body;
  if (email === 'email@example.com' && password === 'admin') {
   
  }
  return res.status(401).json({ error: 'invalid email or password' })
}
```



## Generación de Token

A continuación vamos a generar un token con la biblioteca `jsonwebtoken`. 

```jsx
const token = jwt.sign({
	exp: Math.floor(Date.now()/1000)+(60*60*24*30)
	email,
	username: 'jnm'
}, 'secret', )
```

> En este caso para el payload suponemos que el `username` lo obtuvimos de la DB (mientras que el email lo recibimos en `req.body.email`).
>
> El segundo parámetro es el secreto que luego lo reemplazaremos por una variable de entorno.
>
> Con `exp` indicamos la expiración del token que en este caso lo hacemos de 30 días.

Luego podemos enviarlo en el cuerpo de la respuesta con `res.json({token})` pero esto obligaría al frontend a almacenarlo (por ejemplo en Local Storage).

```jsx
import { sign } from 'jsonwebtoken'

export default function loginHandler(req, res) {
  const { email, password } = req.body;

  if (email === 'email@example.com' && password === 'admin') {
    const token = sign({
      exp: Math.floor(Date.now() / 1000) + 60 * 60 * 24 * 30,
      email,
      username: 'jnm'
    }, 'secret');

    return res.json(token)
  }
  return res.status(401).json({ error: 'invalid email or password' })

}
```

> Con a partir del token recibido en el frontend podremos ingresar a jwt.io y verificar que el payload es el esperado.



## Headers y Cookies

> El objetivo es guardar el token en una cookie y esta en un header por lo que no será necesario guardarlo manualmente en el frontend.

Vamos a enviar el token en un **header** (cabecera con información extra) con `res.setHeader('Set-Cookie',token)` como buena práctica vamos a **serializarlo** utilizando la biblioteca `cookie`. La cookie también puede tener su tiempo de expiración independiente del que tenga el token y eso lo hacemos con un objeto de configuración. 



```jsx
const serialized = serialize('myTokenName', token, {
      httpOnly: true,
      secure: process.env.NODE_ENV === "production",
      sameSite: "strict",
      maxAge: 60 * 60 * 24 * 30,
      path: "/"
});
res.setHeader('Set-Cookie', serialized)
return res.status(200).json({
    message: "Login successful",
})
```

* Con `httpOnly: true` indicamos que será accedida mediante HTTP, lo que significa que en producción no será mostrada la cookie por el navegador en la consola utilizando las herramientas de desarrollo (si veremos el valor en el apartado Application/Cookies).
* Con `secure` indicamos que tiene que ser enviado por HTTPS (en desarrollo no tenemos un servidor con SSL, por lo que realizamos una configuración dependiente del entorno.
* `sameSite: 'strict'` como estamos trabajando con Next.js estamos en el mismo dominio, si tuviéramos un backend externo tendríamos que utilizar otra configuración como `sameSite: 'none'`.
* Con `maxAge: 1000*60*60*24*30` establezco el tiempo de expiración en **segundos**, en este caso de  30 días.
* Con `path: '/'` establezco la ruta desde la cual será entregada la cookie.




> En desarrolo `process.env.NODE_ENV` será `development` por lo tanto `secure: false`



```jsx
import { sign } from 'jsonwebtoken'
import { serialize } from 'cookie'

export default function loginHandler(req, res) {
  const { email, password } = req.body;

  if (email === 'email@example.com' && password === 'admin') {
    const token = sign({
      exp: Math.floor(Date.now() / 1000) + 60 * 60 * 24 * 30,
      email,
      username: 'jnm'
    }, 'secret');

    const serialized = serialize('myTokenName', token, {
      httpOnly: true,
      secure: process.env.NODE_ENV === "production",
      sameSite: "strict",
      maxAge: 60 * 60 * 24 * 30,
      path: "/"
    });
    res.setHeader('Set-Cookie', serialized)
    return res.json({
      message: "Login successful",
    });

  }
  return res.status(401).json({ error: 'invalid email or password' })

}
```

:pushpin: En caso de éxito retornamos un json con el campo `message` y en caso de error uno con el campo `error`.

En las DevTools en Expires/Max-Age podremos verificar que la fecha mostrada sea coherente con lo que configuramos.



## Manejo de Erorres Axios

Queremos mostrar el mensaje devuelto por el backend a través del catch y como ya dijimos es un json con un campo `error`.  Como no es un 200 lo recibiremos a través del catch del front.

En este caso tendremos la información deseada en `error.response.data.error`. Sin embargo habrá casos en que no vamos a tener el campo `response` por lo que tendremos que chequear primero que lo tenemos y sólo en ese caso podremos mostrar el campo.

```jsx
try {
      const res = await axios.get('/api/profile')
      console.log(res.data)
}
    
catch (error) {
	if (error.response) {
		console.log(error.response.data)
	}
	else {
		console.error(error)
	}
}
```



### Caso de uso completo

```jsx
try{
    
}
catch(error) {
    if (error.response) {
        // The request was made and the server responded with a status code
        // that falls out of the range of 2xx
        // console.log(error.response.data);
        // console.log(error.response.status);
        // console.log(error.response.headers);
    } else if (error.request) {
        // The request was made but no response was received
        // `error.request` is an instance of XMLHttpRequest in the 
        // browser and an instance of
        // http.ClientRequest in node.js
        console.log(error.request);
    } else {
        // Something happened in setting up the request that triggered an Error
        console.log('Error', error.message);
    }
    console.log(error.config);
}
```



## Redireccionamiento después de Login

Si en el frontend recibimos un 200 redireccionamos a `/dashboard` (para ello creamos un componente para tal fin)

```jsx
import axios from 'axios';
import React, { useState } from 'react'
import {useRouter} from 'next/router'

function LoginPage() {
  const router = useRouter()

  const [credentials, setCredentials] = useState({
    email: '',
    password:''
  })

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const res = await axios.post('/api/auth/login', credentials)

      if (res.status === 200) {
        router.push("/dashboard");
      }

    }
    catch (error) {
      if (error.response) {
        console.log(error.response.data)
      }
      else {
        console.error(error)
      }
    }
  }

  const handleChange = (e) => {
    setCredentials({...credentials, [e.target.name]:e.target.value})
  }

  return (
    <form onSubmit={handleSubmit}>
      <input name="email" type="email" placeholder="Email" onChange={handleChange} />
      <input name="password" type="password" placeholder="Password" onChange={handleChange} />
      <button type="submit">Login</button>
    </form>
  )
}

export default LoginPage
```



## Verificación Token

En las DevTools podremos verificar el token obtenido en el apartado Application/Cookies. Luego copiando ese valor en jwt.io podremos verificar que el payload también coincide con el que le pasamos desde el backend.

Como la cookie con el token ya está almacenada en el navegador en las subsiguientes peticiones la utilizaremos y podremos validarla desde el backend (la vamos a recibir en `req.cookies`). 

* En el `Dashboard` agregamos un botón que al hacer click hará un get a `/api/profile`.

  

* En `pages/api` creamos un archivo `profile.js` donde vamos a verificar que hemos recibido el token deseado en la cokie. No sólo queremos chequear que recibimos una cookie con el nombre esperado sino que esta haya sido generada con nuestro secreto, si no lo fue nos tirará error por lo que ubicamos la comprobación en un bloque try catch.

```jsx
import { verify } from "jsonwebtoken";

export default function profileHandler(req, res) {
  const { myTokenName } = req.cookies;

  if (!myTokenName) {
    return res.status(401).json({ error: 'no token' })
  }
  try {
    const user = verify(myTokenName, 'secret')
    return res.json({ email: user.email, username: user.username })
  }

  catch (error) {
    return res.status(401).json({ error: 'invalid token' })
  }
}
```

> La función `verify` funciona de manera asíncrona cuando proporcionamos una callback o de manera síncrona cuando no lo hacemos.
>
> Lo colocamos dentro de un bloque try catch ya que si el token es incorrecto queremos enviar un error apropiado y no que crashee el programa.



## Mostrar Info Perfil en Dashboard

Creamos una variable de estado `user` y mostramos esa información cuando hace click en el botón Get Profile:

```jsx
import React, { useState } from 'react'
import axios from 'axios'

function Dashboard() {
  const [user, setUser] = useState({
    email: '',
    username: ''
  })
  
  const getProfile = async () => {
    try {
      const res = await axios.get('/api/profile')
      setUser(res.data)
    }
    catch (error) {
      if (error.response) {
        console.log(error.response.data)
      }
      else {
        console.error(error)
      }
    }
  }
  
  return (
    <>
      <div>Dashboard</div>
      <div>{JSON.stringify(user)}</div>
      <button onClick={getProfile}>Get Profile</button>
    </>
  )
}

export default Dashboard
```



## Cerrar Sesión

Una forma de cerrar sesión desde las DevTools es yendo a Application/Cookies y sobre http://localhost:3000 hacer click derecho y elegir Clear.

Queremos incorporar un botón de Logout.

```jsx
import React, { useState } from 'react'
import axios from 'axios'
import { useRouter } from 'next/router'

function Dashboard() {
  const router = useRouter();
  const [user, setUser] = useState({
    email: '',
    username: ''
  })
  const getProfile = async () => {
    try {
      const res = await axios.get('/api/profile')
      setUser(res.data)
    }
    catch (error) {
      if (error.response) {
        console.log(error.response.data)
      }
      else {
        console.error(error)
      }
    }
  }
  const logout = async () => {
    try {
      const res = await axios.get('/api/auth/logout')
    }
    catch (error) {
      console.error(error)
    }
    router.push('/login')
  }

  return (
    <>
      <div>Dashboard</div>
      <div>{JSON.stringify(user)}</div>
      <button onClick={getProfile}>Profile</button>
      <button onClick={logout}>Logout</button>
    </>
  )
}

export default Dashboard
```



En `pages/api` creamos un archivo `logout.js`

Lo que hacemos es establecer la misma cookie pero con un valor de tiempo de expiración de 0 y con un valor nulo.

> En este caso a la hora de usar la función verify no me interesa extraer el email y username sino que el simple hecho de que no haya fallado (y por ende entrar al catch) ya me da la pauta de que fue verificado como correcto.

```jsx
import { serialize } from "cookie"

export default function logoutHandler(req, res) {

  const { myTokenName } = req.cookies;

  if (!myTokenName) {
    return res.status(401).json({ error: 'Not logged in' })
  }

  const serialized = serialize('myTokenName', null, {
    httpOnly: true,
    secure: process.env.NODE_ENV === "production",
    sameSite: "strict",
    maxAge: 0,
    path: "/"
  });
  res.setHeader('Set-Cookie', serialized)
  return res.status(200).json({
    message: "Logout successful",
  })
}
```



## Redireccionamientos

Queremos que al hacer logout nos redireccione a la página principal y lo hacemos con `useRouter`.

* En el caso de que nos estemos logueando  solo debemos redireccionar a `/dashboard` si devolvió un 200.

* En el caso de que estemos haciendo logout quremos que redireccione a `/login`. Debemos colocar el post dentro de un `try/catch` y manejar el error pero en ambos casos (éxito y fracaso) redireccionar.

Cuando trabajemos con una **biblioteca de manejo de estado** como ContextAPI o Redux tendremos las funciones para `login` y `logout` dentro de él y podríamos guardar los datos del perfil de usuario y si está autenticado o no.



## Protección de Rutas

Si bien tenemos un redireccionamiento cuando nos deslogueamos, si el usuario no autenticado ingresa a `/dashboard` podría acceder al contenido que no queremos que vea.

En este caso debemos hacer uso del Middleware de Next.js que se ejecuta antes del código propio de la página y tendremos que comprobar si tiene un token o un determinado rol y sólo en ese caso continuar y en caso contrario nos redireccionará a otro lado.

En la [documentación](https://nextjs.org/docs/advanced-features/middleware) podremos entender cómo debe funcionar el middlware y también ver [el orden de ejecución](https://nextjs.org/docs/advanced-features/middleware#matching-paths).

Creamos un archivo `middleware.js` en la raíz (debe estar al mismo nivel que `pages`) 

El código que coloquemos allí dentro se ejecutará cada vez que el navegador solicite un archivo no sólo páginas, sino también javascript, favicon, css, etc.



```jsx
import { jwtVerify } from 'jose';
import { NextResponse } from 'next/server'

export async function middleware(req) {
  const jwt = req.cookies.get('myTokenName');

  if (!jwt) {
    return NextResponse.redirect(new URL('/login', req.url))
  }
  try {
    const { payload } = await jwtVerify(jwt, new TextEncoder().encode('secret'))
    console.log(payload);
    return NextResponse.next();
  }
  catch (error) {
    console.error(error)
    return NextResponse.redirect(new URL('/login', req.url))
  }
}

export const config = {
  matcher: ['/dashboard', '/']
}
```

:triangular_flag_on_post: Notar que para verificar el token no utilizamos `jsonwebtoken` sino que usamos la biblioteca `jose`. En caso de haber utilizado `import {verify} from 'jsonwebtoken'` obtendríamos un error por motivos relacionados con Edge o CDN. Si bien jose realiza exactamente la misma tarea lo hace de un modo distinto por eso es necesario en lugar de trabajar con `verify(myTokenName, 'secret')` hacerlo con  `await jwtVerify(jwt, new TextEncoder().encode('secret')`



* Con `request.url` obtenemos un string que para la ruta que nos interesa, en desarrollo será http://localhost:3000/dashboard. Tendremos que tener cuidado pues la primera parte cambiará en producción.

* Con `request.nextUrl` obtenemos un objeto  siendo útil para nostotros el `pathname` que será `/dashboard`.



* `request.cookies.get()` devolverá undefined cuando no tengamos la cookie en el navegador.

* En caso de que no tengamos token o que el token sea inválido redireccionamos a /login.
* Con `NextResponse.next();` continuamos con la página que estamos visitando. Recordemos que cuando hacemos login y obtenemos un 200, nos redirecciona a dashboard y ahi se ejecuta este middleware.



## Proteger Múltiples Rutas

Si bien podríamos utilizar un arreglo con las rutas a proteger, Next.js dispone de una característica llamda Matcher en la cual podremos indicar todas las rutas que queremos proteger.



```jsx
import { jwtVerify } from 'jose';
import { NextResponse } from 'next/server'

export async function middleware(req) {
  const jwt = req.cookies.get('myTokenName');

  if (!jwt) {
    return NextResponse.redirect(new URL('/login', req.url))
  }
  try {
    const { payload } = await jwtVerify(jwt, new TextEncoder().encode('secret'))
    console.log(payload);
    return NextResponse.next();
  }
  catch (error) {
    console.error(error)
    return NextResponse.redirect(new URL('/login', req.url))
  }
}

export const config = {
  matcher: ['/dashboard', '/']
}
```



Si quiero proteger **sólo una ruta** puedo hacerlo con:

```jsx
export const config = {
    matcher: '/dashboard'
}
```

Si quiero proteger también `/admin/profile` y `/admin/user` no hace falta que ingrese ambas páginas en el listado sino que puedo poner directamente `/admin/:path*` y representará a todas las subrutas.

```jsx
export const config = {
    matcher: ['/dashboard', '/admin/:path*']
}
```

