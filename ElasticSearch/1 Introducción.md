# ElasticSearch

> Basado en el curso de Platzi de Introducción a ElasticSearch

## Introducción

ElasticSearch nos permite realizar búsquedas y analíticas en tiempo real lo cual representa una gran ventaja cuando trabajamos con volúmenes de datos muy grandes.

Funciona con una interfaz HTTP y mediante documentos JSON tanto para hacer las consultas como para entregar la información.

> Luego de guardado un documento es indexado y buscable luego de aproximadamente 1 segundo (en caso de necesitarlo inmediatamente podemos utilizar `?refresh`).



Dentro de los casos de uso tenemos la búsqueda de información en una aplicación web,  motor de almacenamiento, machine learning, manejo de información geoespacial, etc.



A **nivel lógico** ElasticSearch está conformado por **índices** y **documentos**. Los índices almacenan documentos y los documentos son la unidad de información.

A **nivel físico** ElasticSearch está formado por un **cluster** que involucra a todos los **nodos** siendo estos todos las máquinas corriendo ElasticSearch. Los nodos están formados por shards o piezas que componen el índice. Tendremos la pieza primaria y las réplicas. Las réplicas almacenan información como backup de los otros nodos, para permitir restablecer la información en caso de problemas con uno o mas nodos.

![Elasticsearch Shards: Definition, Sizes, and Optimization - Dattell](https://dattell.com/wp-content/uploads/2019/01/elasticsearch-cluster2-1024x770.png)

## Instalación con Docker

Creamos un directorio `md-elastic` y dentro un archivo `docker-compose.yml`.

Nos basamos en la [documentación oficial](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html) y lo modificamos para trabajar con un nodo (así nos queda de manera similar a lo indicado en el curso pero con la última versión).

```yaml
version: '2.2'
services:
  es01:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.6.0
    container_name: es01
    environment:
      - node.name=es01
      - cluster.name=es-docker-cluster
      - cluster.initial_master_nodes=es01
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data01:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - elastic

volumes:
  data01:
    driver: local

networks:
  elastic:
    driver: bridge

```

> Le dice a Docker que use la imagen de Elasticsearch con versión 7.6.0.
>
> Le dice que el nombre del contenedor será es01.
>
> Le indica unas configuraciones para crear el cluster y el nodo por defecto.
>
> Le indica que el puerto sobre el que corre Elasticsearch es el 9200. A su vez le dice que exponga ese mismo puerto a tu máquina para que puedas usar el servicio bajo el puerto estándar.



Luego debemos levantar el servicio con `docker-compose up`

Debemos seguir este paso indicado en la documentación utilizando Cmder:

>Windows with Docker Desktop WSL 2 backend

> The `vm.max_map_count` setting must be set in the docker-desktop container:

```
wsl -d docker-desktop
sysctl -w vm.max_map_count=262144
```

Para comprobar que Elasticsearch ya está corriendo sobre tu máquina, abres Postman y ejecutas un GET sobre la url [http://localhost:9200](http://localhost:9200/) y tenemos que recibid un objeto como el siguiente:

```json
{
    "name": "es01",
    "cluster_name": "es-docker-cluster",
    "cluster_uuid": "Dmt7oCbMR5ap_bxnivddSQ",
    "version": {
        "number": "7.15.1",
        "build_flavor": "default",
        "build_type": "docker",
        "build_hash": "83c34f456ae29d60e94d886e455e6a3409bba9ed",
        "build_date": "2021-10-07T21:56:19.031608185Z",
        "build_snapshot": false,
        "lucene_version": "8.9.0",
        "minimum_wire_compatibility_version": "6.8.0",
        "minimum_index_compatibility_version": "6.0.0-beta1"
    },
    "tagline": "You Know, for Search"
}
```



## Índices y Documentos

### Creación de un documento

La creación de un nuevo documento podremos hacerla desde Postman tanto con el verbo `POST` como con `PUT`. 

Vamos a trabajar sobre el índice `users` esto significa que las peticiones serán a `localhost:9200/users` y como éste no existe, ElasticSearch lo creará para nosotros.

En el caso de utilizar `POST` el id será asignado automáticamente y utilizaremos la URL `localhost:9200/users/_doc`.
Luego en **Body** elegimos **raw (JSON)** y completamos lo siguiente:

```
{
	"name":"juan",
	"user":"@juaneme8"
}
```

> No necesariamente en todas las peticiones tendremos que incluir estos mismos campos, sino que podríamos por ejemplo poner sólo `name`.
>
> Luego de presionar Send obtendremos el documento creado y veremos que tiene una propiedad `_id` de la forma `QXMN4nwBk3wgrMfGMY07`.
>
> En el documento devuelto veremos `"_version": 1,` lo cual nos da cuenta de que ElasticSearch maneja versiones de los documentos. Esto significa que si hacemos un `PUT` como veremos más adelante a ese id nos devolverá `"version: 2"`.

Como salida obtendremos:

```
{
    "_index": "users",
    "_type": "_doc",
    "_id": "RnNV4nwBk3wgrMfG1o1o",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 1,
    "_primary_term": 1
}
```



En caso de usar `PUT` la URL será por ejemplo `localhost:9200/users/_doc/1` con esto indicamos que vamos a trabajar sobre el documento con id 1. Esto puede ser interesante si queremos que el documento en ElasticSearch tenga el mismo id que la base de datos.

En este caso como salida obtendremos

```
{
    "_index": "users",
    "_type": "_doc",
    "_id": "1",
    "_version": 1,
    "result": "created",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 0,
    "_primary_term": 1
}
```



#### Parámetro `refresh`

En caso de tener la necesidad de obtener el documento para las búsquedas inmediatamente podemos agregar el parámetro opcional `refresh`, de modo tal que la URL nos quedará (suponiendo un `POST`) `localhost:9200/users/_doc?refresh` . Veremos que el documento devuelto tiene la propiedad `"forced_refresh": true`.



### Creación de Múltiples Documentos

Creamos un archivo `users.json` con el siguiente contenido:

```
{"index":{"_id":3}}
{"name":"Beth", "user":"@beth"}
{"index":{"_id":4}}
{"name":"Mark", "user":"@mark "}

```



> Debemos terminar el archivo con un salto de línea.

Luego en Postman hacemos un `POST` a `localhost:9200/users/_bulk` y en **Body** no será raw (JSON) sino **binary** y luego elegimos el archivo `users.json`. Por último enviamos la consulta con Send.

Luego obtendremos un JSON que nos indicará si hubo algún error en el campo `errors` y un array de objetos `items` con la salida de cada uno de los documentos creados.



## Listar documentos

### Obtener todos los documentos

Utilizando el verbo `GET` podremos listar los documentos dentro de un índice, realizar una búsqueda o efectuar un mapeo dentro de un índice. Si queremos listar los documentos del índice `users`

```
localhost:9200/users/_search 
```



```
{
    "took": 4,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 8,
            "relation": "eq"
        },
        "max_score": 1.0,
        "hits": [
            {
                "_index": "users",
                "_type": "_doc",
                "_id": "1",
                "_score": 1.0,
                "_source": {
                    "nombre": "juan",
                    "apellido": "ocho"
                }
            },
            (....)
            (....)
            (....)
            (....)
        ]
    }
}
```

Obtendremos como respuesta un JSON con toda la información del índice. Por ejemplo en `hits.total.value` veremos la cantidad de elementos devueltos y en `hits.hits` un array con todos los documentos. Por lo que con `hits.hits[0]._source.name` accederemos al nombre del primer documento.



### Obtener un documento

Hacemos una petición `GET` a `localhost:9200/users/_doc/1`

```
{
    "_index": "users",
    "_type": "_doc",
    "_id": "1",
    "_version": 1,
    "_seq_no": 0,
    "_primary_term": 1,
    "found": true,
    "_source": {
        "nombre": "juan",
        "apellido": "ocho"
    }
}
```

Si no nos interesan todos los metadatos podremos hacer un `GET` a `localhost:9200/users/_source/1` y obtendremos simplemente:

```
{
    "nombre": "juan",
    "apellido": "ocho"
}
```



### Obtener info de índice

Con un `GET` a `localhost:9200/users` obtendremos información sobre ese índice



### Mapeo de índice

Hacemos una petición `GET` a `localhost:9200/users/_mapping`

```json
{
    "users": {
        "mappings": {
            "properties": {
                "name": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                },
                "user": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                }
            }
        }
    }
}
```



## Actualizar un documento

Si queremos actualizar un documento  y lo hacemos con un `PUT` a `localhost:9200/_doc/1` (suponiendo que queremos modificar el documento con id 1)  y colocando en Body raw(JSON) la propiedad que queremos agregar. Perderemos el resto de las propiedades. Lo cual podemos verificar haciendo ahora un `GET`.

```
{
    "_index": "users",
    "_type": "_doc",
    "_id": "1",
    "_version": 2,
    "result": "updated",
    "_shards": {
        "total": 2,
        "successful": 1,
        "failed": 0
    },
    "_seq_no": 2,
    "_primary_term": 1
}
```



La forma correcta de hacerlo es con `POST` a `localhost:9200/users/_update` colocando en Body, raw(JSON)

```
{
	"doc":{
		"name":"juan",
		"user":"@juaneme8",
		"age":34
	}
}
```



## Borrar Documento

Para eliminar un documento lo hacemos con la petición `DELETE` a `localhost:9200/users/_doc/1` 



## Borrar Índice

Es posible borrar todo el índice con un `DELETE` a `localhost:9200/users`



VIDEO 5 COMPLETO
