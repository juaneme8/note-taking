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



1h38

```
pm.expect(1).to.be.above(2)
```

