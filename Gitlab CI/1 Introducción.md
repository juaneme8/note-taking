# GitLab CI

https://www.youtube.com/watch?v=Jav4vbUrqII

https://youtu.be/mnYbOrj-hLY

## Introducción

GitLab CI es un **motor de automatización** para el proceso de desarrollo de software. Cuenta con herramientas que facilitan la aplicación de prácticas de DevOps (continuous integration y continuous delivery/deployment). Esto nos permitirá realizar proceso de build, test, deploy usando Pipelines de manera similar a como lo haríamos con Jenkins.

**La herramienta se llama GitLab CI y nos permite hacer CI/CD Pipelines**.

# Pipeline

Un Pipeline de GitLab es un archivo YAML llamado `.gitlab-ci.yml` que vive en la raíz del proyecto.

Podemos pensar al Pipeline como una serie de instrucciones a ejecutar. Está formado por dos partes los **jobs** o tareas que deben ser realizadas y los **stages** que determinan el orden en que se harán estas tareas. 

El programa que ejecucta los jobs en un Pipeline de GitLab se llama **Gitlab Runner**. Es un programa aparte de la aplicación GitLab que puede correr en la máquina local o en un contenedor Docker. Es similar al concepto de Jenkins Agent. Durante la ejecución del pipeline, GitLab asigna a Gitlab Runners disponibles los pipeline jobs.

## Creación Pipeline

Podemos crear un Pipeline de diferentes maneras:

* Creamos un nuevo proyecto y dentro de él un archivo `.gitlab-ci.yml`.
* Ir a **CI/CD** luego a **Pipelines** y como no tenemos el archivo `.gitlab-ci.yml` nos permitirá buscar un template.
* Ir a **CI/CD** y luego a **Editor**. Esta opción tiene algunas ventajas:
  * Mientras escribimos veremos el mensaje **This GitLab CI configuration is valid**, mientras que en el primer paso es necesario hacer el commit antes.
  * Contamos con la pestaña Visualize que nos muestra una representación gráfica del pipeline.

> Es posible instalar la extensión GitLab Workflow que también realiza una validación de la sintaxis.



En este primer pipeline tendremos dos stages un *build stage* donde creamos un archivo y un *test stage* donde verificamos que haya sido creado correctamente.

```
stages:
	- build
	- test

build-job:
	stage: build
	script:
		- echo "Building"
		- mkdir build
		- touch build/info.txt
	artifacts:
		paths:
			- build/

test-job:
	stage: test
	script:
		- echo "Testing"
		- test -f "build/info.txt"
```

La separación la realizamos con `TAB`.

Con `stages` definimos los stages y el orden en que serán ejecutados, es decir que ejecutará primero `build` y luego `test`.

Luego definimos los pipeline jobs como `build-job` y lo vinculamos con un stage colocando `stage: build`. Luego con el keyword `script` indicamos los comandos que va a ejecutar el GitLab Runner asignado a este job.

Es posible tener mas de un job asociado al mismo stage y estos se ejecutarán en paralelo.

### Artifacts

Cada job corre en un entorno limpio, para que los archivos creados en el primer stage estén disponibles  en el segundo, debemos crear lo que se conoce como **artifacts**. De esta manera le informamos al job siguiente aguas abajo que antes de ejecutar los comandos acceda a ellos y los descargue. Esto contrasta con lo que ocurre en Jenkins donde tenemos un entorno compartido y cada job puede acceder a los archivos del anterior.

> Si vamos a **CI/CD** y luego a **Pipelines** haciendo click en los tres puntos verticales podremos acceder a los archivos de los artifacts y descargarlos.

### Estado Pipeline

Si vamos a ver el repositorio en la parte superior veremos un ícono  de un reloj azul azul indicando que el pipeline ha iniciado.

En el apartado **CI/CD** y luego **Pipelines** podremos encontrar información sobre todos los Pipelines. 
Si vamos al output del segundo job veremos **Downloading artifacts for build** de acuerdo a lo dicho anteriormente.



### Herramientas

En ocasiones vamos a necesitar tener instaladas dependencias en el entorno en que se ejecuta el job. Por ejemplo si tuviéramos que compilar un código en Java con JDK en Jenkins instalaríamos las tools necesarias en el host que está corriendo el Pipeline de Jenkins.

Con GitLab en cambio vamos a trabajar con imágenes Docker que tengan estas herramientas y lo haremos con el keyword `image`.

```
image: python
stages:
	- build
	- test
	- deploy

(...)

deploy-job:
	stage:deploy
	script:
		- echo "Deploying..."
		- python3 --version
		- echo "Successfully deployed..."
```

> Podremos referenciar una imagen de Docker propia o una proveniente de un container registry.



### Variables de Entorno

En ocasiones vamos a querer autenticarnos con servicios externos durante la ejecución del pipeline o simplemente queremos que uno de los jobs acceda a un dato. En estos casos podemos trabaar con variable de entorno. Esto lo hacemos con el keyword `variables`

```
image: python

variables:
	USERNAME: juaneme8

stages:
	- build
	- test
	- deploy

build-job:
	stage:build
	script:
		- echo "Building..."
		
test-job:
	stage:build
	script:
		- echo "Testing..."
		
deploy-job:
	stage:deploy
	script:
		- echo "Deploying..."
		- python3 --version
		- echo "Successfully deployed..."
		- echo "Credentials $USERNAME, $PASSWORD"
```

Cuando tenemos que trabajar con datos sensibles como passwords o tokens no los ponemos en texto plano ya que de ese modo formarían parte del sistema de control de versiones también en texto plano y alguien podría acceder a ellos. Lo que hacemos en cambio es ir a **Settings** y luego **CI/CD**. En ese menu vamos a **Variables** y definimos por ejemplo la variable `PASSWORD` y colocamos su valor. Además configuramos **Protect variable** (solo accesible por los branches protegidos) y **Mask variable** (oculto en los jobs logs, esto significa que si entramos a un output de un job como el anterior no veremos el valor sino `[MASKED]`.

Además es posible acceder a variables de entorno del sistema como `$CI_PROJECT-DIR`. Es más podríamos haber puesto 



## Runners

Como dijimos anteriormente los runners son procesos que ejecutan los jobs.

En **Settings** y luego **CI/CD** nos encontramos el apartado **Runners** podremos ver que tenemos *specific runners* y *shared runners* (hosteados por GitLab, 400 CI minutos por mes para proyectos privados). También es posible utilizar nuestros propios recursos de cómputo para no tener esta limitación, descargando el GitLab runner en nuestra máquina o utilizando un contenedor. En ese caso lo registraríamos como *specific runner*.



## Caching

Utilizando el keyword `cache` es posible cachear archivos que GitLab descarga de modo que no tenga que volver a descargarlos, los pipelines siguientes podrán utilizar el caché.