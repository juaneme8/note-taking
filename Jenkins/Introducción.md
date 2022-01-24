# Jenkins

> Basado en Curso de Platzi Automatización y Jenkins
>
> Basado en [Jenkins Tutorial for Beginners](https://youtu.be/MTm3cb7qiEo) de Thetips4you.

## Introducción

Jenkins es la herramienta **open source** de automatización más utilizada en la industria.  Jenkins nos permite automatizar las distintas fases del desarrollo building, testing, deployment logrando que los equipos puedan acelerar el proceso del desarrollo de software.

La **automatización** de procesos nos permite no sólo tener **mayor productividad** sino también **evitar los errores** ocasionados por cansancio al realizar una tarea tediosa de manera repetitiva. 

Es posible automatizar múltiples procesos deployment, ejecución de tests, verificar que un sitio esté activo.

Jenkins es una de las herramientas mas populares de CI (continuous integration). Otras opciones son Gitlab CI, CircleCI, etc

Jenkins también cuenta con una versión enterprise.



## Características

* **Portable**. Es una herramienta escrita en Java lo cual le permite ser corrido en Windows, Linux o en sistemas basados en Unix.

* Extensible mediante **plugins** (escritos en Java). 

* Tiene una **comunidad** muy grande.

* **Escala muy bien verticalmente**: puede correr muchos trabajos concurrentemente en una sóla máquina. En un punto si la máquina no da a basto es posible hacerla más grande dándole así más recursos a Jenkins y si bien esto tiene un límite nos puede llevar muy lejos.
* **Escala muy bien horizontalmente**. En caso de que una máquina no sea suficiente tendremos que escalar horizontalmente.  Jenkins nos permite trabajar con un nodo master y otros nodo esclavos que trabajan por el.
* **Actualizaciones** permanentes y releases de seguridad. Tener en cuenta que al estar almacenadas las llaves de seguridad de AWS o Digital Ocean maneja información muy importante.



> En Jenkins los **jobs** son la unidad de trabajo de Jenkins y es posible escribirlos actualmente en forma de código lo cual se conoce como **pipeline as code** y nos permite que también **la automatización sea programática**. Esto nos permitirá levantar un Jenkins nuevo migrando el trabajo de otras automatizaciones.



## DevOps

DevOps es una cultura de trabajo que busca promover la colaboración entre los equipos de development (Dev) y los de operations (Ops) usando automatización.



## CI

CI (continuous integration) es la práctica de automatizar la integración de cambios en el código de múltiples desarrolladores en un único proyecto, permitiéndoles mergear en un repositorio central donde se ejecutan tests y se realizan los builds y en caso de que no hayan inconvenientes se hace el deployment.

Se conoce como ciclo de vida de DevOps a las siguientes etapas:

* continuous development (plan + code)
* continuous testing (build + test)
* continuous deployment (release + deplot)
* continuous monitoring (operate + monitor)

A la hora de hablar de los procesos de DevOps es habitual encontrarnos con una imagen que forma un infinito con estas etapas.



## Instalación en Windows

En la documentación vemos la instalación de Windows tiene dos formas de hacerlo:

1. Mediante la descarga de un archivo `.war` dentro del apartado *Generic Java package* y para ello es necesario tener instalado Java 8 o Java 11. https://www.jenkins.io/doc/book/installing/war-file/

​	

```
java -jar jenkins.war
```


Veremos que nos creo una carpeta

```
C:\Users\user\.jenkins
```



Además nos mostrará en pantalla la password y además la ruta de un archivo donde la almacena. (No presionar Control + C para copiarlo pues se cerrará :wink: )

```
C:\Users\user\.jenkins\secrets\initialAdminPassword
```



2. Mediante la descarga de un archivo `.msi`.

Debemos tener configurado nuestro usuario con permisos de Logon as a Service, lo cual podemos lograr siguiendo [estos pasos](https://stackoverflow.com/questions/63410442/jenkins-installation-windows-10-service-logon-credentials):

>When installing a service to run under a domain user account, the account must have the right to logon as a service. This logon permission applies strictly to the local computer and must be granted in the Local Security Policy. Perform the following to edit the Local Security Policy of the computer you want to define the ‘logon as a service’ permission:
>
>1. Logon to the computer with administrative privileges.
>2. Open the ‘Administrative Tools’ and open the ‘Local Security Policy’
>3. Expand ‘Local Policy’ and click on ‘User Rights Assignment’
>4. In the right pane, right-click ‘Log on as a service’ and select properties.
>5. Click on the ‘Add User or Group…’ button to add the new user.
>6. In the ‘Select Users or Groups’ dialogue, find the user you wish to enter and click ‘OK’
>7. Click ‘OK’ in the ‘Log on as a service Properties’ to save changes.



En cualquier caso al terminar la instalación nos dirigimos a `localhost:8080` (o el puerto que hayamos elegido) y para iniciar nos pedirá la contraseña almacenada en un achivo cuya ruta nos indicará. 

* Elegimos **Install suggested plugins**
* Creamos el usuario admin.
* Obtendremos por último la URL de Jenkins, si tenemos una dirección del estilo jenkins.organization.com debemos ponerla aca.



> Si queremos trabajar con Jenkins en inglés debemos configurar ese idioma como principal en Chrome.



## Manejo de Usuarios

La forma predefinida de hacerlo es mediante Jenkins (también puede hacerse usando Plugins). Es importante que cada persona tenga su propio usuario para poder auditar qué tarea fue realizada por cada persona.



## Arquitectura de Jenkins

### Configuración master

Se trata de una **configuración centralizada** en la cual tenemos una única máquina encargada de ejecutar todos los jobs

### Configuración master slave

Se trata de una **configuración distribuida** en la cual tenemos un maestro y uno o mas esclavos. La motivación de este esquema surge cuando no es posible ejecutar todos los jobs simultáneamente con una única máquina. Esto podrá deberse a que tengamos muchas tareas o que algunos jobs deban correr en Linux, otros en Windows, etc.

El dispositivo maestro será el encargado de planificar las tareas, monitorear a los esclavos y darles instrucciones.También podría ejecutar tareas pero normalmente de esto se encargan los esclavos. Los esclavos se encargan de ejecutar las tareas y reciben comandos del maestro.

Vamos a Manage Jenkins y Manage Nodes and Slave.

Veremos el Built-in Node o master node.
Podemos hacer click en New Node, elegimos el nomber por ejemplo windows y seleccionamos Permanent Agent y luego guardamos.

En ese momento podremos elegir configurar el elemento creado:

* Labels (por ejemplo windows). Esta etiqueta nos permitirá que al crear jobs  solo se ejecuten aquellos nodos que tienen determinada etiqueta.
* Remote root directory (por ejemplo c:/temp).
* Usage
  * Only build jobs with label expressions matching this node. Si elegimos esta opción sólo queremos que se use con jobs que tengan esta etiqueta.
  * Use this nodeo as much as possible. Si elegimos esta opción cuando creamos un job el nodo maestro se fijará qué nodo esclavo está disponible y distribuirá la tarea.

Una vez guardados los cambios nos aaprecerá una cruz roja que indica que el Agente no está conectado y al entrar un cartel que dice **JNMP agent port is disabled and agents cannot connect this way**, tendremos que ir al link provisto y elegir en Agents **Random** o Fixed.

> Thetips4you tiene una [playlist](https://youtube.com/playlist?list=PLVx1qovxj-amO4WeEfpfrvcm2pg9NB5i9) detallada cómo continuar configurando esta topología.



## Freestyle Job

El primer paso es hacer click en Create a job en la pantalla de bienvenida o en New Item en la barra lateral.

Elegimos Freestyle project e ingresamos el nombre del job que creamos samplefirstjob

En la parte de Build elegimos Execute shell (si estamos trabajando en Linux) y luego `echo "this is my first project"`.

Veremos la nueva tarea presentada como en el listado y podremos seleccionarla y poner Build Now. Luego en el apartado Build History podremos ver `#1` y seleccionando este elemento elegir Console Output y ver la salida que produjo esta tarea.



En `/var/lib/jenkins` veremos una carpeta `nodes`, `plugns`, `users`, `jobs`, `workspace` (creará una carpeta automáticamente con el nombre del proyecto)



# Build Triggers

## Build periodically

Si queremos que una tarea se ejecute de manera periódica podemos hacerlo ingresando una *cron expression*.

Elegimos la tarea que queremos programar, vamos a **Configure** y en la parte de **Build Triggers** elegimos **Build periodically** y en ese campo ponemos la expresión. Por ejemplo si queremos que se ejecute cada dos minutos: `*/2 * * * *`

### Cron Expressions

Las *cron expressions*  tienen la siguiente estructura:

```
<minute> <hour> <day-of-month> <month> <day-of-week> <command>
```

* minute (0-59)
* hour (0-23)
* day of month (1-31)
* month (1-12) (también podemos poner `JAN` - `DEC`)
* day of the week (0 - 6) (también podemos poner `SUN` - `SAT`)



> En la página https://crontab.guru/ podemos simular las distintas expresiones y ver cuando se ejecutarán.



* Si queremos que algo se ejecute en todos los minutos, todas las horas, todos los días del mes, todos los meses y todos los días de la semana tendremos: `* * * * *`
* Si queremos que algo se ejecute cada 5 minutos: `5 * * * *`

* Si queremos ejecutar algo a las 12 del mediodía: `0 12 * * *`
* Si queremos ejecutar algo de 13hs a 13:05hs una vez por minuto todos los días: `0-5 13 * * * `
* Si queremos que algo se ejecute de 13:15 y a las 13:45 todos los jueves de junio: `15,45 13 * 6 Tue (notar que podemos poner `TUE` con cualquier capitalización)
* Si queremos que algo se ejecute el día 15 de todos los meses a las 9:30hs: `30 9 15 * *`
* Si queremos que algo se ejecute cada 2 minutos `*/2 * * * *`



## Trigger builds remotely

Es posible disparar la ejecución de una tarea de manera remota. En el campo donde dice Authentication Token podemos poner por ejemplo `sampleproject` y de esta manera incresando a `JENKINS_URL/job/samplefirstjob/build?token=TOKEN_NAME` generaremos la ejecución de esa tarea. Esto podemos hacerlo desde otra máquina o incluso en un script. 



## Parametrized Job

En la parte de configuración **General** nos encontramos la opción **This project is parametrized** al tildarlo podremos luego elegir **Add Parameter** y elegir entre los distintos tipos (Boolean, Choice, Credentials, File, Multi-line string, Password, Run, String). Luego le daremos un **Name** y **Default Value**. Al guardar veremos que donde decía **Build** ahora dice **Build with Parameters**. Luego podríamos referenciar este valor en la parte del shell command que ejecutamos.



> En caso de elegir el tipo de parámetro Choise a la hora de hacer el **Build with Parameters** tendremos un select entre las opciones ingresadas.



## Webhook and Poll SCM

El propósito de estos dos triggers es que cada vez que haya un commit nuevo en el repositorio dispare la ejecución de un job. 

Para configurarlo nuevamente vamos al job y hacemos click en **Configure**.

En el apartado **Source Code Management** elegimos **Git** y debemos colocar el link al repositorio. 

> Debemos tener git instalado en esa máquina pues de lo contrario obtendremos un mensaje de error.
>
> El repositorio por el momento puede ser un `README.md` y un `project.sh` con un `echo 'Hola Mundo'`
>
> Si el repositorio es privado tendremos que proporcionar credenciales.



En el apartado **Branches to build** por defecto dice `*/master` pero tendremos que poner `*/main` o el que corresponda.



Finalmente en **Build Triggers** podemos elegir si queremos **Poll SCM** o **GitHub hook trigger for GITScm polling**.



### Poll SCM

Poll SCM es un modo de trabajo que cada cierto tiempo chequea en el repositorio si se realizaron cambios (nuevos commits) y en caso afirmativo ejecuta la tarea. 

Debemos ingresar la cron expression por ejemplo si queremos chequearlo cada dos minutos `*/2 * * * *`

Para ejecutar el archivo `project.sh` disponible en el repositorio debemos en el apartado **Build ** hacer click en **Add build step** y elegir **Execute shell** para luego ingresar el siguiente comando:

```
chmod 774 ./project.sh
./project.sh
```

> Con el primer comando `chmod` nos aseguramos no obtener un error de acceso al intentar ejecutar el script.
>
> 774 significa:
>
>  (U)ser / owner can read, can write and can execute. (G)roup can read, can write and can execute. (O)thers can read, can't write and can't execute.

Veremos en `/var/lib/jenkins/workspace/samplefirstjob` que pasados dos minutos clonará el repositorio en esa ubicación.



### GitHub hook trigger for GITScm polling

Como tenemos la instalación standard de plugins veremos esta opción, pero si quisieramos hacer lo mismo con GitLab deberíamos instalar dicho plugin.

Elegimos la opción **GitHub hook trigger for GITScm polling** y queremos que cada vez que creemos un nuevo commit se produzca un disparo de esa tarea. Debemos configurar esto en el proyecto de GitHub, yendo a **Settings** (del proyecto), **Webhooks** y en **Payload URL** poner por ejemplo `http://localhost:8080//github-webhook/`. 

Luego podremos especificar qué evento queremos que dispare pro defecto tendremos **Just the push event** pero puede ser personalizado para otros eventos si así lo deseáramos. 

Finalmente hacemos click en **Add webhook**. Sin embargo, como la IP ingresada es interna no nos funcionará y obtendremos un error de *failed to connect to the host*. Es por eso que si tenemos una ip interna debemos utilizar alguna herramienta como ngrok que nos entrega una URL que actúa como tunel a neustro servidor en localhost.



### Ngrok

Debemos ir a ngrok.com y descargar el instalador (o copiar el comando de instalación en el caso de que estemos utilizando Linux).

Es necesario tener una cuenta y obtendremos un token que nos permitirá autenticarnos desde la terminal.

```
cd /usr/local/bin
./ngrok http 8080
```

A partir de ese momento obtendremos una URL que nos permitirá cargarlo en GitHub para lograr trabajar con webhooks que nos quedará algo así `http://...ngrok.io/github-webhook` y ahora veremos que nos aparece con un tilde verde indicando que la conexión fue exitosa.



## Jenkins Maven Integration

Maven es una heramienta utilizada para cuando queremos realizar proyectos en Java.



## Creación de Usuarios

## Acceso basado en roles

## Notificaciones por Email

Las notificaciones por email nos permiten obtener información acerca de cuando falla la ejecución de una tarea (*build failure*).



## Pipeline

> Notas de CodeWithNana

Se conoce como Pipeline a un conjunto de plugins



El `Jenkinsfile` es un archivo que nos permite que en lugar de tener que crear Jobs utilizando la interfaz de usuario (Jenkins GUI) podamos hacerlo utilizando un archivo en lo que se conoce como **Pipeline as Code**. Esto forma parte del concepto *infrastructure as a code*.

A continuación motraremos la sintaxis declarativa del Pipeline presentando el `Jenkinsfile` más básico que aún sin hacer nada tiene los siguientes elementos requeridos.

```
pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                
            }
        }
    }
}
```

> Otra forma de escribir el pipeline es utilizando una sintaxis de Scripts (groovy) de hecho antes era la única forma disponible. Este método no tiene una estructura definida y es muy flexible y poderoso pero tiene una curva de aprendizaje mas difícil.



Veremos que el `Jenkinsfile` tiene los siguientes campos requeridos:

* `pipeline` debe ser el nivel superior.
* `agent` indicamos dónde queremos ejecutar. Con `any` indicamos que este build debe ejecutarse en cualquier agente disponible de Jenkins. Podría ser por ejemplo `node`. 
* `stages` es donde sucede el trabajo tiene dentro los distintos `stage`.  
* `stage` normalmente tendremos uno de `stage('build')`, `stage('test')` y `stage('deploy')`. Aunque también podríamos tener uno de `checkout` al inicio y uno de `cleanup` al final. Cada `stage` tiene dentro los `steps`.
* `steps` contiene el script que ejecuta algún comando, por ejemplo `sh 'npm install'`.



Creamos el siguiente `Jenkinsfile` en la rama `dev` de nuestro proyecto.

```
pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                echo 'building the application'
            }
        }
         stage('testing') {
            steps {
                echo 'testing the application'
            }
        }
         stage('deploying') {
            steps {
                echo 'deploying the application'
            }
        }
    }
}
```



Para utilizar este `Jenkinsfile` en un Jenkins Pipeline debemos ir a **create new Jobs**, elegimos la opción **Multibranch pipeline** y le damos el nombre **my-app-pipeline**. 

Luego en **Branch Sources** clickeamos **Add source**, indicamos el repositorio y las credenciales.  Si bien podría filtrar las ramas por una expresión regular, si dejamos la opción por defecto **Discover branches**  veremos que analizará todas las ramas y sólo se produce el build solo en aquellos branches que tengan un `Jenkinsfile`. 

Este comportamiento se debe a que en **Build Configuration** por defecto tenemos la opción **by Jenkinsfile** en el apartado **Mode** y en **Script Path** indicamos que el archivo se llamará `Jenkisfile` (en el root directory).



Podremos ver para cada `stage` el log de salida que produjo.



## Atributo `post`

Luego de los `stages` (después de la llave de cierre) podemos tener un atributo `post` que indica la lógica o scripts a ejecutar luego de los stages, bajo distintas condiciones `always` (se ejecutará bajo cualquier condición por ejemplo un mail al equipo informando el resultado), `success`, `failure`

```
pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                echo 'building the application'
            }
        }
         stage('testing') {
            steps {
                echo 'testing the application'
            }
        }
         stage('deploying') {
            steps {
                echo 'deploying the application'
            }
        }
    }
    post{
    	always:{
    	
    	}
    	sucess:{
    	
    	}
    	failure:{
    	
    	}
    }
}
```



## Condicionales en el `stage`

En ocasiones vamos a querer que el `stage` ejecute en caso de que se reúnan ciertas condiciones, por ejemplo si sólo queremos ejecutar el stage de test si estamos haciendo el build del branch `dev` (y no queremos que esto suceda para `feature` o `build`).

```
pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                echo 'building the application'
            }
        }
         stage('testing') {
         	when{
         		expression{
         			BRANCH_NAME == 'dev'
         		}
         	}
            steps {
                echo 'testing the application'
            }
        }
         stage('deploying') {
            steps {
                echo 'deploying the application'
            }
        }
    }
}
```

> La variable de entorno `BRANCH_NAME` (también podemos obtenerla como `env.BRANCH_NAME`) es provista por Jenkins.
>
> Los `steps` sólo se ejecutarán si la expresión booleana provista es verdadera.
>
> Es posible colocar expresiones mas complejas como por ejemplo con OR `BRANCH_NAME == 'dev' || BRANCH_NAME=='master'` y AND `&&`.



## Variables de Entorno

Jenkins proporciona una serie de variables de entorno que podemos utilizar en nuestro `Jenkinsfile`. De la misma manera que usamos `BRANCH_NAME` podemos utilizar muchas otras. Por ejemplo el número de build para utilizar en el versionado.

En `localhost:8080/env-vars.html` podemos ver el listado completo y su explicación.



Además de las variables de entorno provistas por Jenkins podemos definir las nuestras propias.
