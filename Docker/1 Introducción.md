# Docker

> Basado en Ultimate Docker Course de Mosh Hamedani (VIDEO 26 COMPLETO)

## ¿Qué es Docker?

Docker es una **plataforma para construir, ejecutar y enviar aplicaciones de manera consistente**, de manera tal que si una aplicación funciona en un entorno de desarrollo también funcionará en otras máquinas. 



> Posibles Fallas

Existen muchas **razones por las que una aplicación puede fallar en otra máquina** a pesar de funcionar perfectamente donde la estamos desarrollando:

* **Archivos faltantes** (por ejemplo que no hayan sido incluidos en el deployment)

* **Versiones de software diferentes** (por ejemplo supongamos que la aplicación necesita Node 14 y en la máquina destino tenemos Node 9).

* **Variables de entorno o archivos de configuración diferentes**

  

> Contenedor

Utilizando Docker podremos empaquetar nuestra aplicación con todo lo que necesita y así poder ejecutarla en cualquier máquina que cuenta con Docker. Por lo que **si funciona en development funcionará en test y en production**.



> Nuevos miembros del equipo

Además si un miembro se une al equipo no tendrá que perder mucho tiempo realizando la puesta a punto de su máquina para ejecutar la aplicación, no será necesario instalar las dependencias sino que simplemente deberá ejecutar `docker-compose up` para levantar la aplicación y con esto Docker descargará las dependencias y las ejecutará en un entorno aislado llamado **contenedor**. Esto posibilita que puedan coexistir en la misma máquina  con distintas aplicaciones que usan distintas versiones de software. Por ejemplo puede ser que la aplicación 1 utilice Node 14 y la aplicación 2 Node 9 y esto no representaría un problema.



> Desinstalación

Por último no necesitamos más una aplicación, podemos eliminarla y eliminar todas sus dependencias de  una con el siguiente comando: `docker-compose down --rmi all`



## Contenedores vs Máquinas Virtuales

**Un contenedor es un entorno aislado para ejecutar la aplicación** y tiene algunas diferencias comparado con una máquina virtual.

Una **máquina virtual**  es una abstracción de una máquina o pieza de hardware. Es posible correr varias máquinas virtuales en una máquina física.
Por ejemplo es posible en una Mac y en ella ejecutar dos máquinas virtuales una con Windows y la otra con Linux. Esto se logra gracias a un **Hypervisor** que es un programa con el que podremos manejar máquinas virtuales. Existen  distintos tipos de Hypervisors como *VirtualBox* y *VMware* que son multi-plataforma y *Hyper-v* que funciona solo en Windows.
La **ventaja** del uso de maquinas virtuales está en que podremos correr en la misma máquina física dos aplicaciones de manera  aislada y si estas tienen dependencias de distintas versiones no se generarán inconvenientes.
La **desventaja** de las máquinas virtuales está dada por el hecho de que cada VM requiere un sistema operativo completo, esto hace que sean lentas para iniciar y que sean bastante demandantes en cuanto a recursos (tomarán una porción del hardware físico es decir CPU, memoria y disco). Si bien podemos configurar cuantos cores o cuanta memoria destinamos para cada VM, tendremos una limitación en términos del número de VMs que podremos ejecutar. En una máquina física con más de 5 VMs ya podríamos comenzar a experimentar inconvenientes.

 

Los **contenedores** nos permiten también ejecutar múltiples aplicaciones de manera aislada pero son más livianos pues no necesitan un sistema operativo completo. De hecho, todos los contenedores de una máquina comparten el OS del host. 
Además como el OS ya ha iniciado en el host inician más rápido (1 segundo). Por último son menos demandantes en cuanto a recursos de hardware. En un mismo host podremos ejecutar decenas y hasta centenas de contenedores.  



## Arquitectura Docker

 Docker tiene una **arquitectura cliente-servidor** en la cual el cliente se comunica con el servidor a través de una API REST. El server recibe el nombre de **Docker Engine** se ejecuta de fondo y se encarga de construir y ejecutar contenedores docker.

Los contenedores son procesos como los demás procesos que se ejecutan en la computadora pero con algunas características especiales. Como dijimos a diferencia de las VM no contienen un OS completo sino que todos los contenedores del host comparten el OS del host (en especial el kernel). El kernel es el encargado de manejar todas las aplicaciones y recursos de hardware (como memoria y CPU). Cada sistema operativo tiene su propio kernel y estos tiene una API en espacial y es por eso que no podemos ejecutar aplicaciones Windows en Linux y esto es debido a que esa aplicación debe hablar con el kernel del sistema operativo que tien por debajo. 

* En una máquina con Linux sólo podremos ejecutar contenedores Linux.
* En una máquina con Windows 10 podremos ejecutar contenedores Windows + Linux, ya que esta versión de Windows viene con un kernel de Linux además del kernel de Windows de siempre.
* En una máquina con Mac como su kernel no tiene soporte nativo para nuestros contenedores y debemos utilizar una Linux VM para ejecutar contenedores Linux.



## Instalación Docker

Para instalar Docker debemos ir a https://docs.docker.com/get-docker/ y hacer click en Docker Desktop for Windows.

La [documentación](https://docs.docker.com/docker-for-windows/install/) indica que debemos tener WSL-2 instalado de acuerdo a [esta guía](https://docs.microsoft.com/en-us/windows/wsl/install-win10). Recordar reiniciar antes de instalar el `msi` como indica la guía.

>  Luego desde la terminal con el comando `docker version` obtenem os la versión instalada tanto del cliente como del server.



## Flujo de Trabajo

A partir de una aplicación debemos agregar un archivo Dockerfile (con esta capitalización y sin extensión) que es un archivo de texto plano con instrucciones que usa Docker para empaquetarla en una **imagen** con todo lo que nuestra aplicación necesita para correr (un OS, un runtime environment como ser Node o Python, archivos de la aplicación, third-party libraries, variables de entorno). Una vez que tenemos la imagen le diremos a **Docker que inicie un contenedor usando esa imagen**. El contenedor es un tipo de proceso especial que tiene su propio filesystem provisto por la image. En lugar de ejecutar la aplicación dentro de un proceso normal le decimos a docker que la ejecute dentro de un contenedor.

Una vez que tenemos esta imagen podemos pushearla a un Docker Registry como DockerHub y luego podemos pullearla hacia cualquier máquina que use Docker. De esta manera tendremos la misma imagen y podremos iniciarla del mismo modo que en desarrollo, bastará con indicarle a Docker que inicie un contenedor con esa imagen. Con esto nos evitamos tener que generar documentación con los pasos a seguir para ese release.



### Flujo en Development

Supongamos que queremos desarrollar un simple programa que muestra un mensaje en la consola. Para ello creamos un directorio `hello-docker` y dentro de el un archivo `app.js` con `console.log('Hello Docker');` 

Luego si no utilizaramos Docker y quisiéramos enviar este programa a otra computadora deberíamos tener Node.js instalado y recién ahí ejecutar: `node app.js`

Deberíamos hacer las siguientes instrucciones:

1. Iniciar con un OS
2. Instalar Node
3. Copiar los archivos de la aplicación
4. Ejecutar `node app.js`

En esta aplicación que es muy simple debemos seguir varios pasos lo cual nos permite tomar conciencia de que en aplicaciones de magnitud tendremos que seguir al pie de la letra extensos *release documents*.



Utilizando Docker podemos escribir estas instrucciones dentro de un Dockerfile para empaquetar nuestra aplicación.

> Visual Studio Code al crear el archivo `Dockerfile` nos preguntará si queremos instalar una extensión.**Docker | Microsoft**





> 1. Comenzamos de una imagen base que podría ser un Linux vacío (`FROM linux`)  al cual luego le instalaríamos Node o sino podemos usar una imagen construida sobre la base de Linux con Node ya instalado  (`FROM node` ). Estas imagenes están disponibles en [Docker Hub](hub.docker.com) que como dijimos es el registro de imagenes Docker. Una vez allí podremos buscar "node" y veremos que el nombre de la imagen oficial es `node` y por eso ponemos `FROM node`. En Docker Hub veremos que hay múltiples imagenes node cada una de las cuales fue creada sobre una distribución de distinta de Linux y lo especificaremos mediante un tag. En nuestro caso utilizaremos una distribución liviana de Linux llamada `alpine`
>
> 2. Luego debemos copiar los archivos de la aplicación por lo que ponemos `COPY . /app` con esto le decimos que queremos copiar todos los archivos del directorio actual en el directorio `/app` de la imagen.
>
> 3. Finalmente para ejecutar la aplicación lo hacemeos con `CMD node /app/app.js`. Otra alternativa es setear `/app` como directorio de trabajo lo cual hacemos con `WORKDIR /app` y de esta manera después bastará con ejecutar `CMD node app.js`

```dockerfile
	FROM node:alpine
	COPY . /app
	WORKDIR /app
	CMD node app.js
```



Luego queremos que Docker empaquete nuestra aplicación y lo hacemos con `docker build -t hello-docker .`. Con `-t hello-docker` asignamos un tag y luego indicamos donde puede contrarse el `Dockerfile` que como es en la raíz ponemos el punto `.`.



Para listar todas las imagenes ejecutamos `docker images` o `docker image ls`

```bash
REPOSITORY     TAG       IMAGE ID       CREATED          SIZE
hello-docker   latest    6b6e563d95c7   35 seconds ago   112MB
```

> Notar que Docker por default nos agregó el **tag** `latest`  y como podemos asumir, tags nos permiten identificar la versión la imagen de nuestra aplicación.
>
> Como podemos ver el **tamaño** total es de 112MB utilizando Alpine Linux, Node y nuestra aplicación. Si hubieramos utilizado otra distribución de Linux más pesada el tamaño hubiera sido superior y esto afectaría el proceso de deployment donde debemos transferir la imagen de una computadora a otra.
>
> Para ejecutar la imagen debemos ingresar`docker run hello-docker` lo cual podremos realizarlo desde cualquier directorio.



Luego podremos publicar esta imagen en Docker Hub de modo tal que cualquier pueda utilizarla, luego podríamos ir a otra máquina de test o produccción y hacer el pull y ejecutar la imagen. En el caso de este curso la imagen podemos encontrarla con el nombre `codewithmosh/hello-docker`



> Existe un recurso llamado [Play with Docker](https://labs.play-with-docker.com/) (debemos loguearnos con nuestro DockerID) en el cual podremos crear una VM con Linux y Docker haciendo click en **Add New Instance** y utilizarla como Playground. Podríamos ejecutar `docker pull codewithmosh/hello-docker` y luego podremos verificarlo con `docker image ls` y por último ejecutarla con `docker run codewithmosh/hello-docker`.



## Linux

Docker fue construida utilizando conceptos básicos de Linux es por eso que es importante conocer los comandos elementales de Linux para aumentar la productividad y simplificar la búsqueda de errores



### Comandos

> Lo relacionado a comandos de Linux se unificó con los apuntes de Terminal y Línea de Comandos.



### Distribuciones

Linux es software open-source y es por eso que muchos individuos y comunidades han creado sus propias versiones de Linux llamadas *distribuciones* para satisfacer sus necesidades como correr en servidores, computadoras de escritorio,dispositivos móviles, etc. Existen miles de distribuciones Linux entre las cuales nos encontramos con Ubuntu, Debian, Alpine, Fedora, CentOS, etc. La mayoría de las distros soportan los mismos comandos pero puede haber alguna que otra diferencia. Utilizaremos Ubuntu.



## Iniciar Contenedor

En [hub.docker.com](hub.docker.com) podemos encontrar la imagen `ubuntu` y con `docker pull ubuntu` podríamos obtenerla. Otra forma de hacerlo con el *shortcut* `docker run ubuntu` que hace que si tenemos la imagen Docker iniciará un contenedor con esta imagen y sino la pulleará detrás de escenas y luego iniciará un contenedor.

A continuacón como no interactuamos con el contenedor, este se detendrá. Esto lo podemos verificar con el comando `docker ps`  con el cual podremos ver la lista de procesos o contenedores en ejecución que en nuestro caso no entrega nada pues no tenemos nada corriendo. Mientras que con `docker ps -a` veremos todos los procesos incluso los contenedores que se detuvieron y en ese caso sí vemos el contenedor en cuestión.

Para inicializar un contenedor e interactuar con él lo hacemos con `docker run -it ubuntu` y en ese momento nos aparecerá el shell prompt que es un programa que toma nuestros comandos y se los pasa al OS (o al kernel) para su ejecución.

```shell
λ docker run -it ubuntu
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
16ec32c2132b: Pull complete
Digest: sha256:82becede498899ec668628e7cb0ad87b6e1c371cb8a1e597d83a47fac21d6af3
Status: Downloaded newer image for ubuntu:latest
root@5aacf34bd61d:/#
```



En el shell prompt podemos ver `root@5aacf34bd61d:` donde `root` es el usuario logueado actualmente y `5aacf34bd61d` es el nombre de la máquina y `/` es el directorio raíz y `#` indica que tenemos los niveles de privilegio más altos pues estamos logueados como usuario `root`. En caso de estar logueados como usuarios normales veríamos un `$`.



Si queremos cerrar la sesión de la terminal podemos ejecutar `exit` y luego con `docker ps -a` veremos el proceso detenido. Si queremos reiniciarlo podemos hacerlo con `docker start -i 2f7` siendo `2f7` las primeras tres letras del contenedor.



> Podremos escribir las primeras dos o tres letras a menos que haya un contenedor que tenga las mismas, en cuyo caso tendremos que escribir mas caracteres.

