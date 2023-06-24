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
