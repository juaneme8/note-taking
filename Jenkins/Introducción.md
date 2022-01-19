# Jenkins

> Basado en Curso de Platzi Automatización y Jenkins



Jenkins es la herramienta open source de automatización más utilizada en la industria. 

* Portable. Es una herramienta escrita en Java lo cual le permite ser corrido en Windows, Linux o en sistemas basados en Unix.

* Extensible mediante plugins (escritos en Java). 

* Tiene una comunidad muy grande.

* Escala muy bien verticalmente: puede correr muchos trabajos concurrentemente en una sóla máquina. En un punto si la máquina no da a basto es posible hacerla más grande dándole así más recursos a Jenkins y si bien esto tiene un límite nos puede llevar muy lejos.
* Escala muy bien horizontalmente. En caso de que una máquina no sea suficiente tendremos que escalar horizontalmente.  Jenkins nos permite trabajar con un nodo master y otros nodo esclavos que trabajan por el.
* Actualizaciones permanentes y releases de seguridad. Tener en cuenta que al estar almacenadas las llaves de seguridad de AWS o Digital Ocean maneja información muy importante.

La automatización de procesos nos permite no sólo tener **mayor productividad** sino también **evitar los erores** ocasionados por cansancio al realizar una tarea tediosa de manera repetitiva. 

Es posible automatizar múltiples procesos deployment, ejecución de tests, verificar que un sitio esté activo.



Los **jobs** son la unidad de trabajo de Jenkins y es posible escribirlos actualmente en forma de código lo cual se conoce como **pipeline as code** y nos permite que también **la automatización sea programática**. Esto nos permitirá levantar un Jenkins nuevo migrando el trabajo de otras automatizaciones.



## Instalación

En el curso efectúa la instalación sobre una distribución Linux. En la documentación vemos la instalación de windows mediante la descarga de un ejecutable.

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



* Al terminar la instalación nos dirigimos a `localhost:8080` (o el puerto que hayamos elegido) y para iniciar nos pedirá una contraseña almacenada en un achivo cuya ruta nos indicará. 

* Elegimos **Install suggested plugins**
* Creamos el usuario admin.



> Si queremos trabajar con Jenkins en inglés debemos configurar ese idioma como principal en Chrome.



## Manejo de Usuarios

La forma predefinida de hacerlo es mediante Jenkins (también puede hacerse usando Plugins). Es importante que cada persona tenga su propio usuario para poder auditar qué tarea fue realizada por cada persona.

