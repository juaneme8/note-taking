# Administración de Usuarios

Suponiendo que queremos crear un colección de notas y cada nota es escrita por un usuario. Un usuario puede tener múltiples notas, pero cada nota sólo tiene un usuario. 

En nuestra base de datos no relacional NoSQL podríamos implementar algo similar a lo que haríamos en SQL. Por un lado tendríamos una colección de usuarios:

```json
[{
	"user": "juan",
    "id": 123
},{
	"user":"ocho"
	"id":888
}]
```

Y por otro lado tendríamos la tabla de notas con el `userId` del autor.

```json
[{
	"content": "una nota",
	"id": 565656565678,
	"userId": 888
},{
	"content": "otra nota",
	"id": 898989898965,
	"userId": 123
}]
```

Otra opción podría ser tener en cada usuario un array `notes` con el id de cada una de las notas, ya sea conservando además el `userId` en cada nota (lo cual favorece la obtención de información luego) o no.

```json
[{
	"user": "juan",
    "id": 123,
    "notes":[565656565678]
},{
	"user":"ocho",
	"id":888,
	"notes":[898989898965]
}]
```

También es posible tener toda la información de las notas incorporada directamente dentro del documento.

```json
[{
	"user": "juan",
    "id": 123,
    "notes":[
        {
            "content": "una nota",
            "id": 565656565678,
            "userId": 888
        }
    ]
},{
	"user":"ocho",
	"id":888,
	"notes":[
		{
            "content": "otra nota",
            "id": 898989898965,
            "userId": 123
    	}
	]
}]
```

Esto sería útil si frecuentemente queremos solicitar la información de un usuario y sus notas.



Realizaremos la implementación en la cual dentro de la colección `Users` guardamos un array con los datos de las notas que escribió.

```js
const { Schema, model } = require('mongoose')

const userSchema = new Schema({
  username: String,
  name: String,
  passwordHash: String,
  notes: [{
    type: Schema.Types.ObjectId,
    ref: 'Note'
  }]
})

userSchema.set('toJSON', {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id
    delete returnedObject._id
    delete returnedObject.__v
  }
})

const User = model('User', userSchema)

module.exports = User
```

> En el array `notes` indicamos que queremos almacenar un `ObjectId` que tendrá referencia con el modelo `Note` y esto nos permitirá aprovechar algunas funcionalidades.
>
> A la hora de enviar información desde el servidor al cliente, nuevamente transformamos el `toJSON` pero esta vez además de eliminar `__v`, y cambiar `_id` por `id`, eliminamos el `passwordHash`4



Como queremos tener en la colección de notas también vinculación al usuario que la escribió debemos actualizar el modelo:

```
const mongoose = require('mongoose');

const {Schema, model} = mongoose;
const noteSchema = new Schema({
	content: String,
    date: Date,
    important: Boolean,
    user:{
    	type: Schema.Types.ObjectId,
    	ref: 'User'
    }
});
 
const Note = model('Note', noteSchema);
```

> En ocasiones puede pasarnos que a raíz de estos cambios, los documentos que tenemos en nuestra colección han quedado deprecados, en ese caso se suele ejecutar una lamda encargada de normalizar esta situación.



Luego dentro de `controllers` creamos un archivo `user.js` :

```js
const bcrypt = require('bcrypt')
const usersRouter = require('express').Router()
const User = require('../models/User')

usersRouter.post('/', async (request, response) => {
  const { body } = request
  const { username, name, password } = body

  const saltRounds = 10
  const passwordHash = await bcrypt.hash(password, saltRounds)

  const user = new User({
    username,
    name,
    passwordHash
  })

  const savedUser = await user.save()

  response.status(201).json(savedUser)
})

module.exports = usersRouter
```

En la base de datos almacenamos el `passwordHash` que es el password encriptado y `password` que es la contraseña en texto plano. 

Utilizamos como herramienta **bcrypt** y se trata de una operación asincrónica ya que tarda cierto tiempo en completarse. Cuando mayor sea el valor de `saltRounds` más seguro será pero tardará más tiempo en generarse.

Cuando retornamos el elemento creado no le estaremos mandando ese hash ya que en el modelo lo hemos eliminado del objeto que se retorna gracias a la transformación del método `toJSON` (ver mas adelante).

> La página [haveibeenpwned](https://haveibeenpwned.com/) nos permite saber si un determinado correo tuvo filtraciones de sus passwords en algún servicio.



Luego en `index.js` tendremos que importar `usersRouter` y lo usamos de este modo:

```js
app.use('/api/users', usersRouter)
```



## Tests de Administración de Usuarios

> Los tests se encuentran en la sección Testing en Node.js



### Usuarios con `username` único

A la hora de verificar que un `username` propuesto no esté ya siendo utilizado en la base de datos, podemos hacerlo manualmente de esta forma:

```
const user = await User.find(username:req.body.username)
if(user){
	res.status(409).send({error: expected `username` to be unique})
}
```



Otra opción es utilizar la librería **mongoose-unique-validator** que nos permite especificar cuando queremos que un campo sea *unique* directamente en el Schema, con la consiguiente ventaja de no tener que hacerlo todas las veces que fuera necesario luego en los controladores.

**No confundir con la opción `unique` de Mongoose que como indica en su [documentación](https://mongoosejs.com/docs/validation.html) no es para validación**


El archivo del modelo nos queda de la siguiente forma:

```
const { Schema, model } = require('mongoose')
const uniqueValidator = require('mongoose-unique-validator')

const userSchema = new Schema({
  username: {
    type: String,
    unique: true
  },
  name: String,
  passwordHash: String,
  notes: [{
    type: Schema.Types.ObjectId,
    ref: 'Note'
  }]
})

userSchema.set('toJSON', {
  transform: (document, returnedObject) => {
    returnedObject.id = returnedObject._id
    delete returnedObject._id
    delete returnedObject.__v

    delete returnedObject.passwordHash
  }
})

userSchema.plugin(uniqueValidator)

const User = model('User', userSchema)

module.exports = User
```



El agregado de esta funcionalidad que impida crear nuevos elementos con el campo `username` repetido consta de tres pasos:

1. ```
   const uniqueValidator = require('mongoose-unique-validator')
   ```

   

2. ```
    username: {
       type: String,
       unique: true
     },
   ```

   

3. ```
   userSchema.plugin(uniqueValidator)
   ```

   

# JWT

## Introdución

Los JSON Web Tokens o JWT son un standard de la industria que posibilita que dos extremos se puedan comunicar entre sí de manera segura. La información puede ser verificada y confiada debido a que está firmada digitalmente. 

Como podemos ver en la documentación oficial [jwt.io/introduction](https://jwt.io/introduction) un JWT consiste en tres partes separadas por puntos:

* **Header** contiene información del algoritmo y el tipo de token:

  ```json
  {
    "alg": "HS256",
    "typ": "JWT"
  }
  ```

  El header es codificado en **Base64Url** y forma la primera parte del token.

* **Payload** contiene declaraciones acerca del usuario y datos adicionales

  ```
  {
    "sub": "1234567890",
    "name": "John Doe",
    "admin": true
  }
  ```

  El Payload es codificado en **Base64Url** y forma la segunda parte del token.

  > Debemos tener presente que esta información si bien no puede ser manipulada, es accesible por todos por lo que no debemos colocar información secreta ni en el header ni en el payload.

* **Signature** para crear la firma tomamos el header codificado, el payload codificado, un secreto y el algoritmo indicado en el header. La firma nos permite saber que el mensaje no fue modificado en el camino.

> El acceso físico por parte de un tercero a la máquina, podría fácilmente comprometer el token ya que lo guardaremos en local storage o cookies y obteniéndolo podría simular que es él el dueño de la sesión. Sin embargo existen distintas alternativas para minimizar esto por ejemplo fijándonos que la petición sea de la misma localización que aquella en la que se inició la sesión o verificar que haya sido creado con un dispositivo usando el mismo User-Agent. Otra alternativa es agregarle una expiración que hará que luego de cierto tiempo el usuario tenga que volver a loguearse.
>
> También debemos tener presente que si estamos realizando la comunicación mediante http, alguien podría copiarse al vuelo el token pues la comunicación no está cifrada. Es por eso que a excepción de cuando estamos probando en localhost, debemos utilizar https.

## Autenticación con token

Utilizaremos una autenticación basada en token que tiene el siguiente principio:

En primer lugar el usuario completa su usuario y contraseña y presiona el botón Login. Esto representa un `POST` a `/api/login` y el backend devuelve entrega un `token` que identifica la sesión de ese usuario. Luego el token es guardado por el navegador (existen distintas formas de guardarlo). A partir de ese momento cuando el usuario  quiere crear un nuevo recurso realiza un `POST` a `/api/notes` con el token en el header (Se utiliza como encabezado`Authorization` y el esquema `Bearer`). Luego el backend identificará al usuario por medio del token, obtendrá su id del jwt y creará la nota.



## Obtención de token

Creamos un nuevo controlador `login.js` (en `index.js` debemos agregar `app.use('/api/login', loginRouter)`)

Lo primero que hacemos es crear el `POST` en ruta `/api/login` y será buscar el usuario recibido en la base de datos. 

```js
const user = await User.findOne({ username })
```

Si lo encontramos comparamos el `password` recibido con el `passwordHash` guardado en la base de datos . 

Ambas cosas las hacemos con el siguiente operador ternario:

```js
const passwordCorrect = user === null
    ? false
    : await bcrypt.compare(password, user.passwordHash)
```

> `bcrypt.compare(password, user.passwordHash)` devuelve `true` si ambos hashes son iguales.

Ya sea que el usuario no exista o que el password sea incorrecto devolvemos un 401 (Unauthorized) y en el mensaje le decimos que ingresó el password o el usuario mal. Esto es a fin de evitar darle información extra a alguien que pudiera querer realizar un ataque y le resultaría útil saber si un determinado usuario existe o no.



Utilizamos la librería jsonwebtoken 

```
npm install jsonwebtoken
```

Una vez instalada, la importamos y con el método `jwt.sign()` firmamos el token pasándole como argumentos el payload, el secreto y la expiración. Luego le devolvemos el token generado al usuario.



> Es importante tener el `_id` del usuario en el payload del token ya que luego lo usaremos para buscar el usuario a la hora de crear recursos.



El código completo nos queda así:

```js
const jwt = require('jsonwebtoken')
const bcrypt = require('bcrypt')
const loginRouter = require('express').Router()
const User = require('../models/User')

loginRouter.post('/', async (request, response) => {
  const { body } = request
  const { username, password } = body

  const user = await User.findOne({ username })

  const passwordCorrect = user === null
    ? false
    : await bcrypt.compare(password, user.passwordHash)

  if (!passwordCorrect) {
    response.status(401).json({
      error: 'invalid user or password'
    })
  }

  const userForToken = {
    id: user._id,
    username: user.username
  }

  const token = jwt.sign(
    userForToken,
    process.env.SECRET,
    {
      expiresIn: 60 * 60 * 24 * 7
    }
  )

  response.send({
    name: user.name,
    username: user.username,
    token
  })
})

module.exports = loginRouter
```



## Verificación de token

Ahora queremos que sólo los usuarios autenticados sean capaces de crear un recurso al acceder al endpoint `POST` en `/api/notes`

Una forma de implementarlo sería pasándole el token en el cuerpo de la petición por ejemplo `req.body.token`, pero lo correcto es recibir esta información mediante una cabecera http. Lo haremos con el header `Authorization` y utilizando el esquema de autenticación **Bearer**.



Para obtener el header Authorization:

```js
const authorization = request.get('authorization')
```

Necesitamos por un lado haber recibido una cabecera `Authorization` y que tenga el esquema correcto (`Bearer`). A la hora de comprobar esto último lo convertimos a minúsculas porque si bien deberían pasárnoslo como Bearer puede que nos lo pasen con otra capitalización. En caso de que sea correcto guardamos en `token` el contenido del encabezado sacando los primero 7 caracteres es decir "Bearer ".

Luego con ese token y nuestro secreto verificamos que sea correcto con `jwt.verify(token, process.env.SECRET)` y almacenamos el resultado en `decodedToken`.

Ya sea que el `token` no exista o que `decodedToken` sea `false` devolvemos un 401 al usuario.

```js
const authorization = request.get('authorization')

let token = "";
if(authorization && authorization.toLowerCase().startsWith('bearer')){
	token = authorization.substring(7);
    //token = authorization.split(" ")[1];
}
decodedToken = jwt.verify(token, process.env.SECRET)
if(!token || !decodedToken ){
    return res.status(401).json{
        error: 'token missing or invalid'
    }
}
const {id: userId} = decodedToken;
const user = await 
```

> `decodedToken` tiene un campo `iat` que hace referencia a cuando se firmó digitalmente ese token.



Utilizando la extensión REST Client para enviar la petición creamos un archivo `create_note.rest`

```
POST http://localhost:3001/api/notes
Content-Type: application/json
Authorization: Bearer eyJhjhasjdhajs...

{
	content: 'Contenido de la nota',
	"important": true,
}
```

> Notar que ya no tengo que pasarle el userId ya que ese dato se obtiene del token.



## Middleware Extracción Usuario

Creamos un middleware `userExtractor.js` que tendrá como propósito extraer la información del usuario del token y la agregará al objeto `req` de la siguiente forma:

```
req.userId = decodedToken;
```



```js
const jwt = require('jsonwebtoken')

module.exports = (request, response, next) => {
  const authorization = request.get('authorization')
  let token = ''

  if (authorization && authorization.toLowerCase().startsWith('bearer')) {
    token = authorization.substring(7)
  }

  const decodedToken = jwt.verify(token, process.env.SECRET)

  if (!token || !decodedToken.id) {
    return response.status(401).json({ error: 'token missing or invalid' })
  }

  const { id: userId } = decodedToken

  request.userId = userId

  next()
}
```



Este principio puede aplicarse para lograr rutas privadas.



```
app.post('/api/notes', userExtractor, async (req, res, next) => {

});
```

Como sabemos irá ejecutando los middlewares de izquierda a derecha y en caso de que haya un error de autenticación no llegará nunca a ejecutar el código específico de agregado de un nuevo documento.

Esto mismo lo repetimos en todas aquellas rutas que queremos proteger y tendremos certeza de que el usuario tiene sesión.
