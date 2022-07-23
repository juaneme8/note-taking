> Basado en el Bootcamp
>
> :ok: Módulo GIT 21 - 37 COMPLETO

# Introducción

El propósito de este bootcamp es generar los procesos de DevOps con los que tendrá que lidiar un DevOps Engineer



Existen distintos roles IT en lo que es el desarrollo de software.

* Programming (developers)
* Software Testing (developers, dedicated testers, automated testing).
* Release (build application, run on servers, upgrade existing software)
* Operations run software in productions.



La comunicación entre operations y development tiene el objetivo de que cuando los developers crean una nueva feature en un nuevo release esta sea implementada en los servidores de producción sin que haya un downtime.



## CI

CI (continuos integration) o Integración Continua hace referencia a integrar los cambios del código frecuentemente, de esta manera si pusheamos y pulleamos regularmente es menos probable que nos enfrentemos a *merge conflicts* difíciles de resolver.



## Git como DevOps Engineer

En primer lugar necesitaremos de Git cuando trabajamos con conceptos de **Infraestructure as Code** si trabajamos con un *developer team* que realizó un deployment en Kubernetes, en ese caso tendremos que crear varios archivos de configuración de Kubernetes (para crear volúmenes, databases, etc ) y debemos manejarlos de algún modo. 

Supongamos además que el cluster de Kubernetes es deployado en AWS, y para manejar el servidor AWS debemos utilizar herramientas de automatización como Terraform y Ansible por lo que también tendremos esos archivos de configuración que manejar. A esto se sumarán scripts de bash y python.

En esos casos tendremos que garantizar que trackeamos el historial de cambios de los archivos, los almacenamos en un lugar seguro y que sea factible la colaboración por parte de todos los miembros del *DevOps team* a la hora de modificar estos archivos.



En segundo lugar cuando trabajamos con **CI/CD Pipeline and Build Automation** como Jenkins debemos hacer hacer checkout del código, test and build application. Debemos integrar la herramienta de automatización con el repositorio de la aplicación.

En ocasiones vamos a necesitar por ejemplo obtener el commit hash de un commit en particular (por ejemplo el último), chequear si hubo cambios en el código del un frontend o backend y así decidir si ejecutar tests, etc.