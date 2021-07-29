# Docker

> Basado en Ultimate Docker Course de Mosh Hamedani (VIDEO 6 COMPLETO)

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



La [documentación](https://docs.docker.com/docker-for-windows/install/) indica que debemos tener WSL-2 instalado de acuerdo a [esta guía](https://docs.microsoft.com/en-us/windows/wsl/install-win10). Es importante reiniciar antes de instalar el `msi`

Siguiendo los pasos establecidos en 

>  Con el comando `docker version` obtenemos la versión instalada.
