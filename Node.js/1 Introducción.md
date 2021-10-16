# Node.js
>The Net Ninja - Node.js Crash Course (2020) - [Repositorio ](https://github.com/iamshaunjp/node-crash-course)
>Se suman apuntes
>* The Net Ninja - Node JS Tutorial for Beginners (2016).
>* Code with Mosh - The Complete Node.js Course 

## Introducción
Antiguamente JavaScript corría únicamente en navegadores en el frontend y lo usábamos para aportar interactividad al sitio (por ejemplo al trabajar con click events). Node.js nos permite ejecutar código de JavaScript en el servidor o en nuestra pc por lo que decimos que es un _runtime environment_.

El motor de Google Chrome llamado **V8 engine** es un programa escrito en C++ que compila el JavaScript a machine code en tiempo real. Esto nos permitiría utilizar JavaScript sólamente dentro del navegador. Node.js es un programa escrito en C++ que utiliza el V8 Engine y como sí podemos ejecutar código C++ en la computadora, seremos capaces de correr código JavaScript tanto en nuestra computadora como en el server.
Node.js no es simplemente un wrapper del v8 engine, sino que aporta funcionalidades extra a JavaScript como ser:

* Lectura y escritura de archivos en la computadora
* Conexión a base de datos
* Funcionalidad de servidor

> Como contrapartida al utilizar Node.js perdemos al DOM (document object model) por lo que no podremos interacturar con elementos HTML. 
>
> Recordar que `document` forma parte del objeto `window` el cual no existe en Node.js, sino que contamos con el objeto `global`.

## Características Node.js
* **Multiplataforma** 
* **Open-source**
* Permite crear **API REST ** (application programming interface) con la cual vamos a a través de endpoints podremos crear información, recuperar, borrar, editar, etc. Un mismo path puede servir para diferentes funciones dependiendo de la acción que utilicemos.
* Es un **runtime environment**.
* Node tiene una naturaleza **asincrónica** o no bloqueante. Gracias a esto permite crear aplicaciones **escalables**. En el siguiente ejemplo de "hola mundo", pueden atenderse muchas conexiones simultáneamente sin agregar más hardware. Por cada conexión, se activa la devolución de llamada o _callback_, pero si no hay trabajo que hacer, Node.js se dormirá.

* Es utilizado por **PayPal, LinkedIn, Netflix, Trello, Walmart, Ebay, NASA**, etc.
* Si ya manejamos JavaScript no es necesario el aprendizaje de un nuevo lenguaje para el server y es posible compartir código entre frontend y backend.
* Tiene una **comunidad** muy grande.
* Gran ecosistema de **open source libraries** que posibilita no tener que desarrollar de cero los building blocks de nuestra aplicación logrando así concentrarnos en lo más específico de nuestro programa.
> Podremos buscar paquetes en  [npmjs](https://www.npmjs.com/)
> Podremos comparar paquetes en [npm trends](https://www.npmtrends.com/) 
* Es ideal para aplicaciones real-time como ser chats gracias a los websockets.
* Es ideal para data intensive apps pero no para cpu intensive apps (ya que los cálculos harán que el resto de los cliente deba esperar).

​       

## Módulos CommonJS

Para importar módulos en Node.js utilizamos CommonJS Modules de modo que si queremos trabajar con `http` lo hacemos del modo visto anteriormente:

```js
const http = require('http');
```

CommonJS son un tipo de módulos creados por Node.js cuando no existían los módulos ECMAScript

En las últimas versiones de Node ha dado soporte al sistema de módulos ECMAScript y es posible trabajar con ellos:

```js
import http from 'http'
```

Sin embargo, en la mayor parte de la documentación y foros online encontraremos información basada en CommonJS.

Debemos tener presente que `require()` cachea el resutlado y sólo la primera vez que se require ejecuta el código de inicialización. Dicho en otras palabras si tenemos un `console.log()` en un archivo que importamos y lo importamos en más de un lugar sólo lo veremos una vez.



## El Rol de JavaScript

El rol de JavaScript consiste en ejecutar código JavaScript del lado del servidor y manejar requests provenientes del navegador. Por ejemplo ante un usuario que visita el sitio, el navegador hará un request al server por lo que el servidor Node ejecutará código JavaScript para reaccionar a esa solicitud (ya sea comunicandose con una DB o archivos del servidor) y luego formulará una respuesta y la enviará al navegador. Esta respuesta podrá ser HTML (con contenido dinámico), imágenes, CSS, etc. 

## Arquitectura de Node
En Node.js contamos con un único thread. Cuando un cliente genera un request por ejemplo para procesar una query a una DB, el **single thread** se usará para servir a otro request de otro cliente y mientras tanto monitorea la **event queue** ya que es allí donde la DB pondrá un mensaje de **data ready**.

Arquitecturas de frameworks como ASP.NET en cambio, a **cada request lo manejan con un thread** y en caso de tener un gran número de clientes al mismo tiempo podríamos quedarnos sin threads para atenderlos. En ese caso será necesario esperar a que se liberen threads o bien incorporar más hardware. Es decir que no estamos usando los recursos de manera eficiente.
Sin embargo es posible implementar una arquitectura asincrónica pero esto demandará trabajo extra, en contraposición con Node.js donde es asincrónica por default.

## Instalación de Node.js
Descargar de [https://nodejs.org/en/](https://nodejs.org/en/) en el botón de la versión LTS.
Veremos que tenemos la **versión LTS** (long-term support) que es la versión de número **par** y la versión de número **impar** con las últimas características.

Para verificar si ya lo tenemos instalado ingresar en la terminal:
`node --version` o `node -v` y en caso de tenerlo veremos el número de versión.

> Es aconsejable actualizar a la versión actual.
>
> Reiniciar la terminal a la hora de volver a chequear el número de versión.

 

Ingresando `node` podremos comenzar a escribir código JavaScript, esto se conoce como **node-repl interactivo** (repl significa read eval print loop).

Sin embargo normalmente utilizaremos un editor de texto (Visual Studio Code, Sublime, Atom).

```bash
mkdir first-app
cd first-app
code .
```
Luego suponiendo que creamos el archivo `test.js` para ejecutar el código ingresamos `node test.js` o bien `node test`



