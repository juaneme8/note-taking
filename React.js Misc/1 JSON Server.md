# JSON Server

> Basado en un [video](https://www.youtube.com/watch?v=MnIEJMgvuvc) de la lista de reproducción The Net Ninja sobre Material-UI
>
> Basado en [lista de reproducción de Codevolution](https://youtu.be/_1kNqAybxW0?list=RDCMUC80PWRj_ZU8Zu0HSMNVwKWw)

## Introducción

Una tarea habitual de los desarrolladores frontend consiste en simular un backend con una API REST que nos devuelva mock data en formato JSON para prototipar componentes de nuestra aplicación.
Si bien podríamos crear el server backend usando Node, Express y MongoDB esto nos insumirá bastante tiempo y con JSON Server podemos crear un *local server* con una *fake REST API* con cero código. manipulamos asincrónicamente la información utilizando endpoints GET, POST, PUT, DELETE.

Se trata de un archivo `.json` que usaremos como base de datos y podremos no sólo hacer un `GET` para obtener todos elementos sino también podremos listar por id, filtrar, ordenar, paginar, listar según operación (`>`, etc), listar por texto, listar elementos padre o hijos y hacer también solicitudes de tipo `POST`, `PUT` y `DELETE`. 

## Configuración

En primer lugar debemos asegurarnos tener un `package.json`  cosa que hacemos con `npm init -y`

Luego debemos instalar json-server propiamente dicho:

```bash
npm install json-server
```

A continuación agregamos un script en `package.json` de manera tal para que json-server sirva los datos siempre actualizados (`--watch`).

```
"scripts":{
	"serve-json": "json-server --watch db.json"
}
```

> Si ya tenemos ocupado el puerto 3000 como suele suceder con las aplicaciones de React debemos especificarle un nuevo puerto y lo hacemos del siguiente modo: `json-server --watch data/db.json --port 8000`.

Si ejectamos `npm run serve-json` y nos dirigimos a `localhost:3000` veremos la página de inicio de JSON Server lo cual nos demuestra que todo está funcionando según lo esperado.



## Primeros Pasos

En el directorio raíz creamos un archivo `db.json`

Luego en el archivo creamos un objeto en formato JSON con aquellos datos que queremos obtener mediante endpoints.



```json
{
	"products": [
		{
		"id":1,
		"title":"Product 1",
		"category":"electronics",
		"price":4000,
        "description":"This is description about product 1"
		},
		{...}
	],
    "reviews":[
    	{
        "id":1
        "rating": 3
        "comment": "Review 1 for product 1"
        "productId": 1
        },
        {...}
    ]
}
```

> Notar el uso de comillas dobles tanto para las propiedades como para los valores que son strings y que el último elemento del array no debe tener una `,` después de el.




Las *top level properties* como en este caso `products` y `reviews` son considerados *resources* y tendrán endpoints asociados para obtener y agregar datos. 



# Petición `GET`

Para obtener todos los datos de los producto podremos hacer un `GET` a `http://localhost:3000/products`

Para obtener un producto en particular hacemos referencia a su `id` `http://localhost:3000/products/1`



## Ejemplo con React

Para hacerlo programáticamente usando `useEffect` y `fetch()`

```jsx
import React, { useEffect, useState } from 'react'

export default function Notes() {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    fetch('http://localhost:8000/products')
      .then(res => res.json())
      .then(data => setProducts(data))
  }, [])

  return (
    <div>
      {products.map(product => (
        <p key={product.id}>{ product.title }</p>
      ))}
    </div>
  )
}
```

Si queremos implementar un POST por ejemplo cuando el usuario ingresa datos en un formulario y presiona el *submit button*. 

```jsx
const history = useHistory();
const handleSubmit = (e) => {
	e.preventDefault()
	fetch('http://localhost:8000/products', {
		method: 'POST',
		headers: {"Content-type": "application/json"},
		body: JSON.stringify({ title, details, category })
	})
	.then(() => history.push('/'));
}
```

> Notar el uso de `JSON.stringify({})` para convertir los objetos JavaScript a strings JSON.
>
> El id será asignado de manera automática .
>
> Para redireccionar una vez enviado el formulario hacemos uso del hook `useHistory()` que importamos de la siguiente forma `import  {  useHistory  }  from  'react-router-dom'`



## Filtering

A la hora de filtrar elementos devueltos por la API de acuerdo al valor de alguna de las propiedades podemos hacerlo utilizando query parameters. 

Si queremos obtener los elementos que tienen la propiedad `category`  con el valor "electronics" podemos hacerlo con:

`http://localhost:8000/products?category=electronics`



Si ahora queremos filtrar los elementos de la categoría electrónica que además tienen descuento en envío, como por ejemplo el elemento que vemos a continuación:

```
[
	{
		"id":10,
		"title":"Product 10",
		"category":"electronics",
		"price": 3000,
		"description":"This is the description about product 10",
		"discount":{
			"type":"shipping"
		}
	}
]
```

En ese caso deberíamos hacer un GET a la direccción: `http://localhost:8000/products?category=electronics&discount.type=shipping`



## Sorting

Cuando trabajemos con tablas o grids es probable que necesitemos contar con un endpoint que nos entregue los datos ordenados con un cierto criterio.

Por ejemplo si queremos obtener los productos ordenados por precio de menor a mayor (asc)

```
http://localhost:8000/products?_sort=price
```

Si queremos orden descendente:

```
http://localhost:8000/products?_sort=price&_order=desc
```

Si queremos ordenar por más de una categoria, por ejemplo por precio (en descendente) y por categoria (en ascendente):

```
http://localhost:8000/products?_sort=price,category&_order=desc,asc
```



## Paginación

Es posible implementar la paginación de los resultados devueltos por JSON Server. Por defecto las páginas son de 10 elementos.

Si queremos obtener la primera página (de 10 elementos por default) debemos usar la siguiente petición:

```
http://localhost:8000/products?_page=1
```

Si queremos obtener la primera página pero que sea de 5 elementos:

```
http://localhost:8000/products?_page=1&_limit=5
```



### Paginación con URLs

Si al ingresar a la URL con las DevTools en **Network** vamos a **Headers**  veremos que dentro de **Link**

tenemos links para la página `first`, `prev`, `next`, y `last`. Podremos acceder a ellos con `response.headers.get("Link")`



## Operadores

Contamos con algunos operadores que nos permiten obtener datos de acuerdo a una condición:

* `_gte` (greater than or equal)
* `_lte` (less than or equal)
* `_ne` (not equal to)
* `_like` este operador nos permite especificar RegEx para filtrar datos.

Si queremos obtener los productos cuyo precio está entre 2000 y 6000 debemos hacer la siguiente petición:

```
localhost:3000/products?price_gte=2000&price_lte=6000
```

Si queremos obtener todos los productos cuyo `id` no es 1:

```
localhost:3000/products?id_ne=1
```

Si queremos filtrar todos los productos que comienzan con la letra f:

`localhost:3000/products?category_like=^f`



## Full-text Search

Si queremos realizar una búsqueda de un texto en cualquier campo del JSON podemos hacerlo con el parámetro `q`. 

Por ejemplo si queremos buscar el texto "in" podemos hacerlo con:

```
localhost:3000/products?q=in
```



## Relaciones

Las relaciones nos permiten hacer un JOIN entre dos tablas de una base de datos y retornar los datos resultantes.

Con `_embed` obtenemos recursos de los elementos hijos:

En el primer ejemplo veremos como incluir recursos de hijos al hacer un query sobre el padre. Por ejemplo si queremeos obtener los productos con sus correspondientes reviews. En nuestro caso es el parámetro `productId` el que vincula los reviews con el producto.

```
localhost:3000/products?_embed=reviews
```

En este caso veremos que a products le agregará la propiedad `reviews` retornando un array de elementos dentro de cada producto.



Esto también funciona con productos individuales:

```
localhost:3000/products/1?_embed=reviews
```



Con `_expand` obtenemos recursos del padre:

Ahora veremos como incluir el padre al hacer un query de elementos hijos. En nuestro caso incluir información del producto al hacer un query del padre:

```
localhost:3000/reviews?_expand=product
```

> Notar que ponemos `product` en singular.



```
localhost:3000/reviews/1?_expand=product
```



# Petición `POST`

Si queremos realizar una petición `POST` podremos hacerlo utilizando fetch API desde el frontend pero si así lo queremos podemos usar otras herramientas como:

* Postman
* REST Client (extensión Visual Studio Code)
* Thunder Client (extensión Visual Studio Code)



Haremos una petición a `http://localhost:3000/products` luego en **Body** dentro de **JSON Content** colocaremos el elemento que queremos insertar.

```
{
	"id":11,
	"title":"Product 11",
	"category":"electronics",
	"price": 4000,
	"description":"This is description about product 11"
}
```



En ese momento obtendremos un 201 y nos devolverá el objeto creado.



# Petición `PUT`

Si queremos modificar el contenido de una propiedad debemos hacer una solicitud al id en particular de ese elemento:

```
http://localhost:3000/products/11
```

Luego en **Body** dentro de **JSON Content** colocaremos el objeto completo con la propiedad que hemos modificado.

```
{
	"id":11,
	"title":"Product 11",
	"category":"furniture",
	"price": 4000,
	"description":"This is description about product 11"
}
```



# Petición `PATCH`

De manera similar a lo visto anteriormente utilizamos la petición `PATCH` cuando queremos modificar sólo una propiedad.

```
http://localhost:3000/products/11
```

Luego en **Body** dentro de **JSON Content** colocaremos la propiedad que hemos modificado.

```
{
	"price":"8000"
}
```



# Petición `DELETE`

Cuando queremos eliminar un elemento utilizamos la petición `DELETE` por ejemplo:

```
http://localhost:3000/products/11
```



VIDEO 10 COMPLETO
