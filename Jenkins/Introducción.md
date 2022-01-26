# Jenkins

> Curso de Platzi Automatización y Jenkins (primeros videos).
>
> [Jenkins Tutorial for Beginners](https://youtu.be/MTm3cb7qiEo) de Thetips4you.
>
> [Complete Jenkins Pipeline Tutorial](https://youtu.be/7KCS70sCoK0) by TechWorld with Nana
>
> [Run Jenkins in Docker Container](https://youtu.be/pMO26j2OUME) by TechWorld with Nana

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



# Tipos de Proyectos en Jenkins

* Freestyle 
* Pipeline
* Multibranch Pipeline



## Freestyle Project

Es para tareas simples y únicas por ejemplo si queremos ejecutar tests.

El primer paso es hacer click en **Create a job** en la pantalla de bienvenida o en **New Item** en la barra lateral.

Elegimos **Freestyle project** e ingresamos el nombre del job que creamos samplefirstjob.

En la parte de Build elegimos Execute shell (si estamos trabajando en Linux) y luego `echo "this is my first project"`.

Veremos la nueva tarea presentada como en el listado y podremos seleccionarla y poner Build Now. Luego en el apartado **Build History** podremos ver `#1` y seleccionando este elemento elegir Console Output y ver la salida que produjo esta tarea.



> En `/var/lib/jenkins` veremos una carpeta `nodes`, `plugns`, `users`, `jobs`, `workspace` (creará una carpeta automáticamente con el nombre del proyecto)



## Pipeline Project

Nos permite realizar con ellos el flujo de trabajo completo test, build, package, deploy.

Antes de la existencia de este tipo de proyectos debíamos encadenar distintos freestyle projects para obtener el mismo resultado. Es para cuando trabajamos con un único branch.



## Mutibranch Pipeline

Como su nombre lo indica es para cuando trabajamos con múltiples ramas.



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



Finalmente en **Build Triggers** podemos elegir si queremos **Poll SCM** que preguntará por cambios cada cierto tiempo o **GitHub hook trigger for GITScm polling** con el que el sistema de control notifica a Jenkins al tener cambios.

Se trata de **métodos complementarios** y podemos pensar que uno actúa como backup del otro. En caso de que GitHub emita una notificación y Jenkins esté caído no se verá notificado de los cambios y en ese caso el tener un chequeo periódico (cada 1 hora por ejemplo para no sobrecargar a Jenkins) nos haría no perder esos cambios y generar el build.



### Poll SCM

Poll SCM es un modo de trabajo que cada intervalos de tiempo regulares chequea en el repositorio si se realizaron cambios (nuevos commits) y en caso afirmativo ejecuta la tarea. 

Debemos ingresar la cron expression por ejemplo si queremos chequearlo cada dos minutos `*/2 * * * *`

>También podemos hacerlo en **Scan Multibranch Pipeline Triggers** eligiendo allí el intervalo.

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

Esta opción es **más eficiente** y consiste en que el sistema de control de versiones notifique a Jenkins ante un nuevo commit.

Como tenemos la instalación standard de plugins veremos esta opción, pero si quisieramos hacer lo mismo con GitLab deberíamos instalar dicho plugin.

Elegimos la opción **GitHub hook trigger for GITScm polling** y queremos que cada vez que creemos un nuevo commit se produzca un disparo de esa tarea. Debemos configurar esto en el proyecto de GitHub, yendo a **Settings** (del proyecto), **Webhooks** y en **Payload URL** poner por ejemplo `http://localhost:8080//github-webhook/`. 

Luego podremos especificar qué evento queremos que dispare pro defecto tendremos **Just the push event** pero puede ser personalizado para otros eventos si así lo deseáramos. 

Finalmente hacemos click en **Add webhook**. Sin embargo, como la IP ingresada es interna no nos funcionará y obtendremos un error de *failed to connect to the host*. Es por eso que si tenemos una ip interna debemos utilizar alguna herramienta como ngrok que nos entrega una URL que actúa como tunel a neustro servidor en localhost.



#### Ngrok

Debemos ir a ngrok.com y descargar el instalador (o copiar el comando de instalación en el caso de que estemos utilizando Linux).

Es necesario tener una cuenta y obtendremos un token que nos permitirá autenticarnos desde la terminal.

```
cd /usr/local/bin
./ngrok http 8080
```

A partir de ese momento obtendremos una URL que nos permitirá cargarlo en GitHub para lograr trabajar con webhooks que nos quedará algo así `http://...ngrok.io/github-webhook` y ahora veremos que nos aparece con un tilde verde indicando que la conexión fue exitosa.



## Jenkins Maven Integration

Maven es una heramienta utilizada para cuando queremos realizar proyectos en Java.



# Creación de Usuarios

# Acceso basado en roles

# Notificaciones por Email

Las notificaciones por email nos permiten obtener información acerca de cuando falla la ejecución de una tarea (*build failure*).

# Pipeline

Se conoce como Pipeline a un conjunto de plugins

> AMPLIAR ESTOS 4 TITULOS CON VIDEO [Jenkins Tutorial for Beginners](https://youtu.be/MTm3cb7qiEo) de Thetips4you.



## Jenkinsfile

El `Jenkinsfile` es un archivo que nos permite que en lugar de tener que crear Jobs utilizando la interfaz de usuario (Jenkins GUI) podamos hacerlo utilizando un archivo en lo que se conoce como **Pipeline as Code**. Esto forma parte del concepto *infrastructure as a code*.

A continuación motraremos la **sintaxis declarativa** del `Jenkinsfile` que tiene una estructura predefinida y algunos elementos requeridos.

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
>
> ```
> node{
> 	//groovy script
> }
> ```
>
> 



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



# Multibranch Pipeline

Para utilizar este `Jenkinsfile` en un Jenkins Pipeline debemos ir a **create new Jobs**, elegimos la opción **Multibranch pipeline** y le damos el nombre **my-app-pipeline**. 



## Branch Sources

Luego en **Branch Sources** clickeamos **Add source**, indicamos el repositorio (con la dirección https con la que lo clonaríamos) y las credenciales.  

Es posible filtrar las ramas por una expresión regular, mientras si dejamos la opción por defecto **Discover branches**  cuando guardemos analizará todas las ramas y sólo producirá el build en los branches que tengan un `Jenkinsfile`. 

Esto es así dado que  en **Build Configuration** por defecto tenemos la opción **by Jenkinsfile** en el apartado **Mode** y en **Script Path** indicamos que el archivo se llamará `Jenkisfile` (en el root directory).



> Filter by Name

Es posible filtrar por nombre de la rama por defecto la expresión regular será `.*` que matcheará con todas las ramas y si queremos `dev`, `master` o `feature` ponemos `^dev|master|feature.*$` 

> Notar que vamos a matchear con`feature-one` pero `bug-one` será ignorada.



> Scan Multibranch Pipeline

Cuando hagamos un commit debemos hacer click en **Scan Multibranch Pipeline Now** para hacer Build con el `Jenkinsfile` actualizado con esos últimos cambios.



> Replay

Luego en el panel **Build History** podremos acceder a cada uno de los builds y es posible hacer un **Replay** cambiando el `Jenkinsfile` de modo de correr esos cambios sin la necesidad de hacer un nuevo commit.

Por ejemplo podríamos practicar una groovy expression:

```groovy
stage("build"){
    steps{
        echo 'building the application'
        script{
            def test = 2 + 2 > 3 ? 'cool' : 'not cool'
            echo test
        }
    }
}
```



> Restart from Stage

Es posible reiniciar desde un stage en particular. Si en el build history elegimos un build en particular podremos iniciar en un stage en particular a partir de un selector tipo drop-down.



Podremos ver para cada `stage` el log de salida que produjo y que nos ha agregado automáticamente un stage llamado Checkout SCM.



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



## Atributo `environment`

Además de las provistas por Jenkins podemos definir nuestras propias  variables de entorno. Lo hacemos dentro del atributo `environment` y luego las variables definidas las accedemos con `${VARIABLE}`

```
pipeline {
    agent any
    environment{
    	NEW_VERSION = '1.3.0'
    }
    stages {
        stage('build') {
            steps {
                echo 'building the application'
                echo "building version ${NEW_VERSION}"
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



> Notar que debemos utilizar comillas dobles para trabajar con variables de entorno, esto tiene que ver con la sintaxis de Groovy. Sin embargo, si trabajamos con strings podemos utilizar tanto comillas simpels como dobles.
>
> Normalmente el número de versión lo extraeríamos del código pero a los fines prácticos lo definimos manualmente.



## Credenciales en Jenkinsfile

Otro ejemplo habitual del uso de variables de entorno es cuando tenemos que proporcionar credenciales, por ejemplo a la hora de deployar la nueva aplicación.

Las credenciales las vamos a definir en Jenkins GUI gracias al plugin **Credentials** y luego podremos accederlas de la siguiente forma:

```
environment{
    	SERVER_CREDENTIALS = credentials(credentialId)
}
```

Tendremos que tener instalado un segundo plugin llamado **Credentials Binding**.



A la hora de agregar nuevas credenciales por ejemplo con scope global, del tipo **Username with password** nos preguntará username, password, id ( siendo este el `credentialId` que le pasamos a `credentials()`).



Luego utilizaremos la variable de entorno de la forma vista anteriormente `${SERVER_CREDENTIALS}`



Si sólo necesitamos las credenciales en un único stage en lugar de definir las variables en un bloque `environment` podemos hacerlo dentro de ese stage:



```
stage("deploy"){
	steps{
		echo 'deploying the application'
		withCredentials([
			usernamePassword(credentials:credentialId, usernameVariable: USER, passwordVariable: PWD)
		]){
			sh 'some script ${USER} ${PWD}'
		}
	}
}
```

> Notar que usamos la sintaxis de objetos en Groovy. Con `usernamePassword()` obtenemos username y password de esas credenciales. Esto se debe a que las credenciales son de tipo **Username with password**.
>
> Esto nos permite guardar en variables el usuario y la contraseña,  en `USER` y `PWD` respectivamente para luego utilizarlos dentro del bloque. 



## Atributo `tools`

El atributo `tools` proporciona herramientas para hacer el build de nuestros proyectos.

```
tools{
	maven Maven
}
```

También podríamos poner `gradle` o `jdk` mientras que si queremos utilizar npm o yarn debemos hacerlo de otra manera como se ve en [este video](https://www.youtube.com/watch?v=L9Ite-1pEU8).



Debemos tener la herramienta instalada en Jenkins para ello debemos ir a **Manage Jenkins** luego **Global Tool Configuration** veremos que por ejemplo Maven ya viene pre-instalado y pre-configurado. También veremos apartados para NodeJS, Gradle, Maven, Docker, etc. 



> `maven Maven` el segundo `Maven` hace referencia al nombre que tenemos en esa instalación.



Esto nos permitirá tener disponibles comandos Maven como `sh 'mvn install'` como podemos ver a continuación.

```
pipeline {
    agent any
    tools{
    	maven Maven
    }
    stages {
        stage('build') {
            steps {
                echo 'building the application'
               	sh 'mvn install'
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



## Directiva `parameters`

Los parámetros son configuraciones externas que nos permiten cambiar el comportamiento. Un posible uso sería para seleccionar la versión que queremos deployar.

```
parameters{
	string(name:'VERSION', defaultValue:'', description: 'version to deploy on prod')
}
```

También podríamos tener otros tipos de parámetros:

* ```
  choice(name:'VERSION', choices:['1.1.0','1.2.0','1.3.0'], description:'')
  ```

* ```
  booleanParam(name:'executeTests', defaultValue:true, description:'')
  ```

> Podremos escribirlos en caps o camelcase.



Estos parámetros podrán ser utilizados en cualquier `stage`, accedemos a ellos como `params.parameterName` y podremos utilizarlos en expresiones como vemos a continuación:

```
stage("test"){
	when{
		expression{
			params.executeTests
		}
		step{
			echo 'deploying the application ${params.VERSION}'
		}
	}
}
```

> También podríamos haber puesto `params.executeTests == true`

A partir de ahora veremos que en lugar de **Build** tenemos **Build with Parameters** y nos dará a elegir los valores de estos parámetros antes de hacer click en Build.



## Groovy Script Externo

En ocasiones puede que el Jenkinsfile tenga muchos stages (como build del frontend, correr tests, build del backend, build docker image, push al registry) tareas que hagan que crezca bastante en cantidad de lógica que maneja. Podemos crear groovy scripts con la lógica en archivos independientes que luego importamos al `Jenkinsfile`.

Hasta ahora en los steps hicimos cosas muy simples con `echo ''` pero es posible utilizar groovy scripts para crear variables, funciones, etc.

```
stage("build"){
	steps{
		script{
			def var =
		}
		echo 'building the application'
	}
}
```

Creamos un archivo `script.groovy` en el cual definimos tres métodos que llamaremos desde el `Jenkinsfile`

```
def buildApp() {
    echo 'building the application...'
} 

def testApp() {
    echo 'testing the application...'
} 

def deployApp() {
    echo 'deplying the application...'
    echo "deploying version ${params.VERSION}"
} 

return this
```

> Es importante que termine con `return this`
>
> Podemos acceder a todas las variables de entorno (las provistas por Jenkins y las definidas por nosotros) y también a los parámetros.



Luego el `Jenkinsfile` nos queda de esta manera

```
def gv

pipeline {
    agent any
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
    }
    stages {
        stage("init") {
            steps {
                script {
                   gv = load "script.groovy" 
                }
            }
        }
        stage("build") {
            steps {
                script {
                    gv.buildApp()
                }
            }
        }
        stage("test") {
            when {
                expression {
                    params.executeTests
                }
            }
            steps {
                script {
                    gv.testApp()
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }
    }   
}
```

Con `def gv` definimos a `gv` como global para poder utilizarlo en todas partes.

Con `gv = load "script.groovy" ` importamos el script.



# Jenkins en Contenedor Docker

Nos dirigimos a DockerHub y si buscamos la imagen oficial de Jenkins veremos que esta está deprecada y nos indican que debemos usar `jenkins/jenkins`.

La [documentación](https://github.com/jenkinsci/docker/blob/master/README.md) nos explica cómo debemos ejecutar esta imagen:

```
docker run 
-p 8080:8080 
-p 50000:50000 
-v jenkins_home:/var/jenkins_home 
-d 
jenkins/jenkins:lts
```

> `-p 8080:8080` Exponemos el puerto 8080 ya que Jenkins por defecto corre en ese puerto.
>
> `-p 50000:50000` Exponemos el puerto 50000 para posibilitar la comunicación master/slave. Nuestro Jenkins será capaz de encontrar slaves en caso de que los haya.
>
> `-d` dettached mode, para ejecutar el contenedor en el background.
>
> `-v jenkins_home:/var/jenkins_home` para la persistencia de datos creamos un named volume. Si la carpeta `jenkins_home` no existe en el host la creará. Mientras que `/var/jenkins_home` existe en el contenedor. Esto es importante dado que toda la información de Jenkins sobre los builds, users, plugins, estará almacenada allí.



Una vez ejecutado el contenedor obtendremos su id con `docker ps | grep jenkins` y suponiendo que es `1df` con `docker logs 1df` vemos la password inicial.

Los pasos siguientes son idénticos a los explicados con la instalación en Windows. 



En red corporativa (probar cuando se solucionen problemas de red)

```
docker run 
-p 8080:8080 
-p 50000:50000 
-v jenkins_home:/var/jenkins_home 
-d 
--env HTTP_PROXY="http://172.30.221.240:8080"
--env HTTPS_PROXY="https://172.30.221.240:8080"
jenkins/jenkins:lts
```



# Credenciales

Las credenciales nos permitirán entre otras cosas conectarnos a un repositorio privado y acceder al código.

En el menú lateral vemos la opción **Credentials** (incorporada gracias a un plugin) que nos permite manejar las credenciales de manera central y luego referenciarlas en distintas partes.



### Credential Type

* **Username with password** es el más común.
* SSH Username with private key
* etc

Dependiendo de los plugins instalados puede que veamos otros tipos de credenciales.

### Credential Scopes

* System: solo disponibles para Jenkins server (no serán accesibles por Jenkins los jobs pipelines)

* Global: accesible en todas partes.

* Project: limitado al proyecto. Accedemos a ellas si entramos al proyecto y luego al menu lateral credenciales (a diferencia de antes que lo hacíamos desde la pantalla principal o **Manage Jenkins** y **Manage Credentials**). Solo disponible en multibranch pipeline.

  

> Si entramos al proyecto luego de crear estos tres tipos de credenciales comprobaremos que las credenciales de tipo system no son accesibles en el.



Por defecto veremos que tenemos un store `Jenkins` y un domain `(global)`. Jacemos click en Global y **Add Credentials**.

Luego completamos el username, password, id (que usaremos para referenciar las credenciales en el Jenkinsfile) y descripción.



2 min de parte 2/4 https://youtu.be/tuxO7ZXplRE?t=163



# Telegram Bot

El plugin Telegram Bot nos permitirá enviar mensajes desde el `Jenkinsfile`. Para instalarlo debemos ir a **Manage Jenkins**, **Manage Plugin** y en **Available** buscar **Telegram Bot** seleccionarl.



> Analizar qué conviene hacer a futuro

**Click en Install without restart**. 
Comenzará la instalación y luego tildamos *Restart Jenkins when installation is complete and no jobs are running*.



## Configuración

En Dashboard vamos a **Manage Jenkins** y luego a **Configure System**
