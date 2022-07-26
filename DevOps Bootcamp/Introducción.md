> Basado en el Bootcamp
>
> :ok: Módulo GIT 21 - 37 COMPLETO

# Introducción

El propósito de este bootcamp es generar los procesos de DevOps con los que tendrá que lidiar un DevOps Engineer



Existen distintos roles IT en lo que es el desarrollo de software.

* Programming (developers)
* Software Testing (developers, dedicated testers, automated testing).
* Release (build application, run on servers, upgrade existing software)
* Operations run software in productions.



La comunicación entre operations y development tiene el objetivo de que cuando los developers crean una nueva feature en un nuevo release esta sea implementada en los servidores de producción sin que haya un downtime.



# Integración Contínua

CI (continuos integration) o Integración Continua hace referencia a integrar los cambios del código frecuentemente, de esta manera si pusheamos y pulleamos regularmente es menos probable que nos enfrentemos a *merge conflicts* difíciles de resolver.



# DevOps Engineer y Git

En primer lugar necesitaremos de Git cuando trabajamos con conceptos de **Infraestructure as Code** si trabajamos con un *developer team* que realizó un deployment en Kubernetes, en ese caso tendremos que crear varios archivos de configuración de Kubernetes (para crear volúmenes, databases, etc ) y debemos manejarlos de algún modo. 

Supongamos además que el cluster de Kubernetes es deployado en AWS, y para manejar el servidor AWS debemos utilizar herramientas de automatización como Terraform y Ansible por lo que también tendremos esos archivos de configuración que manejar. A esto se sumarán scripts de bash y python.

En esos casos tendremos que garantizar que trackeamos el historial de cambios de los archivos, los almacenamos en un lugar seguro y que sea factible la colaboración por parte de todos los miembros del *DevOps team* a la hora de modificar estos archivos.



En segundo lugar cuando trabajamos con **CI/CD Pipeline and Build Automation** como Jenkins debemos hacer hacer checkout del código, test and build application. Debemos integrar la herramienta de automatización con el repositorio de la aplicación.

En ocasiones vamos a necesitar por ejemplo obtener el commit hash de un commit en particular (por ejemplo el último), chequear si hubo cambios en el código del un frontend o backend y así decidir si ejecutar tests, etc.



# Bases de Datos

Las bases de datos como sabemos son utilizadas para persistir información a la hora de crear, leer, actualizar y eliminar datos.



## Implementaciones

En el proceso de desarrollo de software podremos tener bases de datos instaladas localmente o remotamente.

* En el caso de que estén **instaladas localmente** cada desarrollador tendrá que instalar por ejemplo mysql. Esto tiene como ventaja que no podremos arruinar los datos de testing de otro desarrollador y la desventaja de que tendremos que instalar y configurar la DB para cada desarrollador y que iniciaremos de una DB vacía por lo que tendremos que generar datos realísticos.
* En caso de que esté **hosteada de manera remota** y siendo accedida mediante un endpoint y las credenciales apropiadas. Como ventaja no tendremos que instalarla localmente y tendremos los datos disponibles inmediatamente. Como desventaja podremos impactar en el trabajo de otro desarrollador si hacemos algo mal con la DB.

La solución ideal es tener disponible ambas implementaciones, una base de datos local para desarrollo y una hosteada remotamente.



## Variables de Entorno

La aplicación deberá conectarse a cada una de las bases de datos usando la librería o módulo según el lenguaje de programación y el tipo de DB que estemos usando y para ello deberá autenticarse. Para establecer la conexión tendremos que utilizar un endpoint específico de la base de datos y credenciales.

Lo aconsejable es no tener estos datos hardcodeados sino tenerlos definidos en un único lugar como **variables de entorno** esto permitirá configurarlos desde afuera del código y además como tendremos bases de datos de dev, test y prod vamos a querer conectarnos a diferentes dbs de acuerdo al entorno en el que nos encontremos. Teniendo en cuenta que cada db tiene un endpoint y credenciales distintas (debiendo asegurarnos que la de producción sea la mas segura).



## Archivos de Configuración

Es posible pasar las variables de entorno al iniciar la  como parámetros de línea de comandos o editor de código.  Sin embargo, no es práctico ya que todos tendrán que configurarlo y resulta muy engorroso cuando tenemos muchas configuraciones. Lo apropiado es utilizar archivos de configuración para tal fin. Por ejemplo en Node.js tendremos un archivo `config.json` como el siguiente:

```json
{
	"development":{
		"mongodb":{
			"host":"localhost",
			"port":27017,
			"database":"db-name"
		}
	},
	"staging":{
		"mongodb":{
			"host":"mongo-host-staging",
			"port":27017,
			"database":"staging-db-name"
		}
	},
	"production":{
		"mongodb":{
			"host":"mongo-host-prod",
			"port":27017,
			"database":"prod-db-name"
		}
	}
}
```

En este archivo configuraremos los DB endpoints, credenciales, el nivel de logueo personalizado (por ejemplo sólo los errores en producción), endpoints de servicios con los que nos comunicamos, etc.



## Preservación de Datos

Como la preservación de los datos es muy importantes los desarrolladores no suelen manejar las bases de datos de producción y en cambio lo hacen quienes ocupan los roles de **System Administrator** o **DB Engineer** o **DevOps Engineer** que tienen a cargo tareas como la de replicar bases de datos, generar backups regulares, recuperar datos y ser performantes en situaciones de alta demanda.



## Tipos de Bases de Datos 

Existen distintos tipos de bases de datos y la elección de una de ellas dependerá de la aplicación que estemos desarrollando.

* Cassandra
* InfluxDB
* MongoDB
* Cockroach DB
* Elastic Search
* Redis
* Memcached
* MySQL
* PostgreSQL
* Oracle



### Key Value Databases

En este tipo de bases de datos como su nombre lo indica trabajamos con key y value. 

* Redis

* Memcached

* etcd from Kubernetes (son usados para almacenar el estado del cluster en tiempo real)



* Claves Unique.

* Data Model simple.  No tenemos joins ni otros conceptos mas complejos que si tenemos en bases de datos relacionales.
* Almacenan los datos en memoria por lo que son muy rápidos.
* No es posible almacenar muchos datos pero el acceso es muy rápido.
* No son usados como base de datos primaria.

* Son usados como cache o real time delivery of data.

* Aplicación como message nroker (message queue)



### Wide Column

* Cassandra

* HBase



* Contamos con una única key y múltiples columnas.
* Escalable.
* Son menos limitados en cuanto al schema.

* No tienen un schema predefinido.
* Es un lenguaje de queries similar a SQL
* No cuenta con joins, ni otros conceptos complejos de bases de datos relacionales. 
* Es mas simple pero limitada respecto a las db relacionales.
* Es útil para almacenar grandes cantidades de datos no estructurados como historical records, time-series, como ser entradas de dispositivos IoT, sensores, etc. 



### Document Databases

* MongoDB

* DynamoDB

* CouchDB



* Bases de datos basadas en documentos
* Los documentos son agrupados en colecciones
* schema-less
* lecturas mas rápidas que en una db relacional
* escrituras mas lentas  que en una db relacional
* ideal para mobile apps, game apps, cms
* puede ser usado como una db primaria
* util cuando no sabemos cómo estarán los datos estructurados o la forma que tendrán
* no debe ser usado cuando tenemos muchos datos correlacionados que deben ser actualizados frecuentemente.
* denormalized



### Relational Databases

SQL = structured query language

 

* MySQL

* Prostgresql



* Son las mas utilizadas
* Almacenan datos estructurados
* Requieren un squema y especificar los tipos de datos.
* Los datos son organizados en tablas que tienen filas y columnas.
* normalized. Si queremos almacenar comentarios no almacenamos cada comentario acompañado de muchos del usuario sino que tenemos una tabla usuario y una tabla comentario y en esta última indicamos el id `userId`. Evitamos tener datos duplicados de esta forma.
* ACID: atomicity, consistency, isolation, durability. cuando hay una transacción como actualizar datos la consistencia y validez de datos es garantizada, sin importar los inconvenientes tecnicos que sucedan (network, hardware, software). No tendremos cambios a medias en la db o se aplican todos o ninguno de ellos. 
* Útil en aplicaciones bancarias o financieras. Si tengo que actualizar 10 tablas y tras las primeras 5 hay problemas de conexión de red, no se actualizará ninguna, haciendo en dicho caso un *rollback*.
* Son díficiles de escalar. Ejecutarlas en entornos distribuidos como contenedores, clusters. Sin embargo existen alternativas modernas como Cockroach DB que solucionan estos inconvenientes.
* Son tan populares que a la hora de comparar bases de datos hablamos de SQL vs NO-SQL.



### Graph Databases

Las Graph Databases son útiles cuando tenemos relaciones muy complejas many to many (que requieren de muchos joins o una tabla intermediaria). 

Ejemplos de esto podría ser Reddit donde un usuario puede estar en muchos grupos y un grupo puede tener además múltiples usuarios o Youtube donde un usuario puede estar suscripto a múltiples canales y estos tener múltiples suscriptores.



* Neo4j
* Dgraph



* Conexión directa de entidades
* Nodes and edges.
* Edges are the relationships
* Queries más simples comparadas con lo que haria falta en una db relacional.
* Son recomendadas para detectar patterns en los datos de una aplicación, relaciones entre records y recomendaciones.



### Search Database

Search Databases se encargan de buscar datos en cantidades enormes de información.



* ElasticSearch

* Solr

  

* Ofrecen full text search de manera eficiente y rápida.
* Son similares a las bases de datos basadas en documentos ya que los datos son objetos pero la diferencia es que analizan todo el texto del objeto de datos  y crean un índice de cada una de las palabras. De manera similar a un índice de un libro, cuando el usuario realiza una búsqueda solo otendremos que fijarnos el índice de los resultados relevantes en lugar de buscar en todos los documentos de la db.



## Conclusión

La elección de la db dependerá de la aplicación y también es posible utilizar también una combinación de distintas bases de datos por ejemplo una relacional para la mayoría de los datos y una de tipo Search Engine para manejar búsquedas rápidas o manejar cace con key value.