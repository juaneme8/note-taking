# GraphQL

> Basado en curso The Net Ninja



GraphQL es un *query language* muy poderoso para comunicar datos entre un cliente (el navegador) y el server.

En comparación con el enfoque RESTful es más flexible y eficiente.

En una API REST de libros, si queremos obtener información sobre un libro en particular (cuyo `id` conocemos) y su autor deberíamos primero hacer un `GET` a este endpoint:

```
domain.com/books/:id
```

Donde obtendríamos parte de la información deseada `title`, `genre`, `reviews` y también el `authorId` con el cual obtendríamos la info del autor.

```
domain.com/authors/:id
```

De este último obtendríamos `name` ,`age`, `bio`, `booksIds` (un array con los id de cada libro).

Vemos que sólo para obtener info del libro y del autor hemos tenido que hacer dos requests, esto nos da la pauta que en ciertos casos puede volverse ineficiente. Imaginemos si además queremos mostrar todos los libros escritos por ese autor, en ese caso serían necesarios nuevos requests al endpoint mostrado en primer lugar.



Para obtener estos mismos datos con un enfoque GraphQL haríamos la siguiente query:

```
{
    book(id:123){
        title
        genre
        reviews
        author{
            name
            bio
            books{

            }
        }
    }
}
```



> **Simplicidad**

Con un único HTTP request hemos obtenido toda la información anidada.



> **Selectividad**

Con GraphQL podemos ser **selectivos** en cuanto a los datos que queremos recibir, por ejemplo siguiendo con el ejemplo anterior, si no queremos recibir algunos datos podemos omitirlos en la query:

```
{
    book(id:123){
        title
        author{
            name
            books{
				title
            }
        }
    }
}
```



> **Supercharged Endpoint**

En GraphQL en vez de contar con muchos endpoints contamos con uno sólo recargado 