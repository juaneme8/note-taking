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

## 

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
INSERT INTO person (
	first_name,
	last_name,
	gender,
	date_of_birth)
VALUES ('Juan', 'Ocho', 'MALE', DATE '1986-12-06');
```

Notar que no es necesario especificar el id pues contamos con el autoincrement.

Notar que a la hora de definir `date_of_birth` colocamos `DATE 'YYYY-MM-DD'` y no un string.

Consideramos que la persona no tiene un email por ese motivo no se lo pasamos.
