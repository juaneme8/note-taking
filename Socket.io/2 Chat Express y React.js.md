# Socket.io Chat: Express + React

> :link: Basado en [el video de Fazt Code](https://youtu.be/C1_Rw_H7Q3A)



## Introducción

En este proyecto vamos a crear un real time chat utilizando un servidor Express y una aplicación React creada con Vite.js que utiliza TailwindCSS.



## Configuración Proyecto

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

```jsx
import express from 'express'
import morgan from 'morgan'
import { Server } from 'socket.io';
import http from 'http';

const app = express();
const server = http.createServer(app);
const io = new Server(server);

app.use(morgan('dev'))

app.listen(3000);
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

app.listen(3000);
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

app.listen(PORT);
console.log(`Server Started on Port ${PORT}`)
```



### Client Setup

Vamos a crear la aplicación cliente con Vite.js (en el video utilizan CRA). De acuerdo [a la documentación](https://vitejs.dev/guide/) y como queremos que nos cree la carpeta `client`:

```
npm create vite@latest client
```

Al terminar debemos navegar al directorio e instalar las dependencias.



#### Instalación Socket.io

```
npm i socket.io-client
```

> Tenemos que hacerlo estando en al carpeta `client`.

En `App.js`



## Ejecución desde consola única

En este momento debemos desde la raíz ejecutar `npm run dev` para levantar el servidor y luego navegar a `client` y ejecutar `npm run dev`. Esto mismo podemos hacerlo con un único comando si utilizamos el paquete [concurrently](https://www.npmjs.com/package/concurrently).

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

