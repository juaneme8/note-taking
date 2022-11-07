# Socket.io Chat: Express + React

> :link: Basado en [el video de Fazt Code](https://youtu.be/C1_Rw_H7Q3A) :writing_hand:



## Introducción

En este proyecto vamos a crear un real time chat utilizando un servidor Express y una aplicación React creada con Vite.js que utiliza TailwindCSS.



## Project Setup

En este proyecto en un mismo directorio vamos a tener el código del servidor y del cliente. Para ello vamos a crear una carpeta `react-express-sockets` en ella iniciamos un proyecto Node.js.

```bash
npm init -y
```



### Server Setup

* Instalar dependencias

```bash
npm i express socket.io cors morgan
```
> Vamos a utilizar morgan para ver en consola las peticiones en la medida que las vamos recibiendo.



```bash
npm i nodemon -D
```



* Modificar `package.json` para poder utilizar imports.

```json
"type": "module",
```



* Crear scripts de start y dev:

```json
 "scripts": {
    "start":"node server/index.js",
    "dev": "nodemon server/index.js"
 },
```



* Crear un directorio `server`.
* Crear dentro de ese directorio `index.js`




* #### Crear servidor Express básico.

```jsx
import express from 'express'

const app = express();

app.listen(3000);
console.log('Server Started on Port 3000')
```

Luego si ejecutamos `npm run dev` o `npm start` y vamos a http://localhost:3000 veremos un **Cannot GET /** lo cual nos da la pauta que el servidor está funcionando aunque no estemos trabajando con esa ruta aún.

* #### Incorporar Morgan

```jsx
import express from 'express'
import morgan from 'morgan'
const app = express();

app.use(morgan('dev'))

app.listen(3000);
console.log('Server Started on Port 3000')
```

Si en este momento hacemos una petición yendo a http://localhost:3000 veremos **GET / 404 4.377 ms - 139**



#### Configuración Socket.io

A partir de la aplicación de Express la convierto a un servidor HTTP que es lo que tengo que pasarle a Server de socket.io [como podemos ver en la documentación](https://socket.io/get-started/chat).

:rotating_light: A la hora de iniciar el servidor utilizo `server` ya que es este quien tiene la conexión con socket.io, por eso cambiamos `app.listen(3000);` por `server.listen(3000);`

```jsx
import express from 'express'
import morgan from 'morgan'
import { Server } from 'socket.io';
import http from 'http';

const app = express();
const server = http.createServer(app);
const io = new Server(server);

app.use(morgan('dev'))

server.listen(3000);
console.log('Server Started on Port 3000')
```



#### Configuración CORS

Queremos posibilitar la conexión desde otro servidor (el frontend).

```jsx
import express from 'express'
import morgan from 'morgan'
import { Server } from 'socket.io';
import http from 'http';
import cors from 'cors';

const app = express();
const server = http.createServer(app);
const io = new Server(server);

app.use(cors())
app.use(morgan('dev'))

server.listen(3000);
console.log('Server Started on Port 3000')
```



#### Archivo Configuración

Como queremos que el puerto sea fácilmente configurable al desplegar, vamos a utilizar un archivo de configuración `config.js`.

```jsx
export const PORT = 4000; 
```

Como estamos utilizando imports de módulos y no es de terceros debemos utilizar la extensión para importarlo.

```jsx
import express from 'express'
import morgan from 'morgan'
import { Server } from 'socket.io';
import http from 'http';
import cors from 'cors';
import {PORT} from './config.js'


const app = express();
const server = http.createServer(app);
const io = new Server(server);

app.use(cors())
app.use(morgan('dev'))

server.listen(PORT);
console.log(`Server Started on Port ${PORT}`)
```



### Client Setup

Vamos a crear la aplicación cliente con Vite.js (en el video utilizan CRA). De acuerdo [a la documentación](https://vitejs.dev/guide/) y como queremos que nos cree la carpeta `client`:

```bash
npm create vite@latest client
```

Al terminar debemos navegar al directorio e instalar las dependencias.



#### Instalación Socket.io

```bash
npm i socket.io-client
```

> Tenemos que hacerlo estando en al carpeta `client`.



## Ejecución desde consola única

En este momento debemos desde la raíz ejecutar `npm run dev` para levantar el servidor y luego navegar a `client` y ejecutar `npm run dev`. Esto mismo podemos hacerlo con un único comando si utilizamos el paquete [concurrently](https://www.npmjs.com/package/concurrently). Además nos permitirá ver los logs de ambas aplicaciones en un único sitio.

```bash
npm i concurrently
```



Creamos un nuevo script en el `package.json` de modo que ejecute ambos comandos utilizando esta herramienta:

```json
"scripts": {
    "start":"node server/index.js",
    "dev": "nodemon server/index.js",
    "server": "concurrently \"npm run dev\" \"cd client && npm run dev\""
},
```





### Primeros pasos socket.io lado cliente

Nos basamos en lo indicado en [la documentación](https://socket.io/docs/v4/client-initialization/).

En `App.js` 

Con `socket` vamos a escuchar lo que nos manda el backend y enviarle también datos.

```jsx
import {io} from 'socket.io-client'

const socket = io('http://localhost:4000')
```

En ese momento intentaremos conectarnos pero no obtendremos respuesta, por lo que en la consola veremos: **Failed to load resource: the server responded with a status of 404 (Not Found)** que nos aparecerá cada un intervalo de tiempo en el que vuelve a intentar la conexión.



### Primeros pasos socket.io lado servidor

#### Evento Conexión

Para evitar el error mostrado anteriormente en el servidor debemos escuchar nuevos eventos de conexión.

```jsx
import express from 'express'
import morgan from 'morgan'
import { Server } from 'socket.io';
import http from 'http';
import cors from 'cors';
import {PORT} from './config.js'


const app = express();
const server = http.createServer(app);
const io = new Server(server);

app.use(cors())
app.use(morgan('dev'))

io.on('connection', () => {
  console.log('A user connected')
})

server.listen(PORT);
console.log(`Server Started on Port ${PORT}`)
```



#### Configurar CORS

En este momento en la consola del cliente nos aparecerá un error de CORS por lo que debemos configurarlo.

```jsx
import express from 'express'
import morgan from 'morgan'
import { Server } from 'socket.io';
import http from 'http';
import cors from 'cors';
import {PORT} from './config.js'


const app = express();
const server = http.createServer(app);
const io = new Server(server, {
  cors: {
    origin: 'http://127.0.0.1:5173'
  }
});

app.use(cors())
app.use(morgan('dev'))

io.on('connection', (socket) => {
  console.log(`User ${socket.id} connected`)
})

server.listen(PORT);
console.log(`Server Started on Port ${PORT}`)
```



:warning: Notar que usamos `origin: 'http://127.0.0.1:5173'`ya que estamos trabajando con Vite.js. Si ingresamos a la dirección http://localhost:5173 que también nos muestra la aplicación nos aparecerá error de CORS. 
Si le coloco `origin: '*'` significa que quiero que cualquier servidor se conecte.

En el evento de conexión obtenemos un objeto `socket` que nos permitirá entre otras cosas ver el `id` de quién se conecta. Si abro varias pestañas en el navegador en todas veremos un nuevo id.



### Enviar eventos del cliente al servidor

Vamos a crear un formulario con un campo de texto de modo que al presionar un botón enviemos la información ingresada al servidor. Vamos a emitir un evento llamado `message`.

```jsx
import { useState } from 'react'
import {io} from 'socket.io-client'

const socket = io('http://localhost:4000')

function App() {
  const [message, setMessage] = useState('')

  const handleSubmit = e => {
    e.preventDefault();
    // console.log(message)
    socket.emit('message',message)
  }

  return (
    <div>
      <h1>Vite.js</h1>
      <form onSubmit={handleSubmit}>
        <input 
            type="text" 
            onChange={e => setMessage(e.target.value)} 
            value={message} 
         />
        <button>Enviar</button>
      </form>
    </div>

  )
}

export default App
```



### Recibir eventos en el servidor

```jsx
io.on('connection', (socket) => {
  console.log(`User ${socket.id} connected`)

  socket.on('message', (data) => {
    console.log(data)
  })
})
```

Notar que estaremos escuchando el evento`message` y que trabajamos con `socket.on` y no `io.on('')` 

```jsx
import express from 'express'
import morgan from 'morgan'
import { Server } from 'socket.io';
import http from 'http';
import cors from 'cors';
import {PORT} from './config.js'


const app = express();
const server = http.createServer(app);
const io = new Server(server, {
  cors: {
    origin: 'http://127.0.0.1:5173'
  }
});

app.use(cors())
app.use(morgan('dev'))

io.on('connection', (socket) => {
  console.log(`User ${socket.id} connected`)

  socket.on('message', (data) => {
    console.log(data)
  })
})

server.listen(PORT);
console.log(`Server Started on Port ${PORT}`)
```



### Reenviar eventos a todos los clientes

Queremos ahora enviar este mensaje recibido a todos los clientes menos a aquel que lo envío.

```jsx
io.on('connection', (socket) => {
  console.log(`User ${socket.id} connected`)

  socket.on('message', (data) => {
    // console.log(data)
    socket.broadcast.emit('message', data)
  })
})
```



```jsx
import express from 'express'
import morgan from 'morgan'
import { Server } from 'socket.io';
import http from 'http';
import cors from 'cors';
import {PORT} from './config.js'


const app = express();
const server = http.createServer(app);
const io = new Server(server, {
  cors: {
    origin: 'http://127.0.0.1:5173'
  }
});

app.use(cors())
app.use(morgan('dev'))

io.on('connection', (socket) => {
  console.log(`User ${socket.id} connected`)

  socket.on('message', (data) => {
    // console.log(data)
    socket.broadcast.emit('message', data)
  })
})

server.listen(PORT);
console.log(`Server Started on Port ${PORT}`)
```



### Escuchar eventos en el cliente

Como vemos en la [documentación](https://socket.io/how-to/use-with-react-hooks) al trabajar en socket.io con react hooks, vamos a trabajar con un `useEffect` y queremos estar a la escucha de los eventos de tipo `message` y luego eliminar los listeners en el proceso de limpieza para evitar registros múltiples.

También sumamos la limpieza del formulario al enviar el mensaje.

```jsx
 useEffect(() => {
    socket.on('message', (data) => {
      console.log(data)
    })
    
    return () => {
      socket.off('message')
    };
 },[])
```

```jsx
import io from 'socket.io-client'
import { useState } from 'react'
import { useEffect } from 'react'

const socket = io('http://localhost:4000')


function App() {
  const [message, setMessage] = useState('')

  useEffect(() => {
    socket.on('message', (data) => {
      console.log(data)
    })
    
    return () => {
      socket.off('message')
    };
  },[])

  const handleSubmit = e => {
    e.preventDefault();
    socket.emit('message', message)
    setMessage('')
    // console.log(message)
  }

  return (
    <div>
      <h1>Vite.js</h1>
      <form onSubmit={handleSubmit}>
        <input 
            type="text" 
            onChange={e => setMessage(e.target.value)} 
            value={message} 
        />
        <button>Enviar</button>
      </form>
    </div>

  )
}

export default App
```



### Presentación de Mensajes

Queremos presentar los mensajes en pantalla, para eso vamos a trabajar con una variable de estado `messages` que es un array de objetos con la propiedad `from` y `body`.

La idea será actualizar esto tanto cuando el usuario hace submit de un nuevo mensaje como cuando recibimos un mensaje de otro cliente. Si bien cuando se trate de nuevos mensajes podremos poner `from: socket.id` cuando sea mensajes que recibimos debemos conocer el id.  



:rotating_light: 

# PENDIENTE FRONT



Para saber quién ha enviado el mensaje debemos modificar el valor entregado por el servidor de modo tal que agregue este dato.

```jsx
io.on('connection', (socket) => {
  console.log(`User ${socket.id} connected`)

  socket.on('message', (data) => {
    // console.log(data)
    socket.broadcast.emit('message', {from: socket.id, body: data})
  })
})
```

