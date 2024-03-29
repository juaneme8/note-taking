# PostgreSQL

:link: [Learn PostgreSQL Tutorial - Full course for beginners](https://youtu.be/qw--VYLpxG4)



## Introducción

PostgreSQL es una de las bases de datos más populares en la actualidad, en su [página](https://www.postgresql.org/) se define como un object-relational database management system y con mas de 35 años de desarrollo. Entre sus características principales podemos decir que es open-source, robusta, ofrece alta performance y tiene muchas funcionalidades que la hacen elegida por muchas startups para sus backends. 

Existen muchos otros motores de bases de datos como Oracle, MySQL, SQL Server, etc. Por ejemplo si comparamos a Oracle con PostgreSQL con este último no tendremos la necesidad de pagar por una licencia.



Durante el aprendizaje no utilizaremos ninguna herramienta gráfica con el propósito de conocer cómo funciona la lógica por detrás, es por eso que utilizaremos la terminal con una shell interactiva llamada pSQL. Esto nos permitirá que el día que tengamos que hacer SSH a un servidor remoto tener los conocimientos necesarios para operar normalmente sobre la db.



PostgreSQL usa SQL como lenguaje de queries, como parte del aprendizaje esperamos dominar los siguientes aspectos:

* Crear databases
* Crear tablas
* CRUD de elementos
* Primary / Foreign keys
* Join de tablas
* Sequences
* Grouping
* Aggregation
* Exportar a CSV.
* Contraints
* Evitar garbage data.



## DB

Una base de datos es una lugar donde almacenamos, manipulamos y del cual obtenemos datos almacenados y normalmente están dentro de un servidor.



## PostgreSQL

PostgresSQL es el conjunto entre Postgres que es el motor de la DB y SQL (structured query language) como lenguaje de programación para interactuar con la db.



## SQL

SQL es un lenguaje para manejar datos de una db relacional. 

Existe desde 1974.

Los datos son almacenados en tablas formadas por columnas y filas.

Por ejemplo podríamos tener una tabla `person` y columnas `id`, `first_name`, `last_name`, `gender` y `age`. Luego las filas serían los datos de la tabla. 



| id   | first_name | last_name | gender | age  |
| ---- | ---------- | --------- | ------ | ---- |
| 1    | Juan       | Ocho      | Male   | 36   |



En las bases de datos relacionales los datos son estructurados en distintas tablas que pueden tener relación entre sí. Por lo que podríamos tener nuestra tabla `person` vinculada con una tabla `car`.



| id   | first_name | last_name | gender | age  | car_id |
| ---- | ---------- | --------- | ------ | ---- | ------ |
| 1    | Juan       | Ocho      | Male   | 36   | 24     |



| id   | make    | model | price |
| ---- | ------- | ----- | ----- |
| 24   | Peugeot | 208   | 25    |



# Instalación

Podremos descargar PostgreSQL para Windows de [esta página](https://www.postgresql.org/download/windows/) donde al hacer click en  Download the installer nos redireccionará a otra página donde elegiremos la versión mas reciente de Windows x86-64. A continuación lo instalamos de manera habitual destildando en el último paso cuando nos pide descargar cosas extra de Stack Builder).

Podremos conectarnos al servidor de base de datos de distintas formas:

* GUI client (DataGrip, Postico, pgAdmin)
* Terminal (pSQL)
* Aplicación



# Conexión con psql

Hacemos click en SQL Shell (psql) y nos aparecerán estas opciones y con ENTER aceptamos la opción por defecto

Server [localhost]: 
Database [postgres]: (es creada automáticamente con la instalación si pusieramos un nombre de una inexistente obtendríamos error)
Port [5432]:
Username [postgres]:
Contraseña para usuario postgres:



# Conexión con pgAdmin 4

En el apartado Browser veremos Servers, al desplegarlo clickeamos sobre PostgreSQL 15 y nos pedirá la contraseña.



## Agregar psql a PATH

De modo que desde la línea de comandos podamos ingresar pql y nos lo reconozca como comando debemos agregar la dirección de su ubicación en la variable de entornoPATH.

Para eso buscamos la ubicación de `psql.exe` que en mi caso es `C:\Program Files\PostgreSQL\15\bin` y luego nos dirigimos a Edit the system environment variables luego a Environment Variables... Luego en el apartado de variables for "username" elegimos Path y luego click en Edit... donde clickeamos New y agregamos la ruta anterior. 

Luego en la terminal ingresamos `psql` pero tener en cuenta que al iniciar nos pedirá la contraseña asociada al usuario de la sesión y cuando instalamos PostgreSQL colocamos la clave para el usuario postgres. Por lo tanto ejecutamos `psql -U postgres` y luego la clave definida al momento de instalar.



# Comandos

Todos los comandos comienzan con `\`

* `\?` para obtener ayuda sobre todos los comandos que podemos aplicar.

* `\q` para salir



# Bases de datos

## Listar dbs

```
\l
```



## Crear db

```
CREATE DATABASE test;
```

Si bien es posible utilizar minúsculas también, es aconsejable utilizar mayúsculas para distinguir rápidamente los comandos SQL.

El comando debe tenerminar con un `;` para ejecutarse.



## Conexión a db

Podemos conectarnos a una base de datos de dos formas:

```
psql -h localhost -p 5432 -U localhost test
```

Sólo podremos conectarnos a una base de datos si todos los datos son correctos: usuario, puerto y el nombre coincide con una existente de lo contrario obtendremos un error.



Otra forma de hacerlo es si ya estamos dentro de `psql` ejecutando:

```
\c test
\c another
```

Si estando en una db queremos cambiar a otra bastará con ejecutar: `\c another`.



## Eliminar db

```
DROP DATABASE test;
```

Tener presente que este es un comando muy importante ya que ocasionará una pérdida de todos los datos, por lo que debemos usarlo con cuidado y siempre contar con un backup.



# Tipos de datos

En la [documentación](https://www.postgresql.org/docs/current/datatype.html) nos encontramos los distintos tipos de datos:

* `int8` entero signado de 8 bytes.
* `int` o `int4` entero signado de 4 bytes.
* `serial` entero de 8 bytes con autoincremento.
* `bool`
* `char(n)` string de longitud fija.
* `varchar(n)` string de longitud varialbe y con un máximo de caracteres.
* `money`
* `json`
* `date`
* `text` string de longitud variable sin un largo máximo.
* `time`
* `timetzz` incluye time zone.
* `uuid`



# Tablas

## Creación de tabla

```
CREATE TABLE table_name (
	Column name + data type + constraints if any
);
```



## Tabla sin constraints

Por ejemplo si queremos crear una tabla `person`:

```sql
CREATE TABLE person (
	id INT,
	first_name VARCHAR(50),
	last_name VARCHAR(50),
	gender VARCHAR(6),
	date_of_birth DATE,
);
```

> Notar que podremos ir presionando ENTER para ingresar las distintas líneas del comando ya que sólo se enviará cuando coloquemos un punto y coma.

> Con `VARCHAR` se indica que esa columna almacenará caracteres y luego se especifica la cantidad permitida.
>



## Listar todas las tablas

Si queremos obtener un listado de todas las tablas:

```
\d
```

> Asociar `\d` con describe.



## Describir una tabla

Si queremos obtener info sobre una tabla en particular:

```
\d person
```

Veremos cada una de las columnas junto con información extra: `type` con el tipo, `collocation`, `nullable` que indica si admite valor null y `default` que indica si tiene un valor por defecto.



## Eliminar tabla

Con el propósito de crear una tabla con contraints (y volver a llamarla igual a la existente) a continuación introducimos el comando necesario para borrar una tabla:

```sql
DROP TABLE person;
```



## Tabla con constraints

La tabla creada anteriormente admite introducir elementos que no tengan ninguna de las columnas, lo cual no tiene sentido. Cuando queremos asegurarnos que los datos insertados tengan ciertas características especificamos contraints a la hora de la creación de la tabla.

```sql
CREATE TABLE person (
	id BIGSERIAL NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	gender VARCHAR(6) NOT NULL,
	date_of_birth DATE NOT NULL,
  email VARCHAR(150),
);
```

Con `BIGSERIAL` indicamos que id será un entero único y con valores secuenciales debido al autoincremento.

La constraint `PRIMARY KEY` nos asegura que los valores serán únicos y no null. Además crea automáticamente un índice para búsqueda y obtención de datos mas veloz.

El email es un valor que admite valor `null`.



* Con `\d` veremos **dos resultados** uno de ellos de `type` `table` (la tabla `person`) y otro `sequence` vinculada a la secuencia de `BIGSERIAL`.
* Con `\dt` veremos sólo la tabla.

Luego con `\d person` veremos info sobre la tabla, sus valores admitidos, índices, secuencias, etc.



## Insertar records

```sql
INSERT INTO person (first_name, last_name, gender, date_of_birth)
VALUES ('Juan', 'Ocho', 'MALE', date '1986-12-06');
```

> Notar que no especificamos el `id` pues es gestionado mediante la secuencia con auto-increment.
>
> Notar que a la hora de definir `date_of_birth` colocamos `date 'YYYY-MM-DD'` y no un string.

Consideramos que la persona no tiene un email por ese motivo no se lo pasamos. 



```
INSERT INTO person (first_name, last_name, gender, date_of_birth, email)
VALUES ('Anne', 'Smith', 'FEMALE', date '1987-12-06', 'anne@gmail.com');
```



## Generar Mockdata

Utilizaremos [Mockaroo](https://www.mockaroo.com/) para generar 1000 filas.

* Para el `email` establecemos un blank de 30%, por lo que ese porcentaje de personas no tendrán un mail aprovechando que la columna es nullable.
* Para la fecha elegimos el tipo Datetime en este caso y seleccionamos el formato yyyy-mm-dd.
* Agregamos también la columna `country_of_birth` .
* La columna de `id` la agregaremos a mano directamente en el script mas adelante.



:file_folder: Luego seleccionamos el formato de descarga como SQL, le damos el nombre person y tildamos include CREATE TABLE.

:arrow_down: Por último descargamos el archivo `person.sql` (para lo cual nos hará autenticar) y desde VS Code podremos agregar los `NOT NULL` para todos los campos menos para el `mail` , la cantidad máxima de caracteres deseada para los `VARCHAR` y la columna `id BIGSERIAL NOT NULL PRIMARY KEY`.

:page_facing_up: Si bien podríamos copiar y pegar todos los comandos, lo que haremos será ejecutar el comando directamente desde el archivo.



```
 \i /Users/juan.lauria/Desktop/person.sql
```





## Listar records

```
SELECT * FROM person;
```



Si queremos una columna en particular:

```
SELECT first_name FROM person;
```



## `ORDER BY` keyword

Es posible ordenar los resultados recibidos en una columna utilizando el keyword `ORDER BY`.

```
SELECT * FROM person
ORDER BY country_of_birth;
```

En este caso estaremos ordenando ascendentemente (a-z), sería lo mismo que colocar `ORDER BY country_of_birth ASC` que es valor por defecto. Si queremos un ordenamiento descendiente (z-a) tendremos que ingresar: `ORDER BY country_of_birth DESC;`



## `DISTINCT` keyword

Si queremos obtener los países de todos los elementos de la tabla podríamos ejecutar:

```
SELECT country_of_birth from person ORDER BY country_of_birth;
```

Esto nos devolvería un listado donde se repetiría cada país tantas veces como aparezca en la tabla.

Si queremos en cambio obtener en cambio un resultado por cada pais, podemos utilizar el keyword `DISTINCT`:

```
SELECT DISTINCT country_of_birth from person ORDER BY country_of_birth; ;
```



## `WHERE` clause

Utilizando `WHERE` podremos filtrar los resultados basados en condiciones:

```
SELECT * FROM person WHERE gender='Female';
```



# 

## Logical operators `AND` `OR`

Los operadores lógicos son `AND`, `OR` y `NOT` y podremos usarlos a la hora de filtrar los datos:

```
SELECT * FROM person WHERE gender='Female' AND (country_of_birth='Poland' OR country_of_birth='Argentina');
```



## Comparison operators

A continuación presentamos los operadores de comparación:

* `=`, `<>` (not equal)
* `<`, `>`, `<=`, `>=`



> Cuando queremos chequear una condición podremos ejecutar `SELECT 1 = 1;` en este caos obtendremos `t` (lo cual hace referencia a `true`)



## `LIMIT` keyword

`LIMIT` nos permitirá seleccionar las primeras n filas.

```
SELECT * FROM person LIMIT 10; 
```



## `OFFSET` keyword

En ocasiones querremos recibir las filas a partir de un cierto número, por ejemplo si queremos después de la fila 5 podremos colocar:

```
SELECT * FROM person OFFSET 5; 
```

Combinando `LIMIT` y `OFFSET`, para recibir 10 resultados después de la fila 5:

```
SELECT * FROM person OFFSET 5 LIMIT 10; 
```



## `FETCH` keyword

Hemos usado `LIMIT` pero no es una keyword del standard de SQL, como es utilizada en mútiples dbs por lo que terminó siendo aceptada. El modo de hacerlo en Postgres es con `FETCH`.

```
SELECT * FROM person OFFSET 5 FETCH FIRST 5 ROW ONLY; 

SELECT * FROM person OFFSET 5 FETCH FIRST ROW ONLY; 
```



## `IN` operator

El operador `IN` lo utilizamos en el `WHERE` clause para chequear contra una lista de valores. Esto podríamos hacelro utilizando `OR` como vemos a continuación, pero para cada valor habría que escribir bastante:

```
SELECT * FROM person 
WHERE country_of_birth='China'
OR country_of_birth='France'
OR country_of_birth='Brazil';
```



Con el operador `IN` esto mismo puede escribirse de manera reducida 

```
SELECT * FROM person WHERE country_of_birth IN ('China', 'France', 'Brazil');
```



## `BETWEEN` operator

El operador `BETWEEN` nos permite elegir filas entre un rango de valores.

```
SELECT * FROM person 
WHERE date_of_birth 
BETWEEN DATE '2000-01-01' AND '2015-01-01'; 
```



## `LIKE` operator

El operador `LIKE` nos permite matchear valores de texto contra un patrón usando comodines.

Por ejemplo si queremos encontrar todos los emails que terminan en ".com":

```
SELECT * FROM person
WHERE email LIKE '%.com'
```



En estos casos el `%` representa uno o mas caracteres, pero si sabemos que son una cantidad determinada podemos usar `_` guión bajo donde cada uno que representa un caracter.



## `ILIKE` operator

El operador `ILIKE` es similar a `LIKE` pero ignora la capitalización.



## `GROUP BY`

`GROUP BY` nos permite agrupar datos basándonos en una columna. 

Si queremos obtener un listado de todos los países que tenemos:

```
SELECT DISTINCT country_of_birth FROM person;
```



Si queremos saber también cuánta gente tenemos de cada país:

```
SELECT country_of_birth, COUNT(*) FROM person
GROUP BY country_of_birth;
```

