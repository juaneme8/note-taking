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

1. Mediante la descarga de un archivo `.war` dentro del apartado *Generic Java package* y para ello es necesario tener instalado Java 8. https://www.jenkins.io/doc/book/installing/war-file/

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

Se trata de una configuración centralizada en la cual tenemos una única máquina encargada de ejecutar todos los jobs

### Configuración master slave

Se trata de una configuración distribuida en la cual tenemos una máquina maestro y otras máquinas que actúan como esclavos. La motivación de este esquema surge cuando no es posible ejecutar todos los jobs simultáneamente con una única máquina. Es probable que algunos jobs deban correr en Linux, otros en Windows, etc.
