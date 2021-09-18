# Docker

> Basado en Ultimate Docker Course de Mosh Hamedani (VIDEO 71 COMPLETO)

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



## Imagenes

### Aplicación Docker

Cuando tenemos una aplicación React y queremos correrla en una nueuva máquina debemos seguir una serie de pasos empezando por instalar Node.js, ejecutar `npm install` y por último `npm start` para iniciarla. Utilizando Docker buscamos no tener que seguir todos estos pasos cada vez que deseamos utilizar una máquina nueva. Vamos a *dockerizar* la aplicación y meterla en una imagen con la cual podremos deployarla en cualquier lugar.



### Imagenes vs Contenedores

Las **imagenes** contienen todos todos archivos y configuraciones que necesita la aplicación para correr: un sistema operativo reducido, librerías externas, archivos de la aplicación, variables de entorno. Una vez que tenemos una imagen podemos iniciar un contenedor de ella.

Los **contenedores** proporcionan un entorno aislado para ejecutar la explicación, pueden ser iniciados y detenidos y técnicamente son un proceso del sistema operativo. Se trata de un proceso especial que tiene su propio filesystem provisto por la imagen. 

Si tenemos un contenedor corriendo y desde otra ventana de la terminal ejecutamos `docker ps` podremos obtener el *container id* y para iniciar otro contenedor a partir de la misma imagen debemos ejecutar `docker run -it ubuntu` (siendo `ubuntu` el nombre del contenedor). Sin embargo si vamos a `/home` no veremos los mismos archivos que hayamos generado allí usando el otro contenedor. **Es por eso que decimos que un contenedor es un entorno aislado** que obtiene el filesystem de la imagen pero tiene su propia capa de escritura (de modo que lo que escribamos en uno, no será visible en los otros contenedores). Sin embargo, luego veremos que existe un modo de compartir datos entre contenedores.



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



> Cada vez que volvamos a crear la imagen tardará un tiempo considerable debido a la instalación de las dependencias y esto es algo que luego optimizaremos.



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



### Comando `EXPOSE`

Cuando trabajamos con React.js de manera tradicional, ejecutamos `npm start` y luego vamos a `localhost:3000` para acceder a la aplicación. Cuando ejecutemos esta aplicación en un contenedor **el puerto estará abierto en el contenedor** pero no en el host. En la misma máquina podremos tener múltiples contenedores corriendo la misma imagen y todos ellos estarán escuchando en el puerto 3000. Veremos luego cómo mapear un puerto del host con un puerto del contenedor.



El comando `EXPOSE` nos permite **documentar** en qué puerto estará escuchando eventualmente este contenedor, pero no tiene nada que ver con publicar puertos del host por lo que tendremos luego que mapear un puerto del host con el 3000 del contenedor.

```
EXPOSE 3000
```



### Comando `USER`

 Por default Docker ejecuta las aplicaciones con el usuario `root` que es el que tienen el nivel de privilegios más alto lo cual podría ocasionar agujeros de seguridad en nuestra aplicación. Debemos crear un usuario regular con privilegios limitados.

Antes de hacerlo en el `Dockerfile` lo haremos en una sesión de shell en Alpine Linux:

```bash
docker run -it alpine
```



En primer lugar con el comando `addgroup` vamos a crear un grupo primario llamado `app` para luego crear un usuario y agregarlo a dicho grupo:

```bash
addgroup app
```



A continuación creamos un usuario con el comando `adduser` (el comando `useradd` no está disponible en Alpine Linux). Lo hacemos con las opciones `-G` para crear un grupo primario y `-S` para crear un *system user* (no es un usuario normal sino que es un usuario para ejecutar la aplicación).

```bash
adduser -S -G app app
```

> Es una práctica común darle el mismo nombre al grupo y al usuario (en este caso ambos son `app`).



Para verificar los grupos del usuario `app`

```bash
groups app
```



Es posible combinar los primeros dos comandos en uno que realice ambas tareas:

```bash
addgroup app && adduser -S -G app app
```



Esta línea es la que agregaremos al `Dockerfile`

```dockerfile
RUN addgroup app && adduser -S -G app app
```



Luego indicamos que queremos utilizar el usuario `app` para todos los comandos que se ejecuten a continuación:

```dockerfile
USER app
```



Luego reconstruimos la imagen e iniciamos un nuevo contenedor. Para verificar que el usuario actual de la sesión es `app` ejecutamos:

```
whoami
```

Si ejecutamos `ls -l` veremos que todos los archivos son de `root` y como somos `app` caemos dentro de "others" en términos de permisos, por lo que como tiene `r--` no tenemos permisos de ejecución.



### Comando `CMD`

Normalmente para iniciar la aplicación desde el directorio de la aplicación ejecutamos `npm start`



Como ya tenemos la imagen, para iniciar el contenedor en este caso no utilizaremos el modo interactivo porque no queremos interactuar (correr una sesión de shell) con el contenedor, sino que queremos en cambio ejecutar ese comando

```bash
docker run react-app npm start
```

Sin embargo obtendremos un error **permission denied** ya que si analizamos el `Dockerfile`

```dockerfile
FROM node:14.16.0-alpine3.13
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.myapp.com/
EXPOSE 3000
RUN addgroup app && adduser -S -G app app
USER app
```

Como podemos ver estamos ejecutando todos los comandos como `root` y a lo último estamos cambiando `app` que es un usuario con privilegios limitados, por lo que colocamos los últimos dos comandos al comienzo:

```dockerfile
FROM node:14.16.0-alpine3.13
RUN addgroup app && adduser -S -G app app
USER app
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.myapp.com/
EXPOSE 3000
```

Reconstruimos la imagen e iniciamos nuevamente el contenedor.

```bash
docker build -t react-app .
docker run react-app npm start
```



Veremos que el webserver inicia en el puerto 3000 pero como ya dijimos es el puerto 3000 del contenedor y no del host, por lo que si entramos a http://localhost:3000 no veremos la aplicación.

Para evitar tener que ingresar `npm start` cada vez que iniciamos el contenedor es que utilizamos el comando `CMD`, con el cual establecemos el **comando default**, en nuestro caso será:

```dockerfile
CMD npm start
```

> `CMD` represente el comando default por lo que si tenemos más de una línea con `CMD` sólo la última tendrá validez.



Por lo que el `Dockerfile` nos queda:

```dockerfile
FROM node:14.16.0-alpine3.13
RUN addgroup app && adduser -S -G app app
USER app
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.myapp.com/
EXPOSE 3000
CMD npm start
```

Luego de reconstruir la imagen iniciamos el contenedor simplemente con:

```bash
docker run react-app
```



> Si bien es posible ejecutar comandos tanto con `RUN` como con `CMD`, tienen la diferencia que `RUN` es ejecutada durante *build-time* es decir mientras estamos construyendo la imagen y `CMD` durante *run-time es decir al iniciar el contenedor.



El comando `CMD` podemos utilizarlo de dos formas:

* **Shell form**: Docker lo ejecutará dentro de una shell separada (`/bin/sh` en Linux o `cmd` en Windows)

```dockerfile
CMD npm start
```

* **Execute form**: nos permite ejecutarlo directamente sin abrir otro proceso de shell. **Es una buena práctica utilizar esta opción**. Además es más simple y rápido hacer la limpieza de recursos al detener contenedores.

```dockerfile
CMD ["npm", "start"]
```



### Comando `ENTRYPOINT`

El comando `ENTRYPOINT` es similar a `CMD` y tiene también la **shell form** y **execute form**:

```dockerfile
ENTRYPOINT npm start
```

```dockerfile
ENTRYPOINT ["npm", "start"]
```

La diferencia está en que es más simple sobrescribir el comando default (es decir el invocado con `CMD`) de hecho esto es lo que hacemos por ejemplo cuando ejecutamos `docker run -it react-app sh`. Debemos utilizar `CMD` cuando queremos tener flexibilidad de que podremos sobrescribir el comando.

En cambio para sobrescribir el `ENTRYPOINT` es necesario utilizar la opción`--entrypoint` . En la práctica conviene utilizar `ENTRYPOINT` cuando estamos seguros del comando que debe ser ejecutado al iniciar un contenedor.

Sin embargo la elección de uno u otro comando depende de lo que quiera cada programador. Mosh personalmente utiliza`CMD`.



### Optimizar Builds

Cada vez que hacemos un cambio por más pequeño que sea debemos reconstruir la imagen y esto demora bastante tiempo, lo cual se debe principalmente a la instalación de las dependencias. 

Una imagen es una colección de **layers**, pudiendo pensar a los layers como un pequeño file-system que incluye sólo los archivos modificados. Cuando Docker crea una nueva imagen ejecuta una a una las instrucciones del `Dockerfile` creando un nuevo layer que sólo incluye los archivos que fueron modificados como consecuencia de esa instrucción.

Para la primera instrucción `FROM node:14.16.0-alpine3.13` Docker toma la imagen de Node y la coloca en un layer (en verdad la imagen de Node son varios layers pero a los fines prácticos suponemos que es uno sólo con todos los archivos de Linux y Node). Luego ejecuta la segunda instrucción `RUN addgroup app && adduser -S -G app app` y crea un nuevo layer con los archivos que se modifican luego de ejecutarlo (crea un nuevo layer ya que al crear un usuario o un grupo algo se escribe en el filesystem). De manera similar continúa ejecutando los siguientes comandos y creando layers.

El comando `docker history` nos permite conocer los layers asociados a cada imagen. Podremos visualizar el comando que creo ese layer y el tamaño de esta capa. Debemos leer la tabla de abajo hacia arriba. Primero veremos una serie de layers de Linux y Node.js. Luego veremos un layer de poco tamaño correspondiente al comando para crear un grupo y crear un usuario y añadirlo a dicho grupo. Sobre este comando veremos `RUN /bin/sh -c addgroup app && adduser -S -G app` y el hecho de que sea ejecutado dentro de `/bin/sh` es por haber utilizado la forma "shell" en lugar de la forma "execute" de RUN. Cuando ejecutamos `npm install` (veremos también que es ejecutado en `/bin/sh` por haber usado la forma "shell") todas las dependencias son instaladas y almacenadas en esta capa (veremos que tiene un peso significativo).

```bash
docker history react-app
```



Docker cuenta con un **mecanismo de optimización** por el que cada vez que construye una imagen y llega a una instrucción, supongamos `FROM node:14.16.0-alpine3.13` se fijará si esta no ha cambiando respecto de la última vez y en ese caso no reconstruirá dicho layer sino que lo reutilizará de caché. Repitiendo lo mismo para las restantes instrucciones. Cuando llegamos a `COPY . .` bastará con haber cambiado una línea de código para que no pueda reutilizar ese layer y deberá reconstruirlo. Además como consecuencia de esto todos los layers subsiguientes deberán recontruirse también. Entre esas instrucciones tenemos `npm install` y es el cuello de botellas por el cual debemos esperar cerca de medio minuto para que se instalen todas las dependencias.

Hasta ahora tenemos:

```dockerfile
COPY . .
RUN npm install
```

Una solución puede ser copiar prime ro los archivos que necesitamos para instalar las dependencias `package.json` y `package-lock.json`, instalar y luego copiar el resto de los archivos. 

```dockerfile
COPY package*.json .
RUN npm install
COPY . .
```

Como consecuencia de esto al llegar a `COPY package*.json .` si no hemos cambiado ninguna de las dependencias, utilizará el layer del caché y lo mismo hará para `RUN npm install`.  Al llegar a `COPY . .` ese layer sí será reconstruído.

A la hora de ejecutar `docker build -t react-app .` veremos la palabra **CACHED** cada vez que un layer sea reutilizado.

> La primera vez que construyamos la imagen con este nuevo Dockerfile obviamente no reutilizará del caché los layers, pero en la medida que hagamos cambios y volvamos a reconstruirla lograremos el comportamiento esperado de no tener que esperar para la instalación de dependencias.



La organización del `Dockerfile` para lograr la máxima optimización tiene que ser tal que las instrucciones estables se ubiquen en la parte superior y aquellas que cambian en la parte inferior.



### Eliminar Imágenes

#### Eliminar Imagenes Colgadas

Cuando ejecutamos `docker images` vemos algunas que tienen como nombre y como tag la palabra  `<none>` las cuales se originan en la medida que hacemos rebuilds de las imagenes. 



Para eliminarlas debemos utilizar el siguiente comando:

```bash
docker image prune
```



> Para obtener ayuda sobre cualquiera de estos comandos `docker image prube --help`, `docker image save --help`, etc.
>
> 



Debemos verificar si tenemos contenedores corriendo versiones viejas de la imagen, lo cual hacemos con 

```bash
docker ps
```

También podemos visualizar aquellos que estan en estado detenido:

```bash
docker ps -a
```

Si queremos eliminar los contenedores detenidos que acabamos de visualizar:

```bash
docker container prune
```

Y Luego volvemos a ejecutar

```
docker image prune
```



#### Eliminar Imagenes con Nombre

Para eliminar una imagen podemos hacerlo con referenciando su nombre o su ID de imagen

```bash
docker image rm react-app
```

```
docker image rm df3
```

> En lugar de `rm` también podemos utilizar la palabra `remove`.

### Etiquetar Imágenes

Cuando hacemos un build (o rebuild) de una imagen o la bajamos de DockerHub, Docker le asigna automáticamente el tag `latest`, lo cual es simplemente una etiqueta, no significa que sea la versión más actual de la imagen.

> La etiqueta `latest` podemos utilizarla en *development* pero no es aconsejable usarla en *staging* o *production* ya que si algo sale mal dificulta la búsqueda de errores o incluso hacer upgrades/downgrades ya que no sabremos qué versión estamos corriendo realmente.



Si creamos una imagen como venimos haciendo hasta ahora, le asignará automáticamente el tag `latest`

```bash
docker build -t react-app .
```

Cosa que podemos verificar con 

```bash
docker images
```



#### Etiquetado al crear imagen

Para etiquetar una imagen podemos hacerlo a la hora de crear la imagen:

Hay equipos que optan por darles nombres en forma de código

```bash
docker build -t react-app:buster .
```

Otra opción es versionar semánticamente. Esta metodología es común en equipos que no sacan *releases* muy seguido:

```bash
docker build -t react-app:3.1.5 .
```

Los equipos que sacan *releases* seguido suelen preferir números de *build*

```bash
docker build -t react-app:76
```

> Este último método puede ser llevado a cabo por herramientas CI/CD que verificarán el código más reciente del repositorio y construirán una imagen y la etiquetarán automáticamente con el número de build.



> Una misma imagen puede tener múltiples tags.

Si queremos eliminar una etiqueta

```bash
docker image rm react-app:1
```



#### Etiquetado luego de crearla

Debemos identificar la imagen que queremos etiquetar ya sea por su tag actual o por su ID.

```
docker image tag react-app:latest react-app:1
```

```
docker image tag f4b react-app:1
```



> También sería posible cambiarle el nombre completamente `docker image f4b juaneme8/react-app:2` en este caso hemos especificado el `2` como tag y si no ponemos  ninguno usará `latest`.



#### `latest` desactualizado

Si tenemos una imagen con la etiqueta latest y luego creamos una nueva imagen con una etiqueta en particular puede que ejecutemos `docker images` y pensemos que la más actual es una distinta de la que realmente es. En ese caso debemos actualizarla manualmente añadiendole a la nueva la etiqueta `latest` del modo visto. Suponiendo que su ID comienza con `b06`:

```bash
docker image tag b06 react-app:latest
```



### Compartir Imágenes

Para compartir imagenes debemos tener una cuenta en [hub.docker.com](hub.docker.com) y luego debemos crear un repositorio que puede alojar múltiples imagenes.

> Con una cuenta gratuita sólo es posible tener un repositorio privado. En la versión paga también es posible vincular con una cuenta GitHub para que cada vez que hagamos un push, DockerHub haga un pull y genere automáticamente una imagen.

Suponiendo que creamo sun repositorio `react-app` debemos renombrar la imagen a `juaneme8/react-app` lo cual hacemos con:

```
docker image f4b juaneme8/react-app:2
```



Luego debemos loguearnos para lo que nos pedirá usuario y contraseña:

```
docker login
```



Finalmente hacemos un push a ese repositorio

```
docker push juaneme8/react-app:2
```

 

A continuación **realizará el push de cada uno de los layers de la imagen**. La primera vez demorará bastante tiempo debido a que está pusheando el layer con las dependencias de npm pero las sucesivas será más rápido (en la medida que no cambiemos las dependencias).



Luego cuando hagamos cambios tendremos que hacer nuevamente el build

```bash
docker build -t react-app:3 .
```

Le asignamos un nuevo tag a la imagen con nuestro nombre de usuario:

```bash
docker image tag react-app:3 juaneme8/react-app:3
```

Finalmente hacemos el push y veremos que varios de los layers ya existen por lo que es mucho más rápido que la primera vez:

```bash
docker push juaneme8/react-app:3
```



En DockerHub si exploramos el rep ositorio veremos ahora ambos tags.

A partir de este momento podremos hacer el pull de la imagen en cualquier máquina que tenga Docker.



### Guardar Imagenes Comprimida

Suponiendo que tenemos una imagen en una máquina y queremos ponerla en otra sin pasar por DockerHub, en ese caso podremos guardarla como un archivo comprimido y cargarlo en la otra máquina.

```bash
docker image save --output react-app.tar react-app:3
```



> El archivo lo estamos creando en el directorio actual.
>
> Si abrimos el archivo veremos que dentro tiene una serie de archivos que representan a cada uno de los layers.



### Cargar Imagen Comprimida

```bash
docker image load --input react-app.tar
```



## Contenedores

### Iniciar Contenedores

Repamos a continuación algunos de los comandos usados hasta ahora. 

Para listar las imagenes disponibles

```bash
docker images
```

Para ver los contenedores corriendo (recordando que en definitiva son procesos con su propio file-system provisto por la imagen) 

```
docker ps
```



Para iniciar un contenedor con la imagen `react-app` podemos hacerlo con

```bash
docker run react-app
```

Luego de unos segundos tendremos el web-server corriendo pero no podremos ingresar ningún comando adicional y si presionamos `CTRL+C` el contenedor se detendrá (lo cual podremos verificar con `docker ps`).



#### Contenedor en el *background*

También es posible ejecutar un contenedor en **modo dettached** (background), de esta manera tendremos la terminal libre y podremos hacer lo que queramos.

```bash
docker run -d react-app
```



Luego de unos segundos cuando haya iniciado el web-server, si ejecutamos `docker ps` veremos ese contenedor y en la última columna `NAME` veremos que le ha asociado un nombre aleatorio provisto por Docker. En el futuro podremos refererirnos a ese contenedor tanto por su `ID` como por su `NAME`.

Si queremos darle nosotros mismos un nombre podremos hacerlo con:

```bash
docker run -d --name blue-sky react-app
```

 



### Logs de Errores

Suponiendo que tenemos los contenedores iniciados anteriormente corriendo en el *background*, cuyos datos podremos obtener con:

```bash
docker ps
```

Para conocer qué está sucediendo en ellos (qué salida mostraron o si hubo algun error) podremos realizar

```bash
docker logs 655
```

> Suponiendo que 655 son los primeros tres números del ID del contenedor.
>
> En este caso veremos la misma salida que cuando iniciamos el contenedor en el *foreground*

 

Con `docker logs --help` podremos ver las distintas opciones con las que podemos utilizar este comando.



#### Log contínuo

Si tenemos un contenedor que está produciendo salidas permanentemente, en lugar de ejecutar `docker log 655` a cada rato, podemos utilizar la opción `-f` o `--follow`

```bash
docker logs -f 655
```

> De esta manera podremos ver los logs en tiempo real, luego tendremos que presionar `CTRL+C` para salir.



Log de últimas n líneas

Suponiendo que queremos ver sólo las últimas 5 líneas del log

```bash
docker logs -n 5
```



#### Log con timestamps

```bash
docker logs -t 655
```



## Publicar Puertos

A pesar de tener dos contenedores corriendo si vamos a `localhost:3000` no podremos acceder a la aplicación. Esto como ya explicamos se debe a que el puerto 3000 está escuchando en el contenedor pero no en el host. Necesitamos publicar este puerto para poder enviar tráfico a través de él.



Con `docker ps` veremos una columna `PORTS` con el contenido `3000/tcp` donde este `3000` hace referencia al puerto del contenedor.

```bash
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS      NAMES
d588f48d6340   react-app     "docker-entrypoint.s…"   3 seconds ago   Up 2 seconds   3000/tcp   stoic_wozniak
```



### Iniciar un contenedor y publicar puerto

```bash
 docker run -d -p 80:3000 --name c1 react-app
```

Con `3000:3000` hacemos referencia al puerto `80` del host y `3000` en el contenedor.

 De esta manera bastaría con entrar a `localhost` (puerto `80` por default) para ver la aplicación.

> Al hacer esto nos aparecerá un cartel del Firewall para autorizar el acceso.



Si ahora ejecutamos `docker ps` veremos el siguiente mapeo de puertos `0.0.0.0:80->3000/tcp`

```bash
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
26ad639003d0   myapp     "docker-entrypoint.s…"   32 seconds ago   Up 30 seconds   0.0.0.0:80->3000/tcp, :::80->3000/tcp   c1
```



## Ejecutar comandos en contenedores corriendo

Cuando iniciamos un contenedor ejecuta el comando default que especificamos en el `Dockerfile`, en este caso `CMD ["npm", "start"]`

```dockerfile
FROM node:14.16.0-alpine3.13
RUN addgroup app && adduser -S -G app app
USER app
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.myapp.com/
EXPOSE 3000
CMD npm start
```

Sin embargo, si queremos ejecutar un comando tiempo después en un contenedor que está corriendo podremos hacerlo con `docker exec`. Con `docker run` iniciamos un nuevo contenedor y ejecutamos este comando default y con `docker exec` ejecutamos un comando en un contenedor que esté corriendo.

```bash
docker exec c1 ls
```

> Siendo `c1` el nombre del contenedor y `ls` el comando que deseamos ejecutar en el, tener presente que el contenedor continuará corirendo.



Veremos el listado de los archivos del directorio `/app` que es el que hemos establecido como *working directory*. 



Si queremos abrir una sesión de shell, en el modo interactivo:

```bash
docker exec -it c1 sh
```

> Cuando queramos cerrar la sesión ingresamos `exit` y esto no repercutirá en el estado del contenedor que continuará corriendo como podremos ver con `docker ps`



## Detener y Reiniciar Contenedores:

El comando `docker stop` nos permite detener un contenedor:

```bash
docker stop c1
```

> La aplicación de este comando podremos verificarla con `docker ps`



Si ahora queremos reiniciar un contenedor detenido, lo hacemos con `docker start`

```bash
docker start c1
```

> La diferencia entre `docker run` y `docker start` radica en que `docker start` lo utilizamos para reiniciar un contenedor detenido y `docker run` para iniciar uno nuevo.



## Eliminar Contenedores

Existen dos formas de eliminar un contenedor, la primera es:

```bash
docker container rm c1
```

O la versión reducida:

```bash
docker rm c1
```



Si el contenedor que queremos eliminar está corriendo nos aparecerá un mensaje **"You cannot remove a running container"**. Tenemos dos opciones, la primera es detenerlo para luego eliminarlo y la segunda es utilizar la opción *force*

```bash
docker rm -f c1
```



> Una vez eliminado lógicamente no lo veremos en los contenedores corriendo `docker ps`, ni en todos los contenedores (incluyendo detenidos) `docker ps -a`. Si queremos chequearlo mediante un comando podríamos hacerlo utilizando pipes con `docker ps -a | grep c1`
>
> Como ya explicamos, el comando `docker container prune` nos permite borrar todas los contenedores detenidos.



## File-System de Contenedores

Cada contenedor tiene su propio file-system que es invisible para los otros contenedores.

Si tenemos dos contenedores corriendo y en uno de ellos creamos un archivo, este no lo veremos reflejado en el file-system del otro contenedor.

En primer lugar verificamos tener dos procesos corriendo y obtenemos sus IDs.

```bash
docker ps
```



Luego ejecutamos una sesión de shell en el primero de ellos y creamos un archivo

```bash
docker exec -it 655 sh
echo data > data.txt
exit
```



Luego iniciamos una nueva sesión shell en el segundo contenedor y si listamos los archivos veremos que no hay ninguno llamado `data.txt`

```
docker exec -it 6eb sh
ls | grep data
```



Si eliminamos el contenedor estaremos eliminando también el file-system por lo que se irán los archivos que tengamos allí. Nunca debemos colocar 

alli nuestra data, en cambio debemos utilizar **volúmenes**.



## Volúmenes

Los volúmenes proporcionan un método de **almacenamiento por fuera de los contenedores**, puede ser en un directorio del host o en algún lugar de la nube.

Podemos ver los subcomandos ingresando:

```bash
docker volume
```

* `docker volume create`
* `docker volume inspect`
* `docker volume ls`
* `docker volume prune`
* `docker volume rm`



### Creación de Volumen

Para crear un nuevo volumen llamado `app-data`

```bash
docker volume create app-data
```



### Inspección de Volumen

Para inspeccionarlo el volumen recién creado.

```
docker volume inspect app-data
```

Veremos un objeto con las propiedades:

```bash
[
    {
        "CreatedAt": "2021-08-31T00:27:28Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/app-data/_data",
        "Name": "app-data",
        "Options": {},
        "Scope": "local"
    }
]
```



* `Driver` dirá `local` en caso de que el volumen sea un directorio en el host, si queremos usar almacenamiento en la nube tendremos que encontrar un driver para esa plataforma.

* `Mountpoint` dirá el directorio donde fue creado este volumen.

  

### Iniciar contenedor con Volumen

Para iniciar un contenedor y pasarle este volumen para que guarde los datos en él, utilizamos la opción `-v app-data:/app/data` siendo `/app/data` un path absoluto en el filesystem del contenedor.

```bash
docker run -d -p 4000:3000 -v app-data:/app/data react-app
```



### Ejemplo de Persistencia de Datos

No es necesario haber creado el volumen de ante mano, si ponemos un volumen que no existe, Docker lo creará automáticamente, por ejemplo si en lugar de `app-data` que sí creamos ponemos `app-data2`

```bash
docker run -d -p 4000:3000 -v app-data2:/app/data react-app
```



Lo mismo sucede para el directorio dentro del contenedor de la aplicación `/app/data` que si no existe Docker lo creará. **Sin embargo esto tiene un inconveniente que veremos a continuación.**



Para iniciar una sesión de shell sobre ese contenedor (suponiendo que el comando anterior nos devolvío como ID del contenedor `7168b9....`)

```
docker exec -it 716 sh 
```

Si una vez allí navegamos a `/app/data` e intentamos crear un archivo `data.txt`:

```
cd data
echo data > data.txt
```

Obtendremos error **can't create data.txt: Permission denied** si hacemos `ls -l` veremos que el dueño del directorio `data` es `root`, siendo sólo este usuario capaz de escribir en este directorio. Al estar trabajando con el usuario `app` caemos dentro de la categoría de permisos *others* y no podemos escribir. Esto sucede debido a que hemos permitido que Docker cree automáticamente este directorio.



Esto lo solucionamos modificando el `Dockerfile` agregando una instrucción para crear el directorio con el usuario `app`.

```dockerfile
FROM node:14.16.0-alpine3.13
RUN addgroup app && adduser -S -G app app
USER app
WORKDIR /app
RUN mkdir data 
COPY . .
RUN npm install
ENV API_URL=http://api.myapp.com/
EXPOSE 3000
CMD npm start
```

Hemos agregado `RUN mkdir data` y nos aseguramos que se ejecute usando el usuario `app` por lo que tendremos permisos de escritura.

Como modificamos el `Dockerfile` debemos reconstruir la imagen:

```bash
docker build -t react-app .
```

Iniciamos un nuevo contenedor:

```bash
docker run -d -p 5000:3000 -v app-data:/app/data react-app
```

> 5000 del host y 3000 del contenedor.

La diferencia será que esta vez el directorio `/app/data` ya existe dentro del file-system de este contenedor y es propiedad del usuario `app`

Por lo que si ahora ejecutamos

```bash
docker exec -it 007 sh
```

> Suponemos que el ID del contenedor comienza con `007`.



```bash
cd data
echo data > data.txt
```

Veremos que ahora al crear este archivo no nos tira ningún error. 

Gracias al uso de volúmenes podremos borrar el contenedor y el archivo seguirá `data.txt` existiendo.

```bash
docker rm -f 007
```

> Lo forzamos con `-f` porque está todavía corriendo



Si iniciamos un nuevo contenedor con el mismo mapeo del volumen:

```bash
docker run -d -p 5000:3000 -v app-data:/app/data react-app
```

Ejecutamos una sesión de shell

```bash
docker exec -it e1c sh
```

> Suponemos que el id del contenedor empieza con `e1c`

Si listamos los archivos dentro del directorio `data`  veremos que tenemos allí al archivo `data.txt` que creamos en el otro contenedor.

```bash
cd data
ls
```



## Copiar archivos entre host y contenedores

### Copiar archivos del contenedor al host

Supongamos que en un contenedor tenemos un log que queremos analizar y para ello deseamos copiarlo al host.

Primero obtenemos el ID del contenedor que está corriendo y cuyo log vamos a simular (suponemos que es `e1c`).


```
docker ps
```

Luego iniciamos una sesión de shell

```
docker exec -it e1c9043ea8ce sh

echo hello > log.txt
```

> No olvidar que debemos ingresar `exit` para salir.



Para copiar este archivo al host utilizamos el comando `docker cp` debemos especificar origen y destino. El origen lo especificamos como `contenedor:ruta` y el destino como `.` porque queremos que sea el directorio actual del host.

```
docker cp e1c9043ea8ce:/app/log.txt .
```

Luego con `ls` podremos verificar que la operación fue exitosa.

 

### Copiar archivos del host al contenedor

Suponemos tenemos un archivo `secret.txt` en el host que como no forma parte del sistema de control de versiones queremos copiarlo manualmente a un contenedor.

```
echo hello > secret.txt
```

Luego para copiarlo 

```
docker cp secret.txt  e1c9043ea8ce:/app
```

Por último iniciamos una sesión de shell

```
docker exec -it e1c9043ea8ce sh
```

Donde con `ls` verificamos la existencia del archivo en cuestión.



## Publicar cambios en un contenedor

Supongamos que tenemos corriendo una aplicación React que visitamos en localhost:3000 y modificamos uno de sus archivos. Estos cambios no los veremos reflejados inmediatamente en el navegador. Para visualizar estos cambios debemos operar distinto segun estemos en producción o en desarrollo.

Para **producción** deberíamos crear una nueva imagen, asignarle un tag apropiado y luego hacer el deploy.

Para **desarrollo** no queremos perder tiempo haciendo una nueva imagen o copiando los archivos al contenedor cada vez que introducimos cambios en el código. Lo que hacemos en cambio será crear un mapeo o binding entre los directorios en el host y los directorios en el contenedor, de manera que cuando hagamos cambios en nuestros archivos sean visibles inmediatamente en el contenedor.

Hasta ahora iniciamos un contenedor con el siguiente comando:

```
docker run -d -p 5001:3000 -v app-data:/app/data react-app
```

Usando la misma sintaxis que para los volúmenes podemos mapear un directorio del host con un directorio en el contenedor. En lugar de usar un *named volume* que es un directorio manejado por Docker usaremos el directorio actual de la aplicación. En lugar de escribir el path completo utilizamos el comando `pwd`, pero lo ponemos como `$(pwd)` porque si ponemos `pwd` solo Docker pensará que se trata de un *named volume*

```
docker run -d -p 5001:3000 -v $(pwd):/app react-app
```

También podríamos acompañarlo de un *named volume* 

```
docker run -d -p 5001:3000 -v $(pwd):/app -v app-data:/app/data react-app
```



Si tenemos el navegador abierto en localhost:5001 cualquier cambio que introduzcamos gracias a lo que hemos hecho y al *hot reloading* será visto de manera instantánea.



## Docker Compose

Docker Compose nos permite ejecutar de manera simple aplicaciones con múltiples contenedores. Como ejemplo consideramos una aplicación en la que tenemos front-end (React), back-end (Node.js) y base de datos (MongoDB).

Docker Compose es una herramienta construida sobre la base de Docker Engine y en la actualidad para Windows y Mac viene incluida en Docker Desktop, lo cual podemos verificar con `docker-compose --version`. 

> En caso de estar utilizando Linux tendremos que ir a https://docs.docker.com/compose/install/ y seguir las instrucciones de instalación.



### Limpieza del Espacio de Trabajo

Si queremos limpiar el espacio de trabajo y eliminar las imagenes y los contenedores que tengamos corriendo, primero podemos analizar ambos aspectos con:

```bash
docker images
docker ps
```



#### Limpieza Manual de Imagenes

Luego para eliminar todas las imagenes, podríamos hacerlo ingresando uno a uno los IDs de la imagen.

```bash
docker image rm 123 321 111 
```



#### Limpieza Automática de Imágenes

Sin embargo existe una forma automática de hacerlo.

Para obtener información de todas las imagenes, lo hacemos con el comando:

```bash
docker image ls
```

Mientras que si sólo queremos obtener los IDs de las imagenes:

```bash
docker image ls -q
```

Por lo tanto podríamos pasarle esto al comando `docker image rm`

```bash
docker image rm $(docker image ls -q)
```

Esto nos daría un error si tenemos contenedores basados en estas imágenes **corriendo o detenidos**, por lo que tendremos que eliminarlos primero.



Utilizando una técnica similar a la vista anteriormente, eliminamos todos  los contenedores:

```bash
docker container rm -f $(docker container ls -aq)
```

> Notar que a la hora de borrar estamos usando la opción `-f` para eliminar de manera forzada a los contenedores que están corriendo.
>
> También a la hroa de listar agregamos la opción `-a` para también eliminar los contenedores detenidos.



Ahora eliminamos las imagenes:

```bash
docker image rm $(docker image ls -q)
```



Ahora podremos verificar lo realizado anteriormente con

```
docker image ls
docker ps
docker ps -a
```



> Otra forma de hacerlo con Windows es hacer click derecho en el ícono de Docker, luego ir a **Settings**, hacer click en el ícono de **Troubleshoot** y luego click en **Clean/Purge data** que eliminará imagenes, contenedores, volúmenes, etc.



# Aplicación Completa

Trabajaremos con una aplicación formada por múltiples partes frontend, backend y base de datos.

Cuando trabajamos sin Docker para correr la aplicación tenemos que seguir una serie de pasos:

* Descargar el código del frontend y del backend de GitHub
* Iniciar una ventana de la terminal para el frontend y otra para el backend, instalar las dependencias y luego iniciar los web servers de cada aplicación.
* Descargar e instalar MongoDB.         



Con Docker en cambio sólo tendremos que ejecutar un comando. Suponiendo que tenemos creado el archivo `docker-compose.yml` (que nos permitirá componer una aplicación multi-contenedor), sólo tendremos que ejecutar:

```
docker-compose up
```

En ese momento veremos los distintos layers relacionados con la instalación de MongoDB, instalación de dependencias, inicialización de webservers, ejecutará tests etc.

Contamos con un *migration script* que realiza el *populate* de algunos elementos iniciales en la base de datos.



Antes de analizar el archivo `docker-compose.yml` profundizaremos acerca de los formatos `.json` y `.yml`.



## Formato `JSON` y `YAML`

El formato `.json` es un lenguaje para representar datos de una manera humanamente lejible, como por ejemplo:

```json
{
	"name": "The Ultimate Docker Course",
	"price": 149,
	"is_published": true,
	"tags": ["software","devops"],
	"author":{
		"first_name":"Mosh",
		"last_name":"Hamedani"
	}
}
```



El formato `YML` (es posible utilizar la extensión `.yml` o`.yaml`) también nos permite representar datos pero de una manera áun más simple de leer:

> Utilizamos `---` en la parte superior
>
> Utilizamos indentación para indicar jerarquía
>
> No utilizamos comillas
>
> No utilizamos comas para separar pares key/value
>
> Para representar elementos de un array utilizamos guiones e indentación

```yaml
---
name: The Ultimate Docker Course
price: 149
is_published: true
tags: 
	- software
	- devops
author:
	first_name: Mosh
	last_name: Hamedani
```

Como podemos ver el formato `YAML` es mas simple de leer pero tiene la contra de que es un poco más lento de parsear que el formato `JSON`. Por ejemplo no sabe a priori si `149` algo es un string o un número por lo que deberá leer todo como un string y luego evaluarlo.

Los archivos `JSON` se usan habitualmente para intercambiar datos entre cliente y servidor y los `YAML` para archivos de configuración.



## Archivo `docker-compose.yml`

En primer lugar debemos setear la propiedad `version` para ello googleamos "docker compose file" podremos ver una tabla con la versión de compose y la del release de docker engine. 

```yaml
version: "3.8"
```

> Notar que **debemos ponerlo entre comillas** ya que Docker Compose espera que sea un string.



A continuación debemos definir los distintos bloques o servicios con los cuales le diremos a Docker como hacer cada una de las imágenes y cómo iniciar luego los contenedores.

```yaml
version: "3.8"
services:
	frontend:
	backend:
	database:
```

> Les hemos dado a los servicios el nombre `frontend`, `backend`, `database` pero podríamos haberles dado cualquier nombre por ejemplo `web`, `api`, `db`. 

Así como anteriormente para iniciar el contenedor ingresabamos manualmente el comando `docker run -p ... -v ... react-app` la idea es ingresar aca propiedades que nos permitan luego correr el contenedor con esas opciones.

Para cada servicio podemos crear las imagenes o descargarlas de DockerHub. Para el caso del servicio `web` y `api` queremos crearlas por lo que utilizamos la propiedad `build` y hacemos referencia a la ubicación del `Dockerfile`. Para el servicio `db` en cambio queremos descargar la imagen de DockerHub y usamos la propiedad `image`.

```yaml
version: "3.8"
services:
	web:
		build: ./frontend
	api:
		build: ./backend
	db:
		image: mongo:4.0-xenial
```

> Para `db` utilizamos` mongo:4.0-xenial` siendo `xenial` Ubuntu 16. Utilizamos una imagen de Linux ya que las de Windows son muy pesadas.



A la hora de hacer el mapeo de puertos como podemos tener mas de uno debemos utilizar la sintaxis de array:

```yaml
version: "3.8"
services:
	web:
		build: ./frontend
		ports:
			- 3000:3000
	api:
		build: ./backend
		ports:
			- 3001:3001
	db:
		image: mongo:4.0-xenial
		ports:
			- 27017:27017
```



Al poner `3000:3000` estamos mapeando el 3000 del host con el 3000 del contenedor (en ese orden).

> MongoDB por defecto escucha en el puerto `27017` y para poder utilizar un *database client* como MondoDB Compass lo mapeamos al mismo puerto del host.



Googleando ["compose version 3"](https://docs.docker.com/compose/compose-file/compose-file-v3/) podremos ver todas las propiedades válidas las mas comunes son `build`, `image`, `ports`, `volume`, `environment`, etc



Para la api necesitamos una **variable de entorno** que indique donde está la base de datos. 

Esto podemos hacerlo de dos formas:

La primera es utilizando la **sintaxis de listas** ya que podremos tener múltiples variables de entorno:

```yaml
version: "3.8"
services:
	web:
		build: ./frontend
		ports:
			- 3000:3000
	api:
		build: ./backend
		ports:
			- 3001:3001
		environment: 
			- DB_URL=mongodb://db/vidly
	db:
		image: mongo:4.0-xenial
		ports:
			- 27017:27017
```

Cuando iniciamos una aplicación con Docker Compose se crea una **Network** en la cual tendremos tres **Hosts** con nombres equivalentes a los que le hemos dado a los servicios `web`, `api` y `db`. Es por eso que a la hora de especificar el *connection string* usamos `DB_URL=mongodb://db/vidly` siendo `db` el nombre de la red y `vidly` la base de datos.

La otra forma es utilizando la **sintaxis property-value** y tiene la ventaja de que tendremos un mejor highlighting. 

```yaml
version: "3.8"
services:
	web:
		build: ./frontend
		ports:
			- 3000:3000
	api:
		build: ./backend
		ports:
			- 3001:3001
		environment: 
			DB_URL: mongodb://db/vidly
	db:
		image: mongo:4.0-xenial
		ports:
			- 27017:27017
```



No queremos que MondoDB escriba datos en el file system temporario del contenedor, es por eso que debemos utilizar un volumen.

```yaml
version: "3.8"
services:
	web:
		build: ./frontend
		ports:
			- 3000:3000
	api:
		build: ./backend
		ports:
			- 3001:3001
		environment: 
			DB_URL: mongodb://db/vidly
	db:
		image: mongo:4.0-xenial
		ports:
			- 27017:27017
		volumes:
			- vidly:/data/db
volumes:
	vidly:
```

> MongoDB utiliza como directorio default `/data/db`. Queremos que todo lo que el contenedor escriba allí se almacene en un volumen.
>
> En la parte de abajo agregamos nuevamente `vidly` pero sin valor. La idea es que debemos definir un volumen primero para luego utilizarlo en el servicio.



## Crear Imagenes

Como Docker Compose está hecho sobre Docker Engine, todo lo que hicimos anteriormente como crear imagenes, listarlas, iniciar contenedores con ellas, etc está disponible.



Si ejecutamos `docker-compose` podremos ver todos los subcomandos que podemos ejecuctar, estos impactarán sobre la aplicación como un todo es decir en múltiples contenedores.



Para obtener información sobre un subcomando en particular:

```
docker-compose build --help
```

* `--no-cache` sirve para evitar el uso de caché al crear la imagen y esto puede ser útil cuando tenemos algun inconveniente y queremos forzar un *full rebuild*

* `--pull` sirva para intentar hacer un pull de una nueva versión de la imagen.



En nuestro caso usaremos simplemente:

```
docker-compose build
```

Docker utilizará lo que pueda del caché y es por esto que si queremos forzar un *full rebuild* debemos poner la opción `--no-cache`



Luego podremos ver que tenemos tres imagenes con:

```
docker images
```

> El nombre del directorio aparecerá como prefijo del nombre de las imagenes `vidly_api`, `vidly_web`, etc.  



## Iniciar Aplicación

A la hora de iniciar una aplicación con Docker Compose debemos hacerlo con el comando:

```
docker-compose up
```

Si las imagenes están disponibles Docker las correrá dentro de contenedores, en caso contrario creará dichas imágenes previamente.



Para conocer los subcomandos disponibles

```
docker-compose
```



* `--build` para forzar un rebuild cada vez que queremos iniciar la aplicación, no tendremos que poner `docker-compose build` y `docker-compose up` sino que directamente `docker-compose up --build`

```
docker-compose up --build
```



* `-d` para trabajar en *dettached mode* es decir para iniciar los contenedores en el *background* 

```
docker-compose up -d
```



* `ps` podremos ver todos los contenedores relacionados a esta aplicación

```
docker-compose ps
```

> Este comando se diferencia de `docker ps` con el cual vemos todos los contenedores corriendo en todas las aplicaciones.



## Detener Aplicación

Para detener la aplicación y liberar recursos borrando los contenedores (las imagenes permanecerán)

```
docker-compose down
```



## Docker Networking

Cuando ejecutamos la aplicación Docker Compose crea automáticamente una red y agrega los contenedores a esa red.

Esto lo podemos comprobar de la siguiente forma:

```
docker-compose up -d
```

La primera línea que veremos será **"Creating network "vidly_default" with the default driver**



Para listar las redes disponibles:

```
docker network ls
```

> La instalación default de Docker viene con tres redes `bridge`, `host` y `none`. Además veremos a la red `vidly_default` creada por Docker Compose que tiene tres hosts o contenedores.



Tendremos los hosts `web`, `api` y `db` y pueden comunicarse entre sí utilizando su nombre.



Si queremos ver los contenedores corriendo:

```
docker ps
```

Luego iniciamos una sesión shell en el contenedor de `vidly_web` y haremos un ping a `vidly_api`

```
docker exec -it 8cg sh
```

Luego le hacemos un ping

```
ping api
```

Obtenemos **ping: permission denied (are you root?)** 

Salimos con `exit`.

Esto se debe a que desde el Dockerfile hemos establecido que queremos trabajar con el usuario `app` que no tiene permisos de ping, lo que hacemos será loguearnos como `root` user

```
docker exec -it -u root 8c6 sh
```

Ahora al iniciar la sesión de shell veremos en el prompt el `#` que da cuenta de que tenemos los privilegios mas altos y el comando `ping` nos funcionará

```
ping api
```

Cada contenedor tiene asociada una dirección IP y en este caso veremos  la dirección asociada a este a `api`.



Docker viene con un **servidor DNS** embebido que contiene los nombres e IP de los contenedores. 

Dentro de cada contenedor tenemos un componente llamado **DNS Resolver** que habla con el **servidor de DNS** para obtener la direcciones IP deseadas. En nuestro caso dentro del contenedor `web` tenemos un DNS resolver que le pregunta al servidor de DNS la dirección ip de `api` y este luego se lo da en respuesta. Luego el contenedor `web` utilizará la IP para comunicarse con `api`.



Para obtener la dirección IP del contenedor:

```
ifconfig
```

> Veremos los adaptadores de red `eth0` (dentro de este apartado veremos la IP del contenedor) y `lo`.



Cuando definimos la variable de entorno en el servicio `api`

```
DB_URL: mongodb://db/vidly
```

Estamos referenciando a `db` que es el nombre del host (o contenedor)



> Debemos tener presente que el host `db` solo está disponible dentro del entorno de Docker, es decir que si desde el host ingresamos en localhost/db no obtendremos nada sino que podremos hacerlo a través de mapeo de puertos.

```
ports:
			- 27017:27017
```

Es por eso que con Compass podremos conectarnos al contenedor teniendo como hostname **localhost** y como puerto **27017**.



## Visualizar los logs

Si queremos ver los logs de todos los contenedores de la aplicación podemos hacerlo con:

```
docker-compose logs
```

Al lado de cada mensaje podremos ver un identificador del contenedor al cual pertenece dicho log.



Para ver los posibles subcomandos

```
docker-compose logs --help
```



Si queremos ver continuamente la salida de logs en la medida que aparecen podemos hacer uso del follow con `-f` o `--follow`

```
docker-compose logs -f
```

Si queremos mostrar los timestamps de cada log podemos hacerlo con `-t` o `--timestamps`

```
docker-compose logs -t
```



Si queremos ver los logs de cada contenedor individualmente en lugar de todos juntos, primero debemos obtener el id del contenedor:

```
docker ps
```

Luego ejecutamos (y agregamos la opción `-f` si queremos salida continua)

```
docker logs 8c6 -f
```



## Publicar Cambios

Nuevamente no queremos tener que reconstruir las imágenes cada vez que cambiamos el código por lo que otra vez vamos a mapear el directorio del proyecto (como por ejemplo `backend` y `frontend` en nuestro caso) al directorio `app` dentro del contenedor. Así nos aseguramos que los cambios realizados en el directorio del host sean vistos inmediatamnete en el directorio del contenedor. 



En el `docker-compose.yml` agregamos dentro del servicio `api` la propiedad `volumes` para hacer el mapeo de volúmenes:

```yaml
version: "3.8"
services:
	web:
		build: ./frontend
		ports:
			- 3000:3000
	api:
		build: ./backend
		ports:
			- 3001:3001
		environment: 
			DB_URL: mongodb://db/vidly
		volumes:
			- ./backend:/app
	db:
		image: mongo:4.0-xenial
		ports:
			- 27017:27017
		volumes:
			- vidly:/data/db
volumes:
	vidly:
```



> Notar que estamos utilizando un **path relativo** a diferencia de antes que utilizamos un **path absoluto** cuando hicimos `docker run -v $(pwd)`
>
> También notar que estamos mapeando con la carpeta `/app` del contenedor (tendremos una en cada uno de los contenedores).

```bash
docker run -d -p 5001:3000 -v $(pwd):/app react-app
```



Si ahora ejecutamos

```
docker-compose up
```

Obtendremos un error **sh: nodemon: not found** y esto se debe a que estamos copiando los archivos pero en el host no tenemos la aplicación instalada (si la teníamos instalada en el contenedor pero ahora estamos compartiendole el código por lo que verá exactamente lo mismo que en el host) por lo que no tenemos `node_modules`. Para solucionar esto navegamos a `cd backend` e instalamos la aplicación `npm i`

En localhost:3001/api podremos acceder al backend. Si hacemos ahora algun cambio y actualizamos lo veremos actualizado.

Podemos aplicar la misma técnica para el frontend.



## Migrando la Base de Datos

Se conoce como *Database Migration* al hecho de tener la base de datos con algunos datos. Existen varias alternativas pero en nuestro caso hemos utilizado la heramienta __Migrate Mongo__ para crear un un *database migration script*. En `package.json` veremos que tenemos este paquete dentro de las `devDependencies`.

En el proyecto de `backend` dentro del directorio `migrations` almacenamos los migration scripts y dentro de `20210208213312-populate-movies` (notar que en el nombre tiene asociado un timestamp). Tenemos una función `up` para upgrading donde agregaremos elementos a la db y una `down` para downgrading donde los eliminaremos.

```js
module.exports = {
  async up(db, client) {
    await db
      .collection("movies")
      .insertMany([
        { title: "Avatar" },
        { title: "Star Wars" },
        { title: "Terminator" },
        { title: "Titanic" },
      ]);
  },

  async down(db, client) {
    await db.collection("movies").deleteMany({
      title: {
        $in: ["Avatar", "Star Wars", "Terminator", "Titanic"],
      },
    });
  },
};

```



Con el comando `migrate-mongo up` ejecutamos todos los *migration scripts* (en nuestro caso sólo tenemos uno) para cargar datos a la base de datos. Si lo ejecutamos mas de una vez no hará efecto ya que dentro de la DB tenemos una colección llamada `changelog` con los scripts que fueron ejecutados indicando su `fileName` y`appliedAt`.



En `package.json` en el apartado de scripts podremos ver el siguiente alias:

```js
"db:up": "migrate-mongo up",
```

Esto significa que podremos escribir `npm run db:up` y es lo mismo que poner `migrate-mongo up`



Queremos que se ejecute la migración de la base de datos al iniciar la aplicación y si nos fijamos en el `Dockerfile` veremos que tenemos:

``` 
CMD ["npm", "start"]
```

Así como está se ejecutará el comando `npm start`. Para lograr hacer la migración antes de esto debemos modificar el archivo `docker-compose.yml` agregando la propiedad `command` dentro del servicio `api` acompañado del comando que deseamos ejecutar:

```bash
command: migrate-mongo up && npm start
```



Por lo que el `docker-compose.yml` se vería así:

```yaml
version: "3.8"
services:
	web:
		build: ./frontend
		ports:
			- 3000:3000
	api:
		build: ./backend
		ports:
			- 3001:3001
		environment: 
			DB_URL: mongodb://db/vidly
		volumes:
			- ./backend:/app
		command: migrate-mongo up && npm start
	db:
		image: mongo:4.0-xenial
		ports:
			- 27017:27017
		volumes:
			- vidly:/data/db
volumes:
	vidly:
```



Sin embargo hay un problema con esta implementación ya que puede que el servidor de la base de datos no esté disponible al momento de ejecutar este comando (a pesar de que el contenedor esté corriendo puede que el motor de la base datos aún no esté disponible ya que suele tardar varios segundos). 

En estos casos debemos utilizar un *waiting script*. Para ello googleamos "docker wait for container" para acceder a un artículo titulado *Control startup and shutdown order in Compose* podremos encontrar las herramientas que nos permiten tener esta capacidad. Dentro de ellas utilizaremos **wait-for-it**, ingresando al repositorio podremos descargar el script `wait-for-it.sh`. Gracias a este script podremos esperar a que el motor de la db esté disponible antes de hacer algun trabajo.

Incorporamos el archivo `wait-for` (lo estamos renombrando) dentro de la raíz del proyecto y colocamos:

`command: ./wait-for db:27017 && migrate-mongo up && npm start`

> Esperará a tener tráfico en el puerto default de MongoDB que es el 27017.



```yaml
version: "3.8"
services:
	web:
		build: ./frontend
		ports:
			- 3000:3000
	api:
		build: ./backend
		ports:
			- 3001:3001
		environment: 
			DB_URL: mongodb://db/vidly
		volumes:
			- ./backend:/app
		command: ./wait-for db:27017 && migrate-mongo up && npm start
	db:
		image: mongo:4.0-xenial
		ports:
			- 27017:27017
		volumes:
			- vidly:/data/db
volumes:
	vidly:
```



El comando es bastante largo por lo que podremos utilizar otra técnica usando un  *entry point script* mediante un archivo `docker-entrypoint.sh`.

```
#!/bin/sh

echo "Waiting for MongoDB to start..."
./wait-for db:27017 

echo "Migrating the databse..."
npm run db:up 

echo "Starting the server..."
npm start 
```



Para ejecutar este archivo debemos indicar `command: ./docker-entrypoint.sh` por lo que el archivo `docker-compose.yml` nos queda:

```yaml
version: "3.8"
services:
	web:
		build: ./frontend
		ports:
			- 3000:3000
	api:
		build: ./backend
		ports:
			- 3001:3001
		environment: 
			DB_URL: mongodb://db/vidly
		volumes:
			- ./backend:/app
		command: ./docker-entrypoint.sh
	db:
		image: mongo:4.0-xenial
		ports:
			- 27017:27017
		volumes:
			- vidly:/data/db
volumes:
	vidly:
```



Queremos verificar que todo esto funcione, para ello primero ejecutamos `docker-compose down` con lo cual los contenedores se eliminan pero el volumen persiste. Para eliminarlo manualmente primero obtenemos su nombre con `docker volume ls` y luego lo borramos con `docker volume rm vidly_vidly` (nombreAplicación_nombreVolumen). Si ahora ejecutamos `docker-compose up` y vamos al endpoint que devuelve todas las películas localhost:3001/api/movies (o a Compass y revisamos esa colección) veremos que se ha hecho la database migration.



## Ejecutar Tests

En ambos proyectos (frontend y backend) tenemos una serie de tests automatizados:

* tests unitarios
* tests de integración

Podemos ejecutarlos yendo al directorio correspondiente por ejemplo`cd frontend` y ejecutando el comando `npm test`

Si bien este es el modo más rápido de obtener feedback acerca de nuestro código, también podemos ejecutar los tests dentro de un contenedor.



Debemos modificar el archivo `docker-compose.yaml` y agregar un nuevo servicio `web-tests`:

```yaml
web-tests:
	image: vidly_web
	volumes:
		_ ./frontend:/app
	command: npm test
```

> Estamos reutilizando la imagen creada por el servicio `web` por eso ponemos `image: vidly_web`
>
> No necesitamos puertos y dejamos los volumenes igual que en `web` ya que queremos que los cambios que hagamos en el código se vean reflejados en el contenedor de manera inmediata.



El archivo completo nos queda:

```yaml
version: "3.8"
services:
	web:
		build: ./frontend
		ports:
			- 3000:3000
	web-tests:
		image: vidly_web
		volumes:
			_ ./frontend:/app
		command: npm test
	api:
		build: ./backend
		ports:
			- 3001:3001
		environment: 
			DB_URL: mongodb://db/vidly
		volumes:
			- ./backend:/app
		command: ./docker-entrypoint.sh
	db:
		image: mongo:4.0-xenial
		ports:
			- 27017:27017
		volumes:
			- vidly:/data/db
volumes:
	vidly:
```



Cuando ejecutamos `docker-compose up` veremos en pantalla el resultado de los tests.

Si hacemos un cambio se ejecutarán nuevamente los tests y esto llevará algo de tiempo.

Así como definimos un servicio `web-tests` podemos definir uno `api-tests` para los tests del backend.

Este método tiene la ventaja que no tenemos que abrir distintas ventanas para correr los tests del front y del back y tenemos todo en un lugar.



# Deployment

El propósito de esta sección es tomar la aplicación con la que venimos trabajando y deployarla, es decir colocarla en la nube.



## Opciones de Deploy

A la hora de hacer un deployment podremos elegir entre hacerlo en un:

* **host único**. Esta implementación tiene la desventaja de que si el servidor está offline la aplicación no será accesible. Además en caso de crecer rápidamente y tener cientos de miles de usuarios un sólo servidor no sería capaz de manejarlos.
* **cluster** o grupo de servidores o cluster. Tienen como ventaja la alta disponibilidad y la escalabilidad. Para trabajar con clusters necesitamos herramientas de orquestación como Docker Swarm o Kubernetes

Lo aconsejable es comenzar de manera simple con un host único y luego transicionar hacia un cluster en caso de que sea necesario.



## Obtener un VPS

Para deployar la aplicación necesitamos un VPS (*virtual private server*), existen distintas alternativas, que tomando algunas de ellas ordenadas de las mas simples a las mas complejas:

* Digital Ocean
* Google Cloud Platform (GCP)
* Microsoft Azure
* Amazon Web Services (AWS)

> Para utilizar todos estos servicios es necesario una tarjeta de crédito o de débito.
