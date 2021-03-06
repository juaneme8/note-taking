# Docker Swarm

Docker Swarm es una herramienta para poder escalar las aplicaciones dockerizadas a un número muy grande de usuarios en un clúster de múltiples máquinas.



## Introducción

Suponemos el caso de un clúster de máquinas que a partir de una conexión a internet, llega a un balanceador de cargas encargado de recibir peticiones y distribuirlas en tres servidores. 

![image-20220321215509872](C:\Users\JuaNeMe\Documents\Code\note-taking\Docker Swarm\imgs\0.png)

Esto podría resolverse con Docker y Docker Compose, pero podría ser tedioso cuando tengamos que realizar actualizaciones. En cada máquina tendremos el daemon de Docker corriendo los contenedores (con Docker Compose o con un script), tendremos que frenarlos, quitarlos del balanceador de cargas (para no matar una conexión existente) y haciendo que se rebalancee hacia otro lado. Luego volviendo a subir los contenedores y repitiendo lo mismo para todas las máquinas. Esta solución de clustering se conoce como *rolling deployment* esta tarea es tediosa y será necesario automatizarla para no hacerla a mano y que sea propensa a fallos. Lo ideal sería tener muchas máquinas y administrarlas de manera más simple como cuando trabajamos con Docker Compose que podemos escalar de manera simple (ponemos scale 4 y tendremos 4 contenedores sirviendo la misma aplicación). Docker Swarm ofrece una solución de clustering que propone que tener un clúster de muchas máquinas que a los ojos de los operadores de la infraestructura se vea como un único Docker daemon por lo que logra administrarlas muy fácilmente.

![image-20220321220357929](C:\Users\JuaNeMe\Documents\Code\note-taking\Docker Swarm\imgs\1.png)

Lo usamos como si estuviéramos usándolo en nuestra máquina local y tendremos la capacidad de actualizar servicios, rotar contenedores, etc. Con esto **lograremos aplicaciones altamente disponibles y muy fáciles de administrar**.



## Aplicaciones Productivas

Las aplicaciones productivas deben ser capaces de sobrevivir la carga como así también a situaciones catastróficas, sirviendo siempre el contenido a los usuarios.

Para lograr esto nos centramos en dos conceptos:

La **escalabilidad** consiste en poder aumentar la potencia de cómputo de la infraestructura para poder servir a mayor cantidad de peticiones en la medida que la demanda avanza sin producir una denegación de servicio. La escalabilidad entonces se puede encarar de dos formas:

Es posible escalar verticalmente (esto se conoce como *ponerle mas fierro*, mas núcleos, mas procesadores mas memoria RAM). Gracias a esto podré correr más procesos, más contenedores. Si bien esta visión es simple ya que demanda dinero pero no debo configurar redes ni otras cosas, tiene dos desventajas. Por un lado que existe un límite físico en cuanto a la potencia de la máquina (o esta se vuelve muy cara o directamente no la conseguimos) y el riesgo de que si este servidor falla toda la aplicación se cae (aunque no sea sólo una podrían ser pocas). También es posible escalar horizontalmente, que consiste en tener muchas máquinas una al lado de la otra, distribuyendo la carga de procesos en cada una de ellas. Este es el enfoque mas usado, ya que son computadoras mas pequeñas (por ende más baratas y pueden ser físicas o en la nube) que suelen ser todas iguales y de requerimientos accesibles. Incluso los proveedores como AWS, Azure ofrecen soluciones de escalabilidad horizontal preconfiguradas.

La **disponibilidad** es la capacidad de una aplicación o servicio de estar las 24 horas del día disponible, aún ante un evento inesperado. Tener un uptime muy alto por encima del 99.9% aun ante un evento catastrófico por ejemplo la rotura de un servidor (por ejemplo ante la rotura de un disco o una simple pérdida de conexión).  Quizás con ese servidor roto no podré resistir tanta carga pero no será tan grave el problema ya que tendré otros servidores que podrán atender las peticiones.

Es mucho más fácil tener alta disponibilidad con escalabilidad horizontal.

Swarm nos permite enfrentar estos problemas de manera sencilla pudiendo escalar infraestructura para correr contenedores.



## Arquitectura de Swarm

Tenemos un esquema dos tipos de máquinas los **managers** (o administradores) y los **workers**.

![image-20220321185349868](C:\Users\JuaNeMe\Documents\Code\note-taking\Docker Swarm\imgs\2.png)

Los *manager nodes* son aquellos nodos (término para referirnos a la máquina, servidor o VM) encargados de administrar el clúster Swarm. Estas máquinas están conectadas en red entre sí y administra la comunicación entre ellas. Deciden donde se corren los contenedores, cómo se comunican entre sí. También tiene a su cargo monitorear los nodos donde se corren contenedores de modo que si alguno se cae o pierde conectividad con el swarm puedan reposicionar esos contenedores en otros nodos que tengan disponibilidad.

Los *worker nodes* suelen ser mas en cantidad que los *manager nodes* y en ellos se va a correr (muy probablemente) los contenedores productivos. En los manager nodes también podrían correrse contenedores pero como tienen a cargo la sensible tarea tarea de coordinar el estado del cluster, no queremos que destinen sus recursos en otra misión.

Todos los nodos tienen que estar dentro de la misma red y tener el Docker daemon, idealmente de la misma versión



## Requisitos para Swarm

Las aplicaciones deben cumplir ciertos requisitos para ser corridas en Docker Swarm, si bien hay distintas opiniones existe un consenso de que si cumple los siguientes requisitos estará lista:

> Podemos encontrar más información en [12factor.net](https://12factor.net/).

* **Codebase**: el código debe estar en un repositorio (git por ejemplo con una paridad 1 a 1 entre repositorio y aplicación)

* **Dependencies**: las dependencias deben estar declaradas explícitamente en algún archivo versionable (por ejemplo en Node tenemos `package.json`). Además deberían venir empaquetadas con la aplicación y no dar por sentado que estén instaladas en la máquina donde vamos a correr el proceso).

* **Configuration**: si la aplicación será ejecutada en distintos ambientes (desarrollo, testing, integración, QA, produción), deberíamos tener el mismo código y que el comportamiento se modifique mediante una configuración. Esto podría ser mediante un archivo dentro de la imagen o mediante variables de entorno al correr el contenedor.  

* **Backing services**: son los servicios en los cuales se apoya la aplicación (base de datos, cache, storage de archivos por ejemplo) deben ser tratados como servicios externos a los cuales la aplicación se enchufa. No deben formar parte del entregable y por configuración le diremos como conectarse.

* **Build, Release and Run** estas tres fases deben estar bien separadas y no podemos por ejemplo un build cuando vamos a ejecutar el run. 

  > Suponiendo una situación de NodeJs con TypeScript y tenemos que hacer el build, este no podríamos hacerlo en el servidor de producción sino que tendríamos que hacerlo durante el deploy y el entregable ya tiene que estar buildeado.

* **Processes**: la aplicación debe ejecutarse como un proceso stateless no puede depender el funcionamiento correcto de la aplicación de que exista un estado externo (archivos en la máquina o algo en memoria).

* **Port binding**: las aplicaciones deben exponerse a través de un puerto en la máquina sin un intermediario.

* **Concurrency**: la aplicación debe poder correr con multiples instancias en paralelo (notar que esto es lo que plantea la escalabilidad horizontal).

* **Disposability**: la aplicación tiene que estar diseñada para ser fácilmente destruible. Debe poder apagarse y levantar otra de manera muy simple y rápida.

* **Dev/Prod parity**: debemos lograr que el entorno de desarrollo sea lo mas parecido posible a producción.

* **Logs**: todos los logs de la aplicación deben tratarse como flujos de device (no debemos escribir a un archivo sino que la aplicación debe enviar a standard output)

* **Admin processes**: todas las tareas administrativas deben poder ser ejecutadas como proceso independiente de la aplicación (por ejemplo para debuggear algún proceso desde adentro del servidor con el estado en memoria del mismo).

  No es que iniciamos la aplicación en modo administración, sino que debo poder correr un proceso dentro de ella que me permita administrarla.

   Las tecnologias modernas tienen el REPL, cuando entramos a un servidor Node y escribimos `node` tenemos la consola pero tenemos acceso también al estado del servidor.

Cumpliendo estas condiciones podremos aprovechar al máximo lo que ofrece Swarm.



## Instalación Docker

Ver notas de Docker para la instalación de Docker Desktop. 

Una vez instalado podremos ejecutar `docker` y según la salida veremos si la instalación fue exitosa. Luego ejecutamos `docker --version` y obtenemos la versión. Con `docker info` podremos obtener información.



## Primeros pasos

Docker Swarm es un modo de ejecutar Docker, no es que tenemos que instalar algo aparte como sí sucede por ejemplo si queremos usar Docker Compose en Linux.



### Iniciar Modo Swarm

Para iniciar un swarm productivo:

```
docker swarm init
```

Nos aparecerá un mensaje como el siguiente:

```
Swarm initialized: current node (x9ubiu1m8doev9t7lrlk22j77) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-18rstdx2vwjn4xsxhhalv6aqxe3sgijswd46wsbpi28v2ff1o0-bnplqwnvf8k1a27z65rf18q45 192.168.65.3:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```



Como podemos ver nos indica los comandos a ejecutar para agregar un worker o un manager al swarm. De hecho si ingresamos `docker swarm join-token manager` veremos que el comando es el mismo pero cambia el token.



### Listar Nodos

Para listar los nodos:

```
docker node ls
```

> En una primera instancia el único nodo será nuestra computadora.



### Inspeccionar Nodo

Para inspeccionar el nodo:

```
docker node inspect oiz...
```

> Siendo `oiz...` el id del nodo.

Otra forma de hacerlo es:

```
docker node inspect self
```



Para imprimir esta información de manera más lejible podemos usar el flag `--pretty`

```
docker node inspect --pretty self
```

> En casi todos los comandos `inspect` en modo swarm contamos con la opción `--pretty` pero en algunos casos oculta información por lo que debemos usarla cuando sabemos que está allí lo que buscamos.

Obtendremos un JSON y en una parte veremos un campo `TLSInfo `con un certificado TLS con el cual el manager va a encriptar toda la comunicación entre los nodos. Esto es interesante ya que todo se transmitirá en la red encriptado por lo que en caso de correr swarms en la nube y tener que intercambiar secretos o claves privadas para configurar servicios, esto evitaría filtraciones.



### Salir Modo Swarm

Para salir de un swarm:

```
docker swarm leave
```

Veremos que nos tirará un error porque somos un manager (a diferencia de lo que sucedería si se va un worker donde los managers tomarían los contenedores que estaban corriendo en el y se lo van a poner a correr a otro nodo). Si se va un manager el swarm podría quedar en un estado irrecuperable.

Si estamos seguros de que queremos hacerlo:

```
docker swarm leave --force
```

Si ahora ejecutamos 

```
docker info
```

Veremos que dice `Swarm: inactive` y si iniciamos otro swarm pasará a `Swarm: active` y a mostrar mas infromación sobre el nodo.



### Creación Servicio

Si corremos contenedores con `docker run` o `docker compose up` no estaremos aprovechando las virtudes de Docker Swarm. El daemon en modo swarm es igual al normal pero tiene mas funcionalidades. El concepto de servicio es similar al visto al estudiar Docker Compose, siendo este encargado de agrupar a varios contenedores.

Con el Docker daemon corriendo en modo swarm vamos a crear un servicio:

```
docker service create --name pinger alpine ping www.google.com.ar
```

Vemos que pasará por una etapa de preparación donde descarga la imagen, ejecuta el contenedor y finalmente nos aparece un mensaje indicando que convergio.



>Simularemos una aplicación con cierta carga producto de realizar muchas consultas todo el tiempo.

> :information_source: La distribución **alpine** usada habitualmente en Docker como imagen base por su pequeño tamaño tiene la ventaja adicional de que está orientada en seguridad, lo cual hace que tengamos menos vulnerabilidades que usando un despliegue mas grande de Linux.



### Listar Servicios

Para ver los servicios corriendo

```
docker service ls
```



Existe una correspondencia entre servicios y contenedores. En este caso como estamos trabajando con un único nodo podremos ver ese contenedor corriendo (Docker swarm hizo el run por nosotros)

```
docker ps
```

Podremos ver que el contenedor tiene un nombre único generado automáticamente.



## 

### Estado Servicio

Para analizar el estado de un servicio en particular: 

```
docker service ps pinger
```

Veremos que el id devuelto no es ni el del servicio, ni el del contenedor sino el id de la tarea o task.

Las tasks surgen dado que los managers para correr servicios utilizan un enfoque llamado *schedulling* mediante el cual planifican en qué nodos van a ejecutarse los contenedores y para eso les asigna tareas.

 

Como este servicio tiene una sola réplica tiene una sola tarea en **DESIRED STATE** (Running) y **CURRENT STATE** (Running x minutes ago).



##  Ciclo de vida Servicio

![image-20220322093434709](C:\Users\juan.lauria\Documents\Code\note-taking\Docker Swarm\imgs\3.png)

> Notar que la imagen muestra que el cliente le habla al daemon.



### Inspeccionar Servicio

Para inspeccionar un servicio

```
docker service inspect pinger
```

> Recordar que podríamos usar `-pretty` aunque si bien es bonito de entender, no cuenta con toda la data completa.



Veremos en el JSON un campo `TaskTemplate.ContainerSpec.Image` que dice `alpine:latest@sha256:621...` lo que viene después del @ se conoce como digest, es un identificador de la imagen mas allá del tag ya que si pushean otro layer on ese tag, la que tengo yo tengo deja de ser oficialmente `latest`.



La existencia del campo `TaskTemplate` nos da la pauta que el manager node le envía al worker para que puedan construir los contenedores de esta tarea.



### Visualizar Logs Servicio

```
docker service logs -f pinger
```

> Utilizamos `-f` para hacer follow y con `CTRL+c` podremos salir.

Veremos en pantalla todos los pings que está realizando cada un segundo. Acá vemos la importancia de cumplir con los 12 factores ya que al estar escupiendo el log al stdout podemos visualizar lo que está sucediendo cosa que no podríamos si estuviéramos escribiendo un archivo.  



### Eliminar Servicio

```
docker service rm pinger
```

Sin embargo si ejecutamos `docker ps` veremos que aunque se eliminó el servicio, todavía seguimos teniendo el contenedor. El **DESIRED STATE** pasará a ser shut down pero tardará unos segundos en apagarlo.



Si queremos eliminar más de un servicio a la vez podemos hacerlo con:

```
docker service rm app db
```



## PlayWithDocker

Utilizaremos la herramienta [PlayWithDocker](https://labs.play-with-docker.com/) donde podremos loguearnos con nuestra cuenta de Docker, que es un playground gratuito para poder utilizar Docker y Docker Swarm.

Es un espacio de trabajo que tendremos por 4 horas.

con **ADD NEW INSTANCE** tendré una máquina a mi disposición con un cierto hardware, con una determinada IP y con Ubuntu, Git y Docker instalado. 

* Copiando la dirección que aparece en el navegador tendremos acceso a la misma máquina y podremos trabajar de manera colaborativa con varias personas.
* Con `ALT+ENTER` pasamos a una terminal de pantalla completa.
* Haciendo click en **EDITOR** veremos el filesystem. Incluso será posible crear un archivo vacío, `touch hola.txt`, abrir el editor y agregar contenido de manera visual. 
* Es posible conectarnos a este nodo por SSH, desde nuestra terminal podremos pegar el comando que nos ofrece y nos conectaremos automáticamente.
* Es posible copiar con `CTRL+Insert` y pegar con `SHIFT+Insert`.
* Es posible hacer click en la llave fija y elegir un template por ejemplo **3 managers y 2 workers**.
* Cuando hacemos un servicio que publica un puerto nos entrega un endpoint público (url pública) para visitarlo.



## Arquitectura Multi Nodo

Si en el playground ejecutamos

```
docker swarm init
```



Obtendremos un error: **Error response from daemon: could not choose an IP address to advertise since this system has multiple addresses on different interfaces (192.168.0.18 on eth0 and 172.18.0.75 on eth1) - specify one with --advertise-addr**

 Nos está indicando que tenemos múltiples interfaces de red (lo podemos comprobar con `ifconfig`) , cosa que no nos sucede en nuestra máquina pues tenemos una única.

Usaremos la IP que nos aparece en la parte de arriba de la página, por ejemplo 192.168.0.33

```
docker swarm init --advertise-addr 192.168.0.33
```

> Podemos escribir --adv y presionar TAB para que se autocomplete.

> Veremos que nos aparece el icono de una persona al lado del nodo en el panel lateral, lo cual nos da la pauta de que se trata de un manager.

A continuación creamos dos instancias que lógicamente tienen el docker daemon pero no tienen el modo swarm activado (cosa que podremos verificar con `docker info`)

Ejecutamos ahora el comando `docker swarm join` con el token recibido al crear el manager en ambas instancias.

Si ahora en el manager (no en los worker nodes que no podrán manejar aspectos relacionados con la administración del swarm) queremos listar los nodos:

```
docker node ls
```

* Veremos uno de los nodos con **MANAGER STATUS** de Leader y los otros no dicen nada lo cual nos indica que son workers.
* Veremos un asterísco indicándonos en qué nodo nos encontramos actualmente.



### Creación de Servicio

```
docker service create --name pinger ping www.google.com
```



Con `docker service ls` veremos el servicio y para saber donde están asignadas las tareas de este servicio (en este caso será solo una tarea) `docker service ps pinger`. Por defecto las tareas son asignadas a cualquier nodo del swarm y vemos que la asignó al nodo1 aunque es el manager (por ende con `docker ps` lo veremos corriendo allí). Podemos aplicar restricciones si queremos que no corran en algun nodo en particular. 



## Escalar Horizontalmente

Para escalar nuestro servicio para que tenga varias replicas.

Suponemos el caso que queremos tener 5 réplicas:

```
docker service scale pinger=5
```

> Si no queremos ver la salida interactiva y queremos retomar el control de manera inmediata podemos ejecutarlo con la opción `-d`.
>
> ```
> docker service scale -d pinger=5
> ```

```
docker service ps pinger
```

Podremos ver que el servicio tiene 5 tareas que no estarán corriendo el mismo nodo. De hecho si vemos que en `node1` hay dos tareas y desde esa terminal ejecutamos `docker ps` veremos esos dos contenedores.


Si ejecutamos ahora `docker service logs -f pinger` veremos el output de todas las tareas (aún con contenedores de distintos nodos) en tiempo real sin perder nada de información.



En este momento tenemos la ventaja de que si se cae un nodo (salvo que sea el manager) no dejaremos de responder ante peticiones.



## Actualización de Versión

Suponiendo que tenemos una versión nueva del código, lo normal sería generar una nueva imagen y luego actualizar el servicio con ella. En cambio simularemos esos cambios actualizando la plantilla del servicio. Recordemos que un servicio genera un template para las tareas y es eso lo que le da a los nodos para que las ejecuten.

```
docker service inspect pinger
```

> Podremos ver por ejemplo `Mode.Replicated.Replicas`igual a 5.

Nos encontramos con el campo `Spec.TaskTemplate.ContainerSpec.Args` que es igual a `["ping","www.google.com"] y queremos cambiar estos argumentos de modo que le haga ping a otra cosa.



Para hacer una actualización "en caliente" del servicio y cambiarle su configuración (podría ser la imagen lo que le cambiamos)

```
docker service update --args "ping www.amazon.com" pinger
```

> Si no queremos ver el output interactivo y obtener el control inmediatamente podemos utilizar la opción `-d`.
>
> ```
> docker service update -d --args "ping www.amazon.com" pinger
> ```



A partir de este momento se actualizarán de a una todas las tareas, si lo hiciera de una borrando todos los contenedores y creando nuevos tendríamos un momento de *downtime*. Es decir que en caso de que se trate de una aplicación expuesta no podríamos servir a las peticiones de los usuarios. 

  

## Rollback de Versión

Si luego de efectuar una actualización ejecutamos `docker service inspect pinger` veremos que apareció un nuevo campo llamado `PreviousSpec`. Docker swarm cuando hace una actualización de un servicio se guarda el contenido de la configuración anterior de modo que si hay algún problema podamos hacer un *rollback* rápido (no tendremos que preocuparnos por recordar las variables de entorno, imagen usada, ni ponernos a hacer inspect) y volver a la versión anterior.

```
docker service rollback pinger
```

Este rollback lo va efectuando también de a una tarea a la vez.



Ejecutando `docker service ps pinger` veremos las tareas tanto        con sus versiones actuales como sus versiones pasadas. Por ejemplo si tenemos una versión inicial, una cuando actualizamos y una tercera al hacer un rollback, es posible que actualmente la corra un nodo distinto al o los que la corrieron anteriormente. Swarm se encarga de colocar las tareas en nodos que las puedan correr.

**También es posible inspeccionar tareas viejas para saber por qué fallaron.**

Es posible configurar la cantidad de versiones que queremos guardar.

 

## Escenario Realista

Hasta ahora trabajamos con pinger  (servicio que a partir de una imagen de alpine node realiza un ping) para simular una aplicación. Esto lo hicimos para no escribir código, pero más allá de eso lo importante es que buscamos asegurar que funcione todo el tiempo.

Consideramos ahora un caso de alta demanda ejecutando una aplicación crítica que no puede caerse. Esta tiene actualizaciones permanentes con un CI/CD y una situación en la que se produce una falla y no podemos conectarnos remotamente para hacer el rollback.

Para simular un caso de alta demanda escalamos aún mas el servicio (que hasta ahora tenía 5 réplicas).

```
docker service scale pinger=20
```

Otra forma de hacer esto mismo es con:

```
docker service update --replicas=20 pinger
```

Si no queremos ver el output interactivo y obtener el control inmediatamente podemos utilizar la opción `-d`.

```
docker service update -d --replicas=20
```



## Modificar comportamiento default

Queremos sacar una nueva versión nuevamente actualizando los argumentos de la tarea.

```
docker service update --args="ping amazon.com.ar" pinger
```

Cuando tenemos muchas tareas el comportamiento por defecto de actualizar una a una las tareas puede que se vuelva muy lento en situaciones donde tenemos muchas.

Si hacemos `docker service inspect pinger` veremos el campo `"UpdateConfig"` hace referencia a cómo se comportará este servicio cuando tiene que hacer un update.

```json
"UpdateConfig": {
    "Parallelism": 1,
    "FailureAction": "pause",
    "Monitor": 5000000000,
    "MaxFailureRatio": 0,
    "Order": "stop-first"
},
```



* `"Pararellism"` hace referencia a de a cuantas tareas queremos que vaya haciendo el cambio (por defecto es 1)
* `"Order"`: Por defecto es `"stop-first"` hace referencia a que primero apagará las tareas que quiere actualizar y una vez apagadas creará las nuevas. La alternativa es `"start-first"` que creará más tareas y cuando estas estén listas borrará las viejas. Esto en infraestructura de la nube se suele llamar *overprovisioning* y consiste en hacer mas infraestructura de la que necesito para asegurarme que no me quedo sin carga y cuando están los cambios borro lo viejo.



Para cambiar estos dos parámetros `Pararellism` y `Order`:

```
docker service update --update-pararellism 4 --update-order start-first pinger
```

Luego con `docker service inspect pinger` podremos verificar que se cambiaron estos valores.

Luego para actualizar el servicio ya con estos cambios:

```
docker service update --args "ping www.facebook.com" pinger
```



Siguiendo con las otras opciones:

* `FailureAction` es un parámetro que nos permite configurar qué sucede si estamos actualizando y falla. Por defecto es `pause`, pero también podemos continuar o hacer rollback automáticamente.
* `Monitor` es la ventana de tiempo en nano segundos que queremos darle a docker swarm para monitorear a una tarea nueva para determinar si falló o no. El valor por defecto es de 5 segundos pero dependenderá un poco del tiempo de startup de nuestra aplicación.
* `MaxFailureRatio` hace referencia a la proporción de tareas que deben fallar para que se considere que falló.



También tenemos estos mismos parámetros pero asociados a la configuración del rolllback:

```
"RollbackConfig": {
    "Parallelism": 1,
    "FailureAction": "pause",
    "Monitor":  5000000000,
    "MaxFailureRatio": 0,
    "Order": "stop-first"
},
```

La única diferencia es que `FailureAction` acepta `pause` y `continue` (pero lógicamente `rollback` no)

* Queremos que si falla el 10% de las actualizaciones que haga un rollback:

```
docker service update --update-failure-action rollback --update-max-failure-ratio 0.5
```

Notar que estos cambios en las configuraciones son inmediatas, pues debe cambiar solamente el template.

* También queremos que en caso de hacer rollback lo haga de a todas las tareas a la vez (0 indica todas a la vez)

```
docker service update --rollback-pararellism 0 pinger 
```

Luego probamos que esto funcionó actualizando el servicio y haciéndolo fallar:

```
docker service update --args "ping asdasd;asdas" pinger
```

Veremos que realiza el rollback porque falló más del 50% de las tareas, como teníamos `start-first` en la configuración de actualización esto hace que sea mucho más rápido (creo las tareas nuevas pero como estas fallan, las borra y continúa con las viejas).



# Servicios Expuestos al Exterior

Trabajaremos ahora con servicios expuestos al exterior y para ello en el [repositorio](https://github.com/platzi/swarm) del Curso de Platzi nos encontramos con tres proyectos.

El primero es **hostname** que recibe un `GET` a la raíz y responde con el nombre del host del servidor donde está corriendo.

```javascript
const express = require('express')
const os = require('os');
const app = express();
const port = 3000;

const hostname = os.hostname();

app.get('/', (req, res) => {
  res.send(`Saludos desde ${hostname}!`);
});

app.listen(port, () => console.log(`Server listening on port ${port}!`))
```

 

La imagen debe estar disponible en un repositorio (DockerHub pro ejemplo) ya que no basta con construirla en un nodo sino que debe ser accesible por todos ellos.

Desde el directorio del repositorio clonado de GitHub:

```
docker build -t juaneme8/swarm-hostname .
```

Luego la publicamos:

```
docker push juaneme8/swarm-hostname
```



Luego en **PlayWithDocker** en lugar de construir a mano el Swarm, utilizaremos un template haciendo click en el botón de la llave fija. Elegimos la opción **3 Managers and 2 Workers**.

A continuación desde un manager con `docker node ls` podremos ver los nodos.

A continuación vamos a crear el servicio con 3 réplicas (en lugar de escalarlo luego lo creamos con un número de réplicas predefinido).

```
docker service create -d --name app --publish 3000:3000 --replicas=3 juaneme8/swarm-hostname
```

> `-d` nos permite crear el servicio de manera *dettached*
>
> `--publish` podemos usar `-p` como en docker run. 



Con `docker service ps app` poremos ver el estado en **Preparing** cuando estén descargando las imágenes y esperamos hasta que pase a **Running**.

Acabamos de crear un servicio que publica un puerto, por ese motivo en PWD nos aparecerá en la parte superior un link que hace referencia al puerto publicado (3000 en nuestro caso). Haciendo click en ese botón accederemos con el navegador mediante una URL pública, al servidor. 

Luego desde la terminal hacemos curl requests a esta url y veremos que nos responde cada una de las réplicas.  

```
curl http://ip172-18-0-42-c95kqog9jotg0089379g-3000.direct.labs.play-with-docker.com/
```

Swarm se encarga de que nuestra petición llegue a cada uno de los contenedores.

Docker por defecto pone como hostname el id del contenedor de las tareas (:warning:  CHEQUEAR :warning: ).

En cuanto a los puertos cuando trabajamos con docker run -p hacemos un binding del puerto del contenedor y el del host. 

 

# Routing Mesh

En el caso que analizamos anteriormente tenemos 5 nodos o máquinas y tres contenedores. Tenemos la certeza de que las requests llegarán a un nodo con contenedores atendiendo en ese puerto gracias al *routing mesh*.

Cuando un nodo del Docker Swarm recibe una petición para un puerto determinado

Cuando recibo una petición en un nodo **para un servicio en un puerto en particular**, si en ese nodo tengo algo escuchando en ese puerto lo proceso. En caso contrario a esa petición la captura el Routing Mesh. Si tengo un servicio en otro nodo escuchando en ese puerto y si existe deriva la petición allí. De esta manera no se pierden peticiones, cuando tenemos más nodos que contenedores.

Es posible tener más de un contenedor en un mismo nodo y esto se pone de manifiesto si ejecutamos:

```
docker scale app=6
```

Como tenemos 5 nodos, estamos segudos de que tendremos más de un contenedor en un nodo. Esto contrasta con lo que ocurre con Docker Run obtendríamos un error si queremos usar el mismo puerto para dos contenedores.

Con `docker service ps app` podremos saber en qué nodo tenemos más de un contenedor y luego desde ese nodo podremos ejecutar `docker ps` y veremos que ambos aparecen escuchando al mismo puerto con `3000/tcp`.

Con Swarm los puertos de los servicios no se bindean directamente a los puertos del nodo sino que utilizan una red especial llamada *ingress*. Es por eso que podemos tener muchos más servicios escuchando en puertos públicos que la cantidad de nodos que tenemos. En caso de demanda podremos escalar los servicios y aprovechar de este modo una mayor potencia (sin que sea un impedimento pisarnos con un puerto).

```
docker network ls
```

> Si queremos crear otro servicio publicado en el mismo puerto sí obtendremos un error, pero sí podremos tener muchas tareas del mismo servicio gracias al routing mesh.



# Establecer Constraints

Es importante que las tareas corran en nodos workers y no en nodos managers ya que estos tienen a su cargo tareas administrativas fundamentales para el funcionamiento del Swarm (y no queremos que por generar una carga adicional de RAM terminemos ocasionando eventos catastróficos que compromentan al Swarm) y para lograr esto es posible debemos configurar dónde queremos que corra cada tarea.

Para visualizar las tareas de manera más sencilla a cómo lo venimos haciendo hasta ahora:

```
docker service create 
-d 
--name viz 
-p 8080:8080 
--constraint=node.role=manager
--mount type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock
dockersamples/visualizer
```



* Recordemos que los nodos managers son aquellos con acceso a la información del listado de nodos, servicios, etc., por ende esto debemos correrlo en un manager. Es por esto que indicamos que sus tareas están restringidas a los managers `--constraint=node.role==manager`.

* Además debe poder comunicarse con el Docker Daemon cosa que hacemos montando el socket (por donde se puede comunicar uno con él) del docker daemon: `--mount type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock`  

  Con esto le decimos que las tareas del servicio deben montarles a sus contenedores el socket de Docker mediante el cual se comunicarán con el daemon.



Bajará la imagen y veremos que pasar de **Preparing** a **Running**.

```
docker service ps viz
```



En PWD veremos ahora un botón con el puerto 8080 y si ingresamos ahí veremos la UI del Visualizer que nos muestra cada uno de los nodos y los contenedores que corren en ellos.



Debemos agregar una restricción a un servicio existente para que la aplicación corra sólo en nodos workers:

```
docker service update --constrant-add node.role==worker --update-parallellism=0 app
```

Queremos que la actualización no la haga uno por uno (el valor por default es 1) sino que lo haga todas las tareas a la vez. Si ahora vamos a la interfaz gráfica veremos que como detecta que hay tareas que no cumplen con el requisito solicitado, los está replanificando (rescheduling) para que estén las tareas sólo en los nodos workers. De esta manera estamos redistribuyendo la carga de los managers a los workers.



# Vaciar un Worker

Una vez que definimos dónde queremos que se ejecuten las tareas. Ahora es posible que una falla que veamos con algún sensor de sistema de monitoreo (problemas de conectividad, disco, etc) o porque así lo deseamos, queramos vaciar las tareas que se están ejecutando en un nodo. En ese caso vamos a querer quitar las tareas de ese nodo hasta solucionar esta situación.

Desde el manager del Swarm lo primero que hacemos es litar los nodos.

```
docker node ls
```

A continuación suponemos que queremos vaciar el llamado worker2:

```
docker node inspect --pretty worker2
```



Vemos el campo `Availavility` que si bien su información la vemos al ejecutar `docker node ls`, nos servirá para conocer el nombre del campo que queremos actualizar.

```
docker node update --availability drain worker2
```

Con `drain` indicamos que queremos drenar el nodo es decir sacar las cosas que hay en él.

Podemos verificar el nuevo estado con `docker node ls`.



En nuestro caso de 3 managers y 2 workers, las 6 réplicas serán ejecutadas por el `worker1` y veremos que su carga de memoria habrá aumentado. Durante el tiempo en que se rotaron esas tareas tuvimos menos potencia (throughput) pero no tuvimos downtime.

Una vez realizadas las tareas de mantenimiento podremos volver a utilizar el worker2.

```
docker node update --availability active worker2
```

Lo chequeamos nuevamente con `docker node ls`



Sin embargo, las tareas continúan ejecutándose en el `worker1`. Esto es así debido a que el planificador de tareas, si no tiene ningún motivo para redistribuir la carga no lo hará. Por motivo nos referimos a un service update o si tiene tareas que no pudo meter en ningún otro nodo, etc.

Para volver a un entorno distribuido con tareas en `worker2` debemos forzar una actualización, cambiando el valor de una variable de entorno aunque esta no sirva para nada.

```
docker service update -d --env-add UNA_VARIABLE=de-entorno --update-pararellism=0 app
```

> El `--update-pararellism=0` lo hacemos para ganar velocidad pero en un servicio productivo no deberíamos hacerlo.



# Comunicación entre Servicios

Para trabajar con servicios que se comunican entre sí en el mismo Swarm, debemos entender cómo funciona el networking de Swarm, es decir las redes e interconexión entre nodos.



## Listar Redes

```
docker network ls
```

La network de docker standalone viene con las redes `bridge`, `host` y `none`. Cuando iniciemos un Swarm en el docker daemon nos aparecerán dos redes más `docker_gwbridge`(como driver es `bridge` y tiene scope `local`) e `ingress` (que es una red especial que en driver aparece como overlay y en scope swarm).

El hecho de en la red `ingress` el driver sea `overlay` nos da la pauta que está disponible a nivel cluster, es decir está presente en todas las máquinas y es en todas la misma. Son las que debemos usar cuando queremos interconectar servicios de swarm entre sí.

La red `docker_gwbridge` es local y habrá una en cada uno de los nodos. Esta será la encargada de conectar la red ingress overlay con la red física de cada uno de los nodos del swarm.



## Creación Red

Creación de red nueva que pueda ser utilizada por cualquier servicio o tarea desde cualquier nodo.

```
docker network create --driver overlay app-net
```

Para verificar la creación `docker network ls`.

La `ingress` no debemos tocarla normalmente.



## Inspeccionar Red

```
docker network inspect app-net
```

En la medida que tengamos servicios conectados a esta red obtendremos más información.



## ej. Networking

En el repositorio del curso, utilizamos el ejemplo **networking** que consiste en una aplicación express que cuenta con un endpoint que  que se conecta a una DB Mongo y escribe un mensaje.

```
docker build -t juaneme8/swarm-networking .
```

```
docker push juaneme8/swarm-networking
```



Luego en el swarm de PWD debemos crear el servicio de Mongo y conectarlo a la red.

```
docker service create -d --name db mongodb --network app-net
```

> Vamos a tener que pasarle como variable de entorno la URL de la base de datos.

Podemos verificar el estado con `docker service ps db`



Luego creamos el servicio de la aplicación basada en la imagen swarm-networking:

```
docker service create -d --name app --networking app-net -p 3000:3000 juaneme8/swarm-networking 
```

Con `docker service ls` chequeamos tener ambas cosas corriendo.

Si ingresamos a la URL provista por PWD veremos que nos tira error debido a que está intentando trabajar con localhost:27017 y debemos pasarle en cambio la variable de entorno `MONGO_URL` cosa que hacemos con:

```
docker service update --env-add MONGO_URL=mongodb://db/test
```

Si dos servicios están conectados a la misma red van a poder encontrarse por hostname es por eso que utilizo `db`. Esto es similar a lo que ocurre en Docker compose o cuando conectamos dos servicios a la misma red en Docker local, la diferencia es que aca sucede *across host* por tratarse de una red *overlay*.

A partir de este momento nos funcionará ingresando a la URL provista por PWD.

Si hacemos `docker network inspect app-net` desde un nodo que tiene servicios conectados a la red veremos en `Containers` los contenedores conectados a esa red y en `Peers` las ip de los nodos que participan en la red overlay.



### Lectura de datos DB

Podemos fijarnos en qué nodo están las tareas del servicio db y conectarnos a ese contenedor a fin de visualizar los elementos que fueron agregados a la base de datos.

```
docker exec -it c66... bash
```

```
mongo
```

```
use test
```

```
db.pings.find()
```



### Comentario sobre Swarm y DB 

Usando el networking interno de Swarm hemos logrado que dos servicios en nodos distintos puedan comunicarse. **Debemos tener presente que no es aconsejable correr una base de datos en el Swarm.** Si bien esto en una aplicación productiva no debería hacerse pero sí sería útil por ejemplo si queremos levantar un Swarm en un entorno de testing por ejemplo de un pull-request de nuestro proyecto. En ese caso levantamos una pequeña base de datos y cuando terminamos borramos todo.



## Eliminar Red

```
docker network rm app-net
```



# Stack

Hasta ahora vimos cómo crear servicios, dónde queremos que corran, dónde no queremos que corran, como publicar servicios al exterior, cómo interconectar servicios dentro del Swarm,etc. Sin embargo cuando queremos hacer algo complejo tendremos que ejecutar una serie de tareas en orden y esto puede volverse tedioso.

Para solucionar este problema contamos con los stacks que son una herramienta que nos permite controlar como se despliegan los servicios mediante un archivo. 



En la [carpeta stacks](https://github.com/platzi/swarm/tree/master/stacks) del repositorio del curso nos encontramos con un archivo `stackfile.yml` con el siguiente contenido. 

```yaml
version: "3"

services:
  app:
    image: gvilarino/swarm-networking
    environment:
      MONGO_URL: "mongodb://db:27017/test"
    depends_on:
      - db
    ports:
      - "3000:3000"

  db:
    image: mongo
```

Como podemos apreciar es idéntico a un compose file pero logrando correr multinodo.



Copiamos el contenido de ese archivo y lo pegamos en un archivo:

```
vim stackfile.yml 
```



Para ejecutar el contenido de este archivo y crear todos los servicios juntos utilizamos:

```
docker stack deploy --compose-file stackfile.yml app
```

Nos creará la network `app_default` y también los servicios `app_db` y `app_app`



## Ventajas

La importancia trabajar con un stackfile y no una serie de comandos es que podremos versionarlo en el repositorio y también como describirá la arquitectura de la aplicación servirá a la gente que se una al equipo en aquellos casos en que la documentación no sea suficientemente buena.



## Listar Stacks

```
docker stack ls
```

Veremos el stack y nos dirá que tiene 2 servicios. Para listar los servicios podemos ejecutar `docker service ls`.



## Listar Tareas de un Stack

```
docker stack ps app
```

## Listar Servicios de un Stack

```
docker stack services app
```

Acá si tuviéramos n réplicas en uno de los servicios aca veríamos el cartel `n/n` mientras que en el listado anterior veríamos las n tareas por separado.



## Constraints

En caso de estar en un esquema productivo, queremos que todas las tareas corran en los workers para ello podríamos agregar el constraint al servicio que no lo esté haciendo como vimos anteriormente o editar el `stackfile.yml`. 

```
version: "3"

services:
  app:
    image: gvilarino/swarm-networking
    environment:
      MONGO_URL: "mongodb://db:27017/test"
    depends_on:
      - db
    ports:
      - "3000:3000"
    deploy:
      placement:
        constraints: [node.role==worker]
      
  db:
    image: mongo
```

> Podríamos utilizar este mismo archivo para Docker Compose pero ignorará el contenido del atributo deploy.
>
> Esto mismo podríamos hacerlo para los dos servicios pero lo hacemos para el sevicio que vimos que sus tareas se desplegaban en un manager.



Para actualizar de acuerdo a los cambios que acabamos de realizar ejecutamos el mismo comando:

```
docker stacks deploy --compose-file stackfile.yml app
```

Como ya tiene un stack con ese nombre se fija las diferencias y genera los cambios .



## Eliminar Stack

```
docker stack rm app
```

Con este comando eliminaremos los servicios y la red overlay necesaria para la interconexión entre nodos.

 

# Estructura Productiva

Estudiaremos un escenario productivo en el cual queremos múltiples aplicaciones o micro-servicios en el mismo Swarm pero accesibles mediante distintos nombres de dominio (`api.domain.com`, `domain.com/web`, `backoffice.domain.com`). Hasta ahora cuando expusimos una app con el puerto 3000 por ejemplo, utilizamos la url pública provista por PWD. En un entorno productivo en cambio tendremos que realizar ciertas configuraciones.



## Traefik

Vamos a trabajar con un **proxy inverso** que recibe las peticiones y según el nombre de sub-dominio las redirecciona al servicio encargado de atenderla.

Utilizaremos para tal fin la herramienta llamada Trafik. Es un intermediario entre las peticiones que vienen de internet a nuestra infraestructura.

Vamos a querer conectar un montón de servicios a Traefik para que pueda rutear la información. Estos servicios no necesariamente van a estar conectados entre sí, pero sí Traefik debe comunicarse con ellos. Para esto debemos crear una red overlay.

En PWD empezamos creando esta red:

```
docker network create --driver overlay proxy-net
```

Creamos el servicio de Traefik:

```
docker service create 
--name proxy
--constraint=node.role==manager
-p 80:80
-p 9090:8080
--mount type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock
--network proxy-net
traefik
--docker --docker.swarmMode --docker.domain=guido.com --docker.watch
```

Va a detectar que servicios aparecen en el Swarm cuando los creemos y los va a incorporar a su lista de reverse proxy en función de algunas etiquetas que les pongamos al crearlos. Para esto tiene que escuchar los eventos del Swarm y para ello debe tener acceso al socket del Docker daemon del un manager (para eso debe correr en un manager)

En el puerto 80 publicamos el proxy http y en el 9090 (no usamos el 8080 por si allí tenemos el visualizer) la UI de administración

Lo que agregamos después del nombre son parámetros de ejecución para las tareas de ese servicio:

* `--docker --docker.swarmMode` para indicarle que debe trabajar con un orquestador Swarm.
* `--docker.domain=guido.com` para indicarle el dominio con el cual queremos trabajar.

* `--docker.watch` es para descubrir los nuevos servicios qeu van apareciendo en el Swarm.

* `--api` es para que nos muestre la UI de administración.

Luego de ejecutar este comando con PWD podremos entrar al 9090 donde veremos la UI de Traefik. Si entramos al 80 nos tirará un 404.



Vamos a crear un servicio que queremos exponer a través de Traefik.

```
docker service create
--name app1
--network proxy-net
--label traefik.port=3000
gvilarino/swarm-hostname
```

Con la etiqueta `--label traefik.port=3000` le estamos poniendo metadata que le servirá a Traefik para saber a qué puerto debe mandar las conexiones entrantes.

Traefik recibe en el puerto 80 y se lo debe mandar al 3000 a esta aplicación.

Para verificar que esto funcion:

```
curl -H "Host: app1.guido.com" http://localhost
```

Hacemos un curl con host header al dominio que queremos y le pegamos a Traefik con http:localhost. A partir de ese momento obtendremos la respuesta del contenedor.

En `app1.guido.com` decimos que `guido.com` es el *top level domain* y `app1` es el nombre del servicio.



Creamos otro servicio (si bien esto mismo podríamos hacerlo escalando, imaginemos que se trata de aplicaciones distintas)

```
docker service create
--name app2
--network proxy-net
--label traefik.port=3000
gvilarino/swarm-hostname
```

> Notar que ambos servicios están escuchando en el puerto 3000 cosa que es posible mediante (obviamente si ahora le pegamos al swarm localhost:3000 no va funcionar, pero pegándole al 80 con el dominio correcto traefik va a saber a donde mandarlo). Gracias a esto no tendremos que cambiar los puertos en los que escucha cada una de las aplicaciones. Bastará con exponerlos detrás de un reverse proxy.



```
curl -H "Host: app2.guido.com" http://localhost
```



Si queremos cambiar la imagen:

```
docker service update --image gvilarino/swarm-networking app2
```



En el Dashboard de Traefik veremos los frontends (definición de las rutas de entrada) y los backends (los servidores)



# Swarms Productivos

El número de managers debe ser impar. Esto lo podemos comprobar con los templates de PWD (que nos ofrece 3 managers and 2 workers, 5 managers and no workers, etc)

```
docker node ls
```

Veremos en la columna **manager status** que solo uno de ellos dice **Leader** esto significa que uno de ellos será el encargado de tomar las decisiones y el resto acompañará. Esto cambiará después de cierto tiempo en el cual los managers votarán al próximo líder. Para evitar la posibilidad de empate es que deben ser impares. Esto se conoce como **algoritmo raft**.

Como conclusión podemos decir que en un entorno productivo los managers no deben ser sometidos a mucha carga de procesamiento porque deben ocuparse del Swarm y como mínimo deben ser tres (si tenemos sólo uno y se nos cae sería crítico). En tanto los workers en ocasiones vamos a querer que tener distintos grupos de workers con características específicas por ejemplo distintas características de hardware como unos con mucha RAM otros con mucha potencia de CPU según la necesidad. 



## Administración de Swarm Productivo

En lugar de tener que conectarnos siempre por SSH para adminisitrar el Swarm productivo podemos utilizar una herramienta que ofrece una interfaz visual para administrar un Swarm. Esta herramienta se llama **Portainer** y funciona tanto para el modo standalone como para el modo Swarm.

> Estas herramientas deben contar con un almacenamiento ya que deben persistir cosas en disco (estado interno, stacks, etc) mediante volúmenes, pero por defecto Docker Swarm viene sólamente con el Docker local ese decir que ese volumen será local y estará sólo en un nodo. Existen plugins de storage de Swarm que nos permiten que ese volumen en lugar de escribirse en el disco del nodo se escriba en un disco en AWS, Azure o donde queramos.



Creamos un volumen

```
docker volume create portainer_data
```

Podemos verificar que el DRIVER el local con:

```
docker volume ls
```



Creamos un servicio:

```
docker service create 
--name protainer
-p 9000:9000
--constraint node.role==manager
--mount type=bind,src=/var/run/docker.sock, dst=/var/run/docker.sock
--mount type=volume,src=portainer-data,dst=/data
portainer/portainer
-H unix:///var/run/docker.sock
```



> Como contraint utilizamos `--constraint node.role==manager` ya que tendremos que manejar datos de Swarm, desplegar servicios, etc.
>
> Como parámetro debemos pasarle `-H unix:///var/run/docker.sock` (notar el uso de triple barra).



EN PWD tendremos un acceso al puerto 9000 mediante un link público para acceder a la interfaz de Portainer. Lo primero que debemos hacer es crear los datos de login de un usuario administrador.



# Ejemplo Completo Swarm

Podremos ver la potencia de Docker Swarm con [example-voting-app](https://github.com/dockersamples/example-voting-app) que trabaja con distintos servicios una aplicación web en Python llamada `voting-app` que recibe los votos, se almacenan los votos en Redis, luego de cierto tiempo un worker en .NET almacena los votos en una DB en PostgreSQL y con Node.js una aplicación web `result-app` que muestra todos los votos.

En el repositorio nos encontramos con varios archivos stackfiles y usaremos el llamado `docker-stack.yml`, presionamos RAW y copiamos todo el contenido.

```
version: "3"
services:

  redis:
    image: redis:alpine
    networks:
      - frontend
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure
  db:
    image: postgres:9.4
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    deploy:
      placement:
        constraints: [node.role == manager]
   :
    image: dockersamples/examplevotingapp_vote:before
    ports:
      - 5000:80
    networks:
      - frontend
    depends_on:
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure
  result:
    image: dockersamples/examplevotingapp_result:before
    ports:
      - 5001:80
    networks:
      - backend
    depends_on:
      - db
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  worker:
    image: dockersamples/examplevotingapp_worker
    networks:
      - frontend
      - backend
    depends_on:
      - db
      - redis
    deploy:
      mode: replicated
      replicas: 1
      labels: [APP=VOTING]
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints: [node.role == manager]

  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    stop_grace_period: 1m30s
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]

networks:
  frontend:
  backend:

volumes:
  db-data:
```



Luego en Portainer podemos ir a **Stacks** y luego **Add stack**, le asignamos el nombre voting y podremos pegarlo en el editor web, subir un archivo o indicar la ruta del repositorio. Por último hacemos click en **Deploy the stack**.

Tanto desde la interfaz web ingresando al stack como desde la terminal de PWD podremos ejecutar `docker stack ls` o `docker stack ps voting` y visualizaremos los servicios y tareas de cada uno.



# Problemas Swarm

Para evitar tener problemas y eventualmente downtime debemos tener ciertas precauciones.

## Limpieza de Imágenes y Contenedores

Este punto se conoce como house keeping y trata del mantenimiento del espacio en disco, ya que como Swarm depende del algoritmo raft, en esta lógica los contenedores harán uso del disco, las tareas cuando las vamos creando o hacemos deploy de nuevas versiones requerirán nuevas imágenes y aunque estas sean livianas debemos hacer mantenimiento para evitar que se llene el disco.

Utilizaremos un servicio que toma todas las imágenes que no tienen contenedores corriendo y las borra y todos los contenedores que están parados hace n tiempo los borra.

Se trata de un servicio que corre en **modo global** cuyo propósito es conectarse al Docker Daemon y eliminar las cosas que quedaron viejas. Utiliza [meltwarter/docker-cleanup](https://github.com/meltwater/docker-cleanup) que está deprecado por lo que no mostramos los comandos usados.



## Administración de Logs

Los logs también van generando ocupación de disco. Esto podría ocasionar que se caiga un nodo. El log driver por default es json-file, pero podríamos usar uno que utiliza logstash de AWS, utilizar syslog de Linux. Esto lo podemos controlar con:

En la [documentación](https://docs.docker.com/config/containers/logging/configure/) nos encontramos con la configuración de los logging drivers que nos permite configurar el driver, limitar el tamaño por log y la cantidad de archivos.



## Herramientas de Monitoreo

Es posible limitar la cantidad de recursos utilizados por las tareas de un servicio mediante un flag en docker service create. Para ello es importante ver de manera gráfica qué sucede con el swarm. 

Existen distintas alternativas, una de ellas es Premetheus o una paga llamada Datadog
