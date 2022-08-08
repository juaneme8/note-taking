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
npm i jsonwebtoken
npm i cookie
npm i jose
```





## Verificación de Datos

Una vez que el usuario ingresa los datos en el formulario. Los recibimos en el backend y lo primero qeu hacemos será chequear que estos sean correctos.



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



## Headers y Cookies

> El objetivo es guardar el token en una cookie y esta en un header por lo que no será necesario guardarlo manualmente en el frontend.

Vamos a enviar el token en un **header** (cabecera con información extra) con `res.setHeader('Set-Cookie',token)` como buena práctica vamos a **serializarlo** utilizando la biblioteca `cookie`. La cookie también puede tener su tiempo de expiración independiente del que tenga el token y eso lo hacemos con un objeto de configuración. 



```jsx
const serialized = serialize('myTokenName', token, {
	httpOnly: true,
	secure: process.env.NODE_ENV === 'production',
    sameSite: 'strict'
});
setHeader('Set-Cookie', serialized)
return res.json('login successfully')
```

* Con `httpOnly: true` indicamos que será accedida mediante HTTP, lo que significa que en producción no será mostrada la cookie por el navegador en la consola utilizando las herramientas de desarrollo (si veremos el valor en el apartado Application/Cookies).

* Con `secure` indicamos que tiene que ser enviado por HTTPS (en desarrollo no tenemos un servidor con SSL, por lo que realizamos una configuración dependiente del entorno.

* `sameSite: 'strict'` como estamos trabajando con Next.js estamos en el mismo dominio, si tuviéramos un backend externo tendríamos que utilizar otra configuración como `sameSite: 'none'`.

* Con `maxAge: 1000*60*60*24*30` establezco el tiempo de expiración en segundos, en este caso de  30 días.

* Con `path: '/'` establezco la ruta desde la cual será entregada la cookie.

  

Notar que esto lo vamos a querer hacer sólo si los datos de autenticación son correctos, en caso contrario simplemente `return res.status(401).json({error: 'invalid email or password'})`.



## Verificación Token

En las DevTools podremos verificar el token obtenido en el apartado Application/Cookies. Luego copiando ese valor en jwt.io podremos verificar que el payload también coincide con el que le pasamos desde el backend.

Como la cookie con el token ya está almacenada en el navegador en las subsiguientes peticiones la utilizaremos y podremos validarla desde el backend (la vamos a recibir en `req.cookies`). 

* En `pages` creamos un archivo `dashboard.js` y en el un componente `Dashboard` con un botón que al hacer click hará un get a `/api/profile`.
* En `pages/api` creamos un archivo `profile.js` donde vamos a verificar que hemos recibido el deseado en la cokie.

```jsx
const {myTokenName} = req.cookies;

if(!myTokenName){
    return res.status(401).json({error: 'no token'});
}

try{
	const user = verify(myTokenName, 'secret')
	console.log(user)
    return res.json({email: user.email, username: user.username})
}
catch(error){
	return res.status(401).json({error: 'invalid token'})
}

```

> La función `verify` funciona de manera asíncrona cuando proporcionamos una callback o de manera síncrona cuando no lo hacemos.
>
> Lo colocamos dentro de un bloque try catch ya que si el token es incorrecto queremos enviar un error apropiado y no que crashee el programa.



## Cerrar Sesión

Una forma de cerrar sesión desde las DevTools es yendo a Application/Cookies y sobre http://localhost:3000 hacer click derecho y elegir Clear.

Queremos incorporar un botón de Logout

Lo que hacemos es establecer la misma cookie pero con un valor de tiempo de expiración de 0 y con un valor nulo.

En este caso a la hora de usar la función verify no me interesa extraer el email y username sino que el simple hecho de que no haya fallado (y por ende entrar al catch) ya me da la pauta de que fue verificado como correcto.

En `pages/api` creamos un archivo `logout.js`

```jsx
import { serialize } from "cookie";

export default function logoutHandler(req, res) {
  const { myTokenName } = req.cookies;
  if (!myTokenName) {
    return res.status(401).json({ error: "Not logged in" });
  }
  try{
    verify(myTokenName, 'secret')
      
	const serialized = serialize("myTokenName", null, {
    httpOnly: true,
    secure: process.env.NODE_ENV === "production",
    sameSite: "strict",
    maxAge: 0,
    path: "/",
    });

    res.setHeader("Set-Cookie", serialized);
    return res.status(200).json({
    	message: "Logout successful",
    });
  }
  catch(error){
	return res.status(401).json({error: 'invalid token'});
  }

}
```



## Redireccionamiento

Queremos que al hacer logout nos redireccione a la página principal y lo hacemos con `useRouter`.



En el caso de que estemos haciendo logout quremos que redireccione a /login. Debemos colocar el post dentro de un try catch y manejar el error pero en ambos casos (éxito y fracaso) redireccionar.

En el caso de que nos estemos logueando  solo debemos redireccionar a /dashboard si devolvió un 200.

Cuando trabajemos con una **biblioteca de manejo de estado** como ContextAPI o Redux tendremos las funciones para login y logout dentro de él y podríamos guardar los datos del perfil de usuario y si está autenticado o no.



## Protección de Rutas

Si bien tenemos un redireccionamiento cuando nos deslogueamos, si el usuario no autenticado ingresa a `/dashboard` podría acceder al contenido que no queremos que vea.

En este caso debemos hacer uso del Middleware de Next.js que se ejecuta antes del código propio de la página y tendremos que comprobar si tiene un token o un determinado rol y sólo en ese caso continuar y en caso contrario nos redireccionará a otro lado.

En la [documentación](https://nextjs.org/docs/advanced-features/middleware) podremos entender cómo debe funcionar el middlware y también ver [el orden de ejecución](https://nextjs.org/docs/advanced-features/middleware#matching-paths).

Creamos un archivo `middleware.js` en la raíz (debe estar al mismo nivel que `pages`) 

El código que coloquemos allí dentro se ejecutará cada vez que el navegador solicite un archivo no sólo páginas, sino también javascript, favicon, css, etc.



```jsx
import {NextResponse} from 'next/server'
import {jwtVerify} from 'jose'

export async funcion middleware(request){
	console.log(request.url); 
    console.log(request.nextUrl.pathname) 
	
    const jwt = request.cookies.get('myTokenName'); 
    
    if(request.nextUrl.pathname.includes('/dashboard')){
        if(jwt === undefined){
            return NextResponse.redirect(new URL('/login', request.url))
        }
		try{
        	const {payload} = await jwtVerify(jwt, new TextEncoder().encode('secret')
			console.log(payload)
            return NextResponse.next();
    	}
        catch(error){
            console.error(error)
            return NextResponse.redirect(new URL('/login', request.url))
        }
    }
    
	return NextResponse.next()
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
import {NextResponse} from 'next/server'
import {jwtVerify} from 'jose'

export async funcion middleware(request){
	console.log(request.url); 
    console.log(request.nextUrl.pathname) 
	
    const jwt = request.cookies.get('myTokenName'); 
    
    
    if(jwt === undefined){
    return NextResponse.redirect(new URL('/login', request.url))
    }
    try{
    const {payload} = await jwtVerify(jwt, new TextEncoder().encode('secret')
    console.log(payload)
    return NextResponse.next();
    }
    catch(error){
    console.error(error)
    return NextResponse.redirect(new URL('/login', request.url))
    }
    
    
	return NextResponse.next()
}

export const config = {
    matcher: ['/dashboard', '/']
}
```



Si quiero proteger sólo una ruta puedo hacerlo con:

```
export const config = {
    matcher: '/dashboard'
}
```

Si quiero proteger también `/admin/profile` y `/admin/user` no hace falta que ingrese ambas páginas en el listado sino que puedo poner directamente `/admin/:path*` y representará a todas las subrutas.

```
export const config = {
    matcher: ['/dashboard', '/admin/:path*']
}
```

