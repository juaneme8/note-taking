# Docker

> Basado en Ultimate Docker Course de Mosh Hamedani (VIDEO 36 COMPLETO)

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

> Visual Studio Code al crear el archivo `Dockerfile` nos preguntará si queremos instalar una extensión.**Docker | Microsoft** lo cual aceptamos.



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



## Loguin con Usuario

Si estamos logueados con root y queremos loguearnos como un usuario normal **en ese mismo contenedor** abrimos una nueva ventana de la terminal y con `docker ps` podremos ver los contenedores en ejecución. Luego suponiendo que el id `17fd53d105f6` y para correr una sesión de bash ejecutamos `docker exec -it 17f bash`  con lo que nos loguearemos también como **root**. Como queremos loguearnos como `john` debemos primero cerrar la sesión con `exit` y ejecutar `docker exec -it -u john 17f bash` 

* Si ejecutamos  `whoami` obtendremos `john`.
* Si ejecutamos `cd ~` y luego `pwd` veremos `/home/john`.
* En el prompt veremos un signo `$` en lugar del `#` lo cual da cuenta que somos un usuario normal y no root. Es por esto que si queremos ejecutar `cat /etc/shadow` obtendremos **Permission denied**. 



# Imagenes Docker

## Imagenes vs Contenedores

Las **imagenes** contienen todos todos archivos y configuraciones que necesita la aplicación para correr: un sistema operativo reducido, librerías externas, archivos de la aplicación, variables de entorno. Una vez que tenemos una imagen podemos iniciar un contenedor de ella.

Los **contenedores** proporcionan un entorno aislado para ejecutar la explicación, pueden ser iniciados y detenidos y técnicamente son un proceso del sistema operativo. Se trata de un proceso especial que tiene su propio filesystem provisto por la imagen. 

Si tenemos un contenedor corriendo y desde otra ventana de la terminal ejecutamos `docker ps` podremos obtener el *container id* y para iniciar otro contenedor a partir de la misma imagen debemos ejecutar `docker run -it ubuntu` (siendo `ubunto` el nombre del contenedor). Sin embargo si vamos a `/home` no veremos los mismos archivos que hayamos generado allí usando el otro contenedor. **Es por eso que decimos que un contenedor es un entorno aislado** que obtiene el filesystem de la imagen pero tiene su propia capa de escritura (de modo que lo que escribamos en uno, no será visible en los otros contenedores). Sin embargo, luego veremos que existe un modo de compartir datos entre contenedores.



## Dockerizar Aplicación

Cuando tenemos una aplicación React y queremos correrla en una nueuva máquina debemos seguir una serie de pasos empezando por instalar Node.js, ejecutar `npm install` y por último `npm start` para iniciarla. Utilizando Docker buscamos no tener que seguir todos estos pasos cada vez que deseamos utilizar una máquina nueva. Vamos a *dockerizar* la aplicación y meterla en una imagen con la cual podremos deployarla en cualquier lugar.

### `Dockerfile`

El archivo `Dockerfile` contiene instrucciones para construir la imagen, ya hemos visto algunas ahora repasaremos la lista completa:

* `FROM` para especificar la imagen base.
* `WORKDIR` para especificar el directorio de trabajo.
* `COPY` nos permite copiar archivos y directorios a la imagen.
* `ADD` nos permite también copiar archivos y directorios a la imagen pero con características extra.
* `RUN` para ejecutar comandos del sistema operativo.
* `ENV` para configurar variables de entorno.
* `EXPOSE` para especificar el puerto del contenedor
* `USER` para especificar el usuario que puede correr la aplicación (normalmente será un usuario con privilegios limitados).
* `CMD` para especificar el comando que debe ser ejecutado al iniciar la aplicación.
* `ENTRYPOINT`



### Comando `FROM`

El comando `FROM` nos permite establecer la imagen base que podrá ser un sistema operativo como Linux o Windows o un OS + un entorno de ejecución (si somos desarrolladores JavaScript nos convendrá empezar por una imagen node). En [docs.docker.com/samples/](https://docs.docker.com/samples/) podemos encontrar ejemplos de `Dockerfile` para los distintos stacks tecnológicos. 

> Las imágenes pueden estar en cualquier *registry*, el default es **DockerHub** pero por ejemplo Microsoft usa **mcr** (microsoft container registry). En caso de utilizar otro *registry* debemos ingresar la dirección completa. 



En [hub.docker.com](hub.docker.com) buscamos "node" y dentro del repositorio de la imagen oficial nos encontramos con muchas imagenes. Si vamos al apartado tags podremos ver la versión de Node y sobre qué distro+versión de Linux está instalado. Podremos filtrar por versión 14 y veremos nuevamente muchas imagenes y a su vez cada una tendrá distintas versiones segun el CPU (cuando hagamos el pull descargará la más adecuada para nuestra arquitectura).



Siempre debemos especificar una versión ya que si ponemos simplemente`FROM node` es como si pusiéramos `FROM node:latest` y esto no es recomendable ya que si en un futuro volvemos a reconstruir la imagen de la aplicación y hay una versión de Node, la reconstruirá con esa nueva versión.



Por ejemplo dentro del tag `14-buster` veremos:

* linux/amd64
* linux/arm/v7
* linux/arm64/v8

Todas estas pesan comprimidas alrededor de 300 MB y descomprimida estará cerca del GB. Como buscamos velocidad en nuestros deployments utilizaremos una **Alpine Linux**, buscando el tag `14.16.0-alpine3.13` que tiene un tamaño comprimido cercano a los 40 MB.

> Agosto 2021 cuando dentro de la imagen oficial de Node filtramos por Alpine nos encontramos con `current-alpine3.14`.

En el `Dockerfile` ponemos:

```dockerfile
FROM node:14.16.0-alpine3.13
```

Luego para construir la imagen desde el directorio del proyecto:

```bash
docker build -t react-app .
```

* Con `-t react-app` le indicamos el tag que queremos darle a la imagen.
* Con `.` indicamos donde puede encontrar el archivo `Dockerfile`



Con `docker images` o `docker image ls` veremos todas las imagenes disponibles.

Para iniciar un contenedor con esta imagen ejecutamos 

```bash
docker run -it react-app
```

* Con `-it` indicamos que queremos ejecutarlo en modo interactivo.

  

A partir de este momento estaremos en un entorno Node por lo que podremos poner código JavaScript y Node.js lo ejecutará. Como queremos navegar el filesystem, presionamos `Ctrl+C` dos veces y así detendremos el contenedor.



```bash
docker run -it react-app bash
```

Con `bash` indicamos el comando a ejecutar cuando iniciemos el contenedor.


Pero como Alpine Linux no viene con Bash obtendremos un error. Usaremos en cambio shell que sí viene incorporado.

```bash
docker run -it react-app sh
```

Con `sh` indicamos que queremos ejecutar shell cuando iniciemos el contenedor.

Luego podremos ejecutar `ls` y veremos todos los directorios de Linux o `node --version` para conocer la versión de Node que tenemos instalada.

 Hasta ahora en esta imagen tenemos Alpine Linux y Node.js pero no tenemos los archivos de la nuestra aplicación por lo que debemos copiarlos.



### Comando `COPY`

El siguiente paso es copiar los archivos de la aplicación en la imagen y esto lo hacemos en el `Dockerfile` con las instrucciones `COPY` o `ADD`  (que tiene algunas características adicionales). Solo podremos copiar cosas del directorio de trabajo actual (donde tenemos el `Dockerfile`). Esto es así ya que cuando ejecutamos `docker build -t react-app .` **Docker Client** envía a **Docker Engine** el contenido de este directorio (*build context o directory context*) y comienza a ejecutar las instrucciones del `Dockerfile`.

* Si queremos copiar `package.json` en el directorio `/app` dentro de la imagen (como no existe ese directorio lo creará). 

```
COPY package.json /app
```



* Si queremos copiar más de un archivo:

```dockerfile
COPY package.json README.md /app/
```

> Notar que es necesario terminar con `/` cuando copiamos más de un archivo, de lo contrario obtendremos un error de sintaxis.



* Si queremos copiar todos los archivos que cumplen un cierto patrón:

```dockerfile
COPY package*.json /app/
```



En nuestro caso queremos copiar todo el contenido del directorio de trabajo en `/app         `

```dockerfile
COPY . /app
```

Con `/app` estamos utilizando una ruta absoluta, si queremos utilizar rutas relativas debemos utilizar la instrucción `WORKDIR`

```dockerfile
WORKDIR /app
COPY . .
```



* Si queremos copiar un archivo que tiene un espacio en su nombre, debemos utilizar una array de strings donde cada elemento representa un argumento de la instrucción.

```dockerfile
COPY ["hello world.txt", "."]
```



### Comando `ADD`

El comando `ADD` nos permite realizar lo mismo que con `COPY` y dos funciones extra:

* Copiar un archivo de una URL en nuestra imagen

```dockerfile
ADD http://.../file.json .
```

* Podremos pasarle un archivo comprimido y lo descomprimirá automáticamente en el directorio

```dockerfile
ADD file.zip .
```

La instrucción `ADD` es aconsejable utilizarlo sólo cuando necesitemos estas dos caracterísitcas, siempre que sea posible utilizar `COPY`



Por último creamos la imagen e iniciamos un contenedor con esta imagen y veremos que estamos en `/app` ya que es nuestro directorio de trabajo y con `ls` podremos ver todos los archivos y directorios que tenemos en nuestro proyecto (incluido `node_modules`).



### `.dockerignore` 

En el procedimiento anterior hemos copiado `node_modules` lo cual ocasiona que el *build context* sea bastante pesado (cercano a los 150 MB, cosa que vemos en el output del build donde dice *transferrring context*) aún en un proyecto que casi no tiene funcionalidades. A la hora del deployment veremos que el Docker Client debe hablar con un Docker Engine en otra máquina y transferir el contenido del build context por medio de la red. No es necesario transferir este directorio ya que en `package.json` tenemos definidas todas las dependencias y podremos reinstalarlas en la imagen de destino.

El build context es menor por lo que debemos transfererir **menos datos por la red**, lo que ocasionará que el build será más **rápido**.

De la misma manera que tenemos el archivo `.gitignore`, podemos crear en la raíz un archivo `.dockerignore` y listar allí los archivos y directorios a excluir.

```
node_modules/
```

Si ahora ejecutamos nuevamente el build veremos que nos aparece como *transferring context* un valor cercano a los 10 KB. 



Como no tenemos la carpeta `node_modules` debemos ejecutar `npm install` cosa que haremos a continuación con el comando `RUN`.



### Comando `RUN`

El comando `RUN` nos permitirá ejecutar cualquier comando y en nuestro caso lo usaremos para instalar las dependencias.

Por lo que en el `Dockerfile` agregamos:

```dockerfile
RUN npm install
```



> Si queremos instalar por ejemplo Python podríamos hacer `RUN apk install python` (usamos `apk` en lugar de `apt` ya que en Alpine Linux viene este manejador de paquetes).



Luego creamos la imagen (y como parte del build se descargarán  e instalarán las dependencias) e iniciamos un nuevo contenedor. Ahora si hacemos `ls -1` veremos que tenemos la carpeta `node_modules`.



### Comando `ENV`

El comando `ENV` nos permite setear variables de entorno.

```dockerfile
ENV API_URL=http://api.myapp.com/
```



> También es posible utilizar una sintaxis antigua que omite el signo `=`

```dockerfile
ENV API_URL http://api.myapp.com/
```



Luego reconstruimos la imagen e iniciamos un nuevo contenedor. 

* Con `printenv` podremos ver todas las variables de entorno
* Con `printenv API_URL` podremos ver el valor de esa variable en particular.
* Con `echo $API_URL` también podremos ver el valor de esa variable.









Versionar imagenes

Compartir imagenes

Guardar y cargar imagenes

Reducir tamaño imagenes

Acelerar buillds

