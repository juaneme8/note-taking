# Clients & Servers
 Cuando ingresamos una dirección web en el navegador y presionamos enter, enviamos un **GET** request al servidor de ese sitio en particular. Esto lo logramos gracias a que el navegador busca la IP asociada a ese nombre de dominio con lo cual logramos acceder al server en la computadora del host donde se encuentra alojada la página. Esto lo hace creando un socket, que es un canal de comunicación en el cual la información puede enviada.  Esta comunicación entre cliente y servidor sucede a través de **HTTP** (hyper-text transfer protocol) es un protocolo que indica una serie de reglas para la comunicación.
El server luego decidirá qué envía como respuesta (HTML, imágenes, css, json, etc). La información es enviada por el socket via **TCP** (a pesar de que la información está estructurada en HTTP).
El protocolo TCP divide los datos en pequeñas secciones llamadas paquetes y los transmite por el socket. Esta funcionalidad está incorporada en nuestras PCs y Node.js nos da la forma de acceder a ella para abrir una conexión entre dos máquinas y enviar información entre ellas.

## Creación de un Servidor:
Crearemos manualmente un servidor en el backend el cual estará escuchando requests del navegador y decidirá qué respuestas entregar.
Con el método `http.createServer()` crearemos el server y tiene como argumento un callback que se ejecutará cada vez que recibamos un request. En `req` tendremos información relacionada con el request como por ejemplo la URL, el tipo de request, etc. Por otra parte `res` es el objeto que usaremos para enviar una respuesta al cliente. Para que el server esté activo escuchando requests, debemos utilizar el método `server.listen()` cuyo primer argumento es el número de puerto, el segundo es host name (el valor default es 'localhost') y el tercero un callback que se dispara cuando comienzo a escuchar. 

```js
const http = require('http');

const server = http.createServer((req, res) => {
	console.log('request made');
});

// localhost is the default value for 2nd argument
server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
});
```
En el ejemplo anterior ingresemos a la URL que ingresemos obtendremos el mensaje "request made" no estamos haciendo un ruteo específico. 
Tener presente que `server` es un *EventEmitter* por lo que tendrá todas las capacidades vistas cuando presentamos `event module` método `on`, `emit`, etc por lo que podríamos tener:

```js
const http = require('http');

//Notar que no tenemos el callback con los argumentos req, res
const server = http.createServer();

server.on('connection', () => {
	console.log('Nueva Conexión')
}
server.listen(3000)
```
Esto significa que cada vez que haya un pedido al server se genera un evento (notar que el handler del evento debe estar antes de escuchar).
Sin embargo en aplicaciones reales no vamos a responder al *connection event* ya que es algo de muy bajo nivel y en cambio lo más común es pasar un callback al método `createServer()` como hicimos anteriormente.


## Localhost y Puertos:
Localhost es similar a un nombre de dominio, que nos lleva a la dirección IP conocida como *loop-back* cuyo número es 127.0.0.1. Cuando el cliente realiza una petición al server lo hace a través de un puerto específico de manera tal que pueda hacérsela a Node.js y no a otro programa (email por ejemplo). Node estará escuchando un puerto en particular por lo que las peticiones deberán ser enviadas a ese puerto.
Los números de puertos representan canales específicos en nuestra computadora a través de los cuales determinado software se comunica para enviar y recibir datos. Cada programa usa un puerto diferente para mantener la información separada entre ellos (podemos pensarlo como una puerta a través de la cual se efectúan las comunicaciones con un software determinado). Nuestro servidor también necesita un número de puerto para comunicarse y un valor comúnmente usado es 3000. 
Ejecutamos el archivo `node server` y este se quedará esperando requests. Por lo que en el navegador escribiremos `localhost:3000` y en ese momento veremos `request made` en la terminal de node donde se está ejecutando el código y no en la del navegador. Sin embargo en el navegador el icono permanecerá cargando dado que no hemos dado una respuesta lo cual haremos con el objeto `res`.