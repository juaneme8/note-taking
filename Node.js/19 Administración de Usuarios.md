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



Como queremos tener en la colección denotas también vinculación al usuario que la escribió debemos actualizar el modelo:

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

Cuando retornamos el elemento creado no le estaremos mandando ese hash ya que en el modelo lo hemos eliminado del objeto que se retorna gracias a la transformación del método `toJSON`.

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

   
