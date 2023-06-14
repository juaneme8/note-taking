# Postman

:link: Basado en [el video](https://youtu.be/VywxIQ2ZXw4) de FreeCodeCamp

Postman es una herramienta para interactuar con una API y así obtener datos de un servidor. Sin embargo, no se trata de una herramienta para realizar tests de performance o de seguridad.

## Variable Scopes

Postman cuenta con distintos scopes de variables. Cuando seleccionamos una parte de la URL nos permite guardarla como variable de entorno y nos ofrece las siguientes:

* Global disponibles dentro de todo el workspace es decir todas las colecciones.

* Active Environment: Nos permite utilizarlo para mas de una colección y a su vez podremos aplicarle distintos entornos a una misma colección (por ejemplo development, testing y producción)

* Collection: tendremos que guardar las requests en la colección para poder usar esas variables.

  

> En el video estudiado se utiliza camelCase para las variables `baseUrl`, `accessToken`, etc.

### Values

`INITIAL VALUE` es lo que podemos compartir con otras personas (podemos dejarlo en blanco incluso) y `CURRRENT VALUE` es el valor que se usa por lo que es donde pondremos nuestros valores secretos.

Por ejemplo si tuviera una variable `accessToken` podría poner como `INITIAL VALUE` algo como --- y como `CURRENT VALUE` el verdadero valor.

### Request names

Como request name debemos poner nombres descriptivos:

Get single book

List of books

API Status



## Duplicar request. 

Hacer click en los tres puntitos en la request desde la colección y luego en Duplicate. 



## Data random

Es posible enviar datos random con Postman, por ejemplo si tenemos que enviar un nombre en el body de una petición podemos usar:  `"{{$randomFullName}}"` a penas pongamos el signo `$` nos aparecerán las opciones.



## Conocer peticiones enviadas

Para conocer qué es lo que enviamos podemos abrir la consola de Postman e inspeccionar cada uno de los requets.



# API Tests

Verificar una API manualmente consume mucho tiempo y encima en caso de cambios en esta API tendremos que volver a testear todo otra vez. Queremos escribir API tests que nos permitan automatizar este proceso.



* API Status
* List of books 
* Get single book
* Order book
* Get all boook orders
* Get an order
* Update an order
* Delete order
* Register API Client



Por ejemplo en caso de la request a `/status` sabemos si la respuesta haciendo un inspect de la respuesta. Sabiendo que lo será si recibimos un 200 como status code y un body con un JSON `{"status":"OK"}`

En la pestaña Tests hacemos click:

> Es importante siempre comenzar con un test fallando y luego hacerlo pasar. En este caso podría ser con `/status/foo` en este caso obtendremos un 404 y no un 200.



Veremos a la derecha una serie de snippets de código como por ejemplo uno llamado "Status code: Code is 200" y si hacemos click:

```js
pm.test("Status should be 200", () => {
    pm.response.to.have.status(200);
});


const response = pm.response.json();

pm.test("Status should be OK", () => {
    pm.expect(response.status).to.eql("OK");
});
```

Si ahora enviamos la request veremos que aparece como Test Results (1/1).

A partir del JSON debemos parsearlo para convertirlo en un objeto de JavaScript.



Podemos chequar los status code para todos los endpoints:

Para los GET 200

Para los POST 201 (Created)

Para los PATCH, DELETE 204 (No Content)



Extraer data de la respuesta

Cuando creamos un recurso y obtenemos el id, la forma manual es crear una variable de entorno y luego usarla para listar, editar y eliminar. Sin embargo, la próxima vez que queramos testear la API tendremos que volver a obtener el id y actualizar la variable.

Queremos en cambio, actualizar la variable de entorno a partir del id obtenido en la respuesta automáticamente.

Suponemos que estamos utilizando parámetros para obtener los libros de `type` `non-fiction`.

```
{{baseUrl}}/books?type=non-fiction
```



Trabajaremos nuevamente en el apartado Tests.

```
const response = pm.response.json();
const nonFictionBooks = respone.filter(book => book.available===true)
console.log(nonFictionBooks[0])

pm.globals.set("bookId", nonFictionBooks[0].id)
```

Queremos asegurarnos que el elemento cuyo id elegimos esté disponible, por lo que los filtramos y nos quedamos con el primero.

Utilizaremos el code snippet "Set a global variable"



Como podría darse un caso en el cual no encuentre un id con esas características, queremos por un lado crear un test que pase si libro fue encontrado y además sólo guardar la variable en ese caso:

```js
const book = nonFictionBooks[0];
pm.test("Book found, () => {
	pm.expect(book).to.be.an('object');
	pm.expect(book.available).to.be.true;
	pm.expect(book.available).to.eql().true;

	pm.expect(book.type).to.eql('non-fiction')

}")

if(book){
	pm.globals.set("bookId", book.id)
}
```

Con este último if evitamos un error de JavaScript.



Si queremos verificar que un valor el valor de una propiedad sea mayor a un cierto umbral.

```js
const response = pm.response.json();

pm.text("Is in stock?", ()=>{
	pm.expect(response["current-stock"]).to.be.above(0);
})
```

> Notar que debido al guión de current-stock no podemos usar dot notation.



## Testear redirects

Cuando hacemos un redirect por defecto en Postman viene configurada la opción "Automatically follow redirects", esto hará que veamos un 302 y en los Headers dentro de location veremos a dónde nos enviaría ese redirect.



## Collection Runner

Una vez que tenemos todos definidos todos los tests y las variables de entorno que se generen a partir de datos de una respuesta para ser usados en la request siguiente, podemos automatizar el proceso. Utilizaremos el collection runner que es una herramienta de postman que nos permite ejecutar toda la colección con un solo click.



En la parte de abajo vemos un botón que dice Runner. Debemos hacer click ahi y en el panel run order debemos arrastrar la colección. En este panel podremos deshabilitar requests y reordenarlas.

También podremos tildar Save response lo cual sera interesante si algo falla.



### Request execution order

Es posible establecer en el código la próxima request que queremos ir de manera programática. Esto puede ser util si por ejemplo tenemos una request de autenticación que no siempre queremos ejecuctar por ejemplo si tenemos la siguiente colección:

1. API Status
2. Register API Client
3. List of books



Estando en la pestaña tests de 1 podremos saltarnos 2 de este modo:

```
postman.setNextRequest("List of books")
```

> Notar que es postman y no pm, esto puede cambiar.
>
> Tener presente que podemos generar un lazo infinito si ponemos un salto hacia atrás en lugar de hacerlo hacia adelante.



Otra opción es moverlo a la última arrastrándolo y entonces en el anteúltimo paso colocar lo siguiente:

```
postman.setNextRequest(null)
```



## Postman Monitors

Los Monitors de Postman nos permite correr los llamados a los endpoints de manera automática ahorrandonos ese click que hemos visto que tenemos que hacer en el collection runner. Podremos establecer con qué frecuencia queremos correrlos, en qué horarios, etc.

Monitos > Create a monitor

Use a collection: Debemos especificar allí la colección.

Luego se ejecutará automáticamente y obtendremos notificaciones por email si algo no funciona bien.

Esta ejecución se lleva a cabo en la infraestructura de Postman por lo que está completamente desacoplado del navegador o nuestra computadora.

Tener presente que a la hora de crear monitores estaremos compartiendo la colección con Postman, por lo que en las variables de entorno accederá al `INITIAL_VALUE`

Si todo funciona bien nos aparecerá como `HEALTHY`

Tanto con monitores como con collection runner resulta complejo el debug ya que no aportan mucha información, pero ante dudas siempre debemos apuntar a variables de entorno o tokens que no reciben su valor.

# Newman

Newman es una herramienta CLI que a partir de una colección de Postman ejecuta todos los tests y genera un reporte. Es posible ejecutar una colección de Postman con API tests en un servidor profesional que se encarga de building y testing de software (jenkins, gitlab ci).

Para utilizarlo localmente debemos tener instalado Node.js. 

```
newman --version
```

Primero debemos exportar la colección y esto podemos hacerlo de distintas formas

* Haciendo click en `...` y luego en Export como JSON. 
* Haciendo click en `...` luego en Share y Get public link. Tendremos que presionar luego Update Link si queremos traernos los últimos cambios las veces posteriores.
* Utilizando la API de Postman.



```
newman run postman_collection.json
```

```
newman run https://getpostman.com/collections/...
```



Tener en cuenta que si utilizamos environments debemos exportarlos y especificarlos también.



## HTML reports con Newman

Newman nos permite generar reportes que contienen la request y respuesta completas.

newman-reporter-htmlextra es una de las opciones mas elegidas





```
npm install -g newman-reporter-htmlextra
```

```
newman run collection.json --reporters cli,htmlextra
```

Notar que queremos seguir generando el reporte por cli

En esa misma carpeta se crerá una carpeta con el reporte llamada newman.



# CI/CD

En Gitlab CI podremos implementar un Pipeline que tenga los siugientes steps:



* Build

* Test (code quality, smoke test, unit tests)

* Deploy (deploy)

* Post deploy (api testing) tendremos un `newman run ...`

* Publishing



# Conexión a DB

:link: Basado en [el video](https://youtu.be/aDvM9CxDw1E) de Valentin Despa

En ocasiones puede que tengamos que chequear algo en una base de datos (por ejemplo si tenemos un endpoint para la creación de un recurso pero no uno para chequear que la data fue ingresada correctamente). 

La forma mas simple sería agregar el endpoint faltante en la API de modo que desde Postman podamos consumir el endpoint nuevo.

**Aclaración:** Siempre que probamos una API nos queremos abstraer de las capas inferiores como ser la DB de hecho, acoplar el testeo de la API con la implementación interana incluso es considerado un anti-patrón, pero asumimos que existe alguna razón importante por la cual debemos hacerlo.



Necesitaremos un Middleware de modo que Postman le hable en HTTP a ese middlware y el middleware se comunique con la db de acuerdo al protocolo de esa base de datos.



Existen soluciones open source y otras enterprise como DreamFactory, Hasura.



# Mock servers

Los mock servers nos permiten simular endpoints y sus correspondientes respuestas sin la necesidad de tener un backend.
