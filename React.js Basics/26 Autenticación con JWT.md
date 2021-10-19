# Autenticación con JWT

En el frontend mostraremos un formulario de usuario y contraseña, luego al enviarlos obtendremos el token que almacenaremos en local storage o cookies y finalmente lo utilizaremos cuando queramos hacer peticiones.



## Inicio de Sesión

Suponiendo que estamos trabajando con Create React App en  `src` creamos una carpeta `services` y en ella un archivo `login.js`. Allí crearemos los distintos métodos asociados al login y luego los exportaremos:

```jsx
import axios from 'axios'

const baseUrl = 'http://localhost:3001/api/login'

const login = async credentials => {
  const { data } = await axios.post(baseUrl, credentials)
  return data
}

export default { login }
```

El método login recibe en `credentials` el `username` y `password` y realiza con ellos una petición POST que si todo anduvo bien retornará el `token` en el objeto `data`.



Luego suponiendo que en `App.js` tenemos el formulario donde el usuario ingresa sus credenciales, creamos al presionar el botón de enviar llamamos a `handleLogin`:

```js
const handleLogin = async (event) => {
    event.preventDefault()

    try {
        const user = await loginService.login({
            username,
            password
        })
        window.localStorage.setItem(
        'loggedNoteAppUser', JSON.stringify(user)
      	)

       noteService.setToken(user.token)

        setUser(user)
        setUsername('')
        setPassword('')
    } catch(e) {
        setErrorMessage('Wrong credentials')
        setTimeout(() => {
            setErrorMessage(null)
        }, 5000)
    }
  }
```

Con los datos de autenticación devueltos hacemos dos cosas:

*  los guardamos en localStorage como string con `JSON.stringify()` (por lo que luego tendremos que parsearlos para que vuelvan a ser un objeto con `JSON.parse()`).

> Utilizamos `window.localStorage` y no `localStorage` directamente para ser conscientes de dónde proviene el método que estamos utilizando.

* Con `noteService.setToken(user.token)` establecemos el token en una variable local para que sea utilizado por los métodos del servicio cuando sea necesario. Luego utilizaremos otra técnica para compartir esa información.



## Uso del Token

Queremos que si refrescamos la página se mantenga la sesión y esto lo hacemos utilizando un efecto en `App` que también repite esas dos tareas en caso de que exista el token.

```jsx
 useEffect(() => {
    const loggedUserJSON = window.localStorage.getItem('loggedNoteAppUser')
    if (loggedUserJSON) {
      const user = JSON.parse(loggedUserJSON)
      setUser(user)
      noteService.setToken(user.token)
    }
  }, [])
```

Como el array de dependencias está vacío sólo se ejcutará cuando se renderice el componente el componente por primera vez.



En `services` tenemos un archivo `notes.js`con el cual vamos a crear recursos utilizando ese token.

```js
import axios from 'axios'
const baseUrl = 'http://localhost:3001/api/notes'

let token = null

const setToken = newToken => {
  token = `Bearer ${newToken}`
}

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

const create = (newObject) => {
  const config = {
    headers: {
      Authorization: token
    }
  }

  const request = axios.post(baseUrl, newObject, config)
  return request.then(response => response.data)
}

const update = (id, newObject) => {
  const config = {
    headers: {
      Authorization: token
    }
  }

  const request = axios.put(`${baseUrl}/${id}`, newObject, config)
  return request.then(response => response.data)
}

export default { getAll, create, update, setToken }
```



## Cierre de Sesión

Si borramos este elemento de local storage y refrescamos nos volverá a pedir que iniciemos sesión.

```
const handleLogout = () => {
	setUser(null);
	noteService.setToken(user.token)
	window.localStorage.remoteItem('loggedNoteAppUser')
}
```



## Ataques XSS

Los ataques XSS (cross-site scripting) o ataques de inyección de scripts pueden perpetrarse por ejemplo si tenemos un formulario donde esperamos un campo de texto como el contenido de una nota y el atacante ingresa código: `<script>...</script>` y ese código es ejecutado. En React.js esto no ocurre de manera inmediata sino que ocurriría si utilizamos la prop `dangerouslySetInnerHTML`.

```
<p dangerouslySetInnerHTML={{__html: note.content}}></p>
```

Esta prop normalmente es utilizada cuando queremos ingresar código html por ejemplo si queremos que el usuario pueda darle formato al texto utilizando `<b>`, `<i>` etc

Otra opción sería si tenemos un input donde podemos poner el enlace de una imagen y ponemos un método `onerror`:

```
https://placeimgxxx.com/320/320/any" onerror="alert('xss injection')
```



En lugar de utilizar Local Storage podríamos haber implementado una solución con cookies, cookies de http-only o incluso lo ideal hubiera sido utilizar la alternativa más moderna de cookies con `sameSite` que configurado como `strict` para que sólo sólo sea posible enviar a una web del mismo dominio, pero esto trae otras complicaciones.