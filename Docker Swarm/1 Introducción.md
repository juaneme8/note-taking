# Docker Swarm

Docker Swarm es una herramienta para poder escalar las aplicaciones dockerizadas a un número muy grande de usuarios en un clúster de múltiples máquinas.



## Introducción

Suponemos el caso de un clúster de máquinas que a partir de una conexión a internet, llega a un balanceador de cargas encargado de recibir peticiones y distribuirlas en tres servidores. Esto podría resolverse con Docker y Docker Compose, pero podría ser tedioso cuando tengamos que realizar actualizaciones. En cada máquina tendremos el daemon de Docker corriendo los contenedores, tendremos que frenarlos, quitarlos del balanceador de cargas (para no matar una conexión existente) y haciendo que se rebalancee hacia otro lado. Luego volviendo a subir los contenedores y repitiendo lo mismo para todas las máquinas. Esto se conoce como *rolling deployment* esta tarea es tediosa y será necesario automatizarla para no hacerla a mano y que sea propensa a fallos. Docker Swarm ofrece una solución de clustering que propone que tener un cluster de muchas máquinas que a los ojos de los operadores de la infraestructura se vea como un único Docker daemon. Lo usamos como si estuviéramos usándolo en nuestra máquina local y tendremos la capacidad de actualizar servicios, rotar contenedores, etc. Con esto **lograremos aplicaciones altamente disponibles y muy fáciles de administrar**.



## Aplicaciones Productivas

Las aplicaciones productivas deben ser capaces de sobrevivir la carga

En este tipo de aplicaciones nos centramos en dos conceptos:

La **escalabilidad** que nos permitirá aumentar la potencia de cómputo para poder servir a mayor cantidad de peticiones o peticiones más pesadas sin producir una denegación de servicio. Es posible escalar verticalmente (esto se conoce como *ponerle mas fierro*, mas núcleos mas procesadores mas memoria RAM). Esta visión tiene dos desventajas, por un lado que existe un límite físico en cuanto a la potencia de la máquina y el riesgo de que si este servidor falla toda la aplicación se cae (aunque no sea sólo una podrían ser pocas).

La **escalabilidad horizontal** consiste en tener muchas máquinas una al lado del a otra, distribuyendo la carga. Este es el enfoque mas usado, ya que son computadoras mas pequeñas físicas o en la nube que suelen ser pequeñas. Los proveedores como AWS, Azure ofrecen soluciones de escalabilidad horizontal.

La **disponibilidad** es la capacidad de una aplicación servicio de estar las 24 horas del día disponible, aún ante un evento inesperado. Tener un uptime del 99.9% aun ante un evento catastrófico por ejemplo la rotura de un servidor. Es mucho más fácil tener alta disponibilidad con escalabilidad horizontal.

Swarm nos permite enfrentar estos problemas de manera sencilla pudiendo escalar infraestructura para correr contenedores



## Arquitectura de Swarm

Tenemos un esquema dos tipos de máquinas los **managers**(o administradores) y los **workers**.

![image-20220321185349868](C:\Users\JuaNeMe\Documents\Code\note-taking\Docker Swarm\imgs\1)

Los *manager nodes* son aquellos nodos (término para referirnos a la máquina, servidor o VM) encargados de administrar el clúster Swarm. Estas máquinas están conectadas en red entre sí y administra la comunicación entre ellas. Deciden donde se corren los nodos, cómo se comunican entre sí. También tiene a su cargo monitorear los nodos donde se corren contenedores de modo que si alguno se cae y pierde conectividad con el swarm puedan reposicionar esos contenedores en otros nodos que tengan disponibilidad.

Los *worker nodes* suelen ser mas que los *manager nodes* y en ellos se va a correr los contenedores productivos. Los manager nodes podrían también correr contenedores pero como tienen a cargo la sensible tarea tarea de coordinar el Swarm y no queremos que destinen sus recursos en otra misión.

Todos los nodos tienen que estar dentro de la misma red y tener el docker daemon, idealmente de la misma versión



## Requisitos para Swarm

Las aplicaciones deben cumplir ciertos requisitos para Docker Swarm, es necesario que siga los siguientes factores (twelvefactor.net)

* **Codebase**: el codigo debe estar en un repositorio (git por ejemplo relación 1 a 1 entre repositorio y aplicación)
* **Dependencies**: las dependencias deben estar declaradas (por ejemplo en `package.json`, deberian venir con la aplicación y no dar por sentado que estén instaladas en la máquina).
* **Configuration**: si la aplicación será ejecutada en distintos ambientes (desarrollo, testing, integración, qa o produción), deberiamos tener el mismo código y que la queremos que el comportamiento se modifique mediante una configuración. Puede ser mediante un archivo dentro de la imagen o mediante variables de entorno al correr el contenedor.  
* Backing services: los servicios en los cuales se apoya la aplicación (base de datos por ejemplo) deben ser tratados como cosas exterrnas a la aplicación y por configuración decimos como conectarse.
* **Build, Release and Run** estas tres fases deben estar bien separadas y no podemos hacer build cuando vamos a ejecutar el run.
* **Processes**: la aplicación debe ejecutarse como un proceso stateless no puede depender de un estado externo (archivos en la maquina o algo en memoria).
* **Port binding**: las aplicaciones deben exponerse a través de un puerto en la máquina sin un intermediario.
* **Concurrency**: la aplicación debe poder correr con multiples instancias en paralelo.
* **Disposability**: la aplicación tiene que estar diseñada para ser fácilmente detruible. Debe poder apagarse y levantar otra de manera mas simple.
* **Dev/Prod parity**: debemos lograr que el entorno de desarrollo sea lo mas pasible a producción.
* **Logs**: todos los logs de la aplicación deben tratarse como flujos de device (la aplicación debe escupir a stdout)
* **Admin processes**: todas las tareas administrativas deben poder ser ejecutadas como proceso independiente REPL teniendo acceso al estado del servidor sin entrar en un modo de administración.

Cumpliendo estas condiciones podremos aprovechar al máximo lo que ofrece Swarm.



Con Docker Desktop instalado podremos ejecutar `docker` y según la salida veremos si la instalación fue exitosa. Luego ejecutamos `docker --version` y obtenemos la versión. Con `docker info` podremos obtener información.