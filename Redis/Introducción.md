# Redis

Redis es una herramienta que almacena estructuras de datos en memoria y puede ser **usado como base de datos NoSQL, como cache distribuido o como message broker.** 



#### Estructuras de Datos

En la [documentación](https://redis.com/redis-enterprise/data-structures/#:~:text=Redis%20Bitmaps%20is%20a%20compact,operations%20between%20multiple%20bitmap%20keys.) podemos ver los distintos tipos de datos que podemos almacenar en Redis, estos aseguran adaptarse mucho mas a las aplicaciones modernas que el clásico par key-value. 



#### Arquitectura

En cuanto a la arquitectura nos encontramos con un servidor y el cliente o maestro y esclavo para sistemas distribuidos.



#### Ventajas

La principal ventaja de redis es la **velocidad** siendo capaz de una cantidad enorme de escrituras y lecturas por segundo. También soporta un pipeline de comandos y también la configuración de múltiples valores en un único comando. 

En cuanto a la **persistencia de datos**, Redis tiene toda la información en memoria pero los cambios son guardados en disco de manera asincrónica.

Los **tipos de datos** con los que trabaja Redis tambipen son una ventaja.

Redis cuenta con **transacciones atómicas** lo cual significa que o se ejecutan todos los comandos o ninguno de ellos lo hará.

Proporciona múltiples utilidades para distintos casos de uso como manejo de cache, messaging queues, short lived data (web application sessions, web page hit counts).



## Instalación

Redis no soporta oficialmente Windows, así que es necesario que nos apoyemos en los paquetes del **Microsoft Open Tech Group** en GitHub, en el [repositorio de Redis](https://github.com/microsoftarchive/redis/releases).

La [documentación](https://redis.io/docs/getting-started/installation/install-redis-on-windows/) en cambio recomienda utilizar WSL2 con Ubuntu y ejecutar una serie de comandos allí descriptos.



```
redis-server
redis-cli
```



## Strings

Podemos guardar un string de la siguiente forma

```
set name Juan
```

Luego lo obtenemos con 

```
get name
```





Si queremos obtener determinados caracteres de un string:

```
getrange name 0 3
```



Si queremos setear mas de un string a la vez:

```
mset lang english tehnology redis
```

Luego para obtener uno de ellos o ambos:

```
mget lang
mget lang technology
```



Si quermeos obtener la cantidad de caracteres:

```
strlen name
```

Si queremos actualizar un string

```
set name "Another Name"
```



#### Almacenar enteros:

```
set count 1
get count
```



##### Incrementarlos en una o mas unidades

```
incr count
incrby count 2
```

##### Decrementarlos en una o mas unidades

```
decr count
decrby count 10
```



#### Almacenar valor de punto flotante