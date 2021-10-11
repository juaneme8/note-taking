# JWT

El uso de JWT (JSON Web Token) nos permitirá que sólo los usuarios autenticados sean capaces de crear nuevas notas.



## Autenticación con Token

Utilizaremos una autenticación basada en token que tiene el siguiente principio:

En primer lugar el usuario completa su usuario y contraseña y presiona el botón Login. Esto representa un `POST` a `/api/login` y el backend entrega un token que identifica a este usuario y se lo retorna. Luego el token es guardado por el navegador (existen distintas formas de guardarlo). A partir de ese momento cuando el usuario presione el botón "Crear Nota", realiza un `POST` a `/api/notes` con el token en el header. Luego el backend identificará al usuario por medio del token y creará la nota.



## POST `/api/login`

Creamos un nuevo controlador `login.js`.

Lo primero que hacemos es crear el `POST` en ruta `/api/login` y será buscar el usuario recibido en la base de datos. 

```js
const user = await User.findOne({ username })
```

Si no lo encontramos indicamos que el password y si lo encontramos comparamos el `passwordHash` guardado en al base de datos con el `password` recibido. Esto lo hacemos con este operador ternario:

```js
const passwordCorrect = user === null
    ? false
    : await bcrypt.compare(password, user.passwordHash)
```

> `bcrypt.compare(password, user.passwordHash)` devuelve `true` si ambos hashes son iguales.

Ya sea que el usuario no exista o que el password sea incorrecto devolvemos un 401 (Unauthorized) y en el mensaje le decimos que ingresó el password o el usuario mal. Esto es a fin de evitar darle información a alguien que pudiera querer realizar un ataque y le resultaría útil saber si un determinado usuario existe.



El código completo nos queda así:

```js
const const jwt = require('jsonwebtoken')
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

  if (!(user && passwordCorrect)) {
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



En `index.js` debemos agregar `app.use('/api/login', loginRouter)`