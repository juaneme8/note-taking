VIDEOS 2 COMPLETO
# Introducción a la terminal y Línea de Comandos
> Basado en el Curso de Platzi de Enrique Devars
>
> Agregados sobre comandos de Linux de Ultimate Docker Course de Mosh Hamedani

## Motivaciones
La terminal se caracteriza por brindarnos **flexibilidad** para realizar todo tipo de tareas con unos pocos comandos, **velocidad** a la hora de realizarlos y es **fundamental a la hora de trabajar con servidores remotos** es la única opción ya que no contamos con una interfaz gráfica.

## ¿Qué es la terminal?
La terminal es una **interfaz gráfica** que muestra el *prompt* y simula una **línea de comandos** o **shell**. La **shell** es un programa que en función de los comandos que le pasamos le indica al **sistema operativo** la acción que debe realizar.

Existen distintos tipos de **shells**:
* Bash Shell (el curso es basado en esta shell y utilizando Arch Linux)
* Z Shell (presente en macOS de 2019 en adelante, casi los mismos comandos que Bash Shell)
* PowerShell (shell de windows con algunos comandos diferentes)

**Un comando es un programa** que se puede ejecutar desde la terminal y puede recibir parámetros y opciones.

> En caso de trabajar con Windows para utilizar Bash Shell es posible utilizar **Windows Subsystem for Linux (WSL)** 

## Instalación WSL
1. Ir a Add or remove programs -> Optional features --> More Windows Features y agregar **Windows Subsystem for Linux** y presionar OK. Luego nos aparecerá un cartel de actualización y nos pedirá reiniciar.
2. Luego de reiniciar ir nuevamente a la misma pantalla y esta vez elegir **Virtual Machine Platform** nuevamente nos pedirá reiniciar.
3. Por último ir a Microsoft Store y descargar Ubuntu 20.04 LTS.
4. Asignar un nombre de usuario y contraseña.

Luego para actualizar los paquetes:
```bash
sudo apt update
```

## Estructura de Archivos
Si estamos trabajando en un sistema Linux (o en Windows utilizando WSL) tendremos un **sistema de archivos** rutas a través de las cuales vamos a caminar similar al siguiente (en macOS es un poco distinto)

1. `/` raíz del sistema de archivos
2. como hijo de `/` tenemos `etc` `dev` `home` `usr` `var`
3. como hijo de `home` los directorios de los usuarios (por ejemplo `foo`)
4. como hijo de `usr` tendremos `lib`

## Primeros Pasos
Cuando abrimos la terminal nos encontramos en `~` que se conoce como *virgulilla* (`ALT+126`) y hace referencia al directorio personal del usuario que está logueado, por lo que si está logueado el usuario foo y hacemos `pwd` será `home/foo`. Si estamos en Windows con Git Bash (Linux emulado) será `/c/Users/foo`



## Comandos

### Comando `echo`

El comando `echo` se utiliza para mostrar líneas de texto que son pasadas como argumento en la línea de comandos. Por ejemplo podremos poner: `echo Hola Mundo` y mostraremos "Hola Mundo" en pantalla.

Con `echo $0` mostramos la ubicación del shell, en mi caso veo `bash`. 

> El nombre bash es un acrónico de **Bourne Again Shell**, ya que es una versión mejorada de Bourne shell y además parafraseando la noción de "born again".
>
> Linux es un sistema operativo **key sensitive** y los comandos debemos escribirlos en minúsculas. Por ejemplo no será lo mismo `Echo $0` que `echo $0`. 



### Comando  `whoiam`

El comando `whoami` nos permite ver cual es el usuario que está ejecutando la terminal, en nuestro caso veremos `root`.



### Comando `pwd`

Con el comando `pwd` *(print working directory)* podremos obtener la **ruta absoluta** donde nos encontramos dentro del file system.



### Comando `ls`

Con el comando `ls` *(list)* listamos el contenido del directorio donde estamos, podemos utilizar distintos **argumentos**:



Con `ls -l` (*long*) listamos los archivos con su fecha de creación, peso (en bytes), permisos, etc.
Con `ls -lh` *(human)* vemos el peso de los archivos en M, K, etc.
Con `ls -a` (*all*) veremos todos los archivos inclusive los ocultos
Con `ls -S` (*size*) veremos todos los archivos y directorios ordenados por tamaño ordenado descendentemente.
Con `ls -r`*(reverse)* listamos los archivos en orden reverso al alfabético.

También es posible combinar opciones poniendo `ls -lahS` o `ls -lahr`

### Comando `cd`
Con el comando `cd` *(change directory*) podremos navegar entre directorios.
Por ejemplo queremos navegar al directorio `Pictures` podemos hacerlo con el comando `cd Pictures` *(change directory)*

Si queremos navegar a `home` (`~`) bastará con poner `cd` sin parámetros.

**Operadores de ruta relativa**: Podemos utilizar `.` para referirnos al directorio actual y  `..` para referinos al directorio padre. 

Podemos utilizar `cd ..` para navegar al directorio padre. 
> A diferencia de lo que ocurre cuando usamos Windows, debemos colocar un espacio entre `cd` y `..`.

> Si queremos **navegar al disco** c debemos poner `cd /c` (de la misma manera que en Windows pondríamos `cd C:`

> En Linux y MacOS puedo tener los nombres de las carpeta son **case sensitive**, esto significa que si tengo una carpeta llamada `carpeta` y otra llamada `Carpeta` y son dos distintas, mientras que en Windows si intento crear la segunda me dirá que ya existe una carpeta con ese nombre.

> El **autocompletado** me permite si quiero navegar a una carpeta llamada `Users` puedo poner `cd U` y presionar `TAB` y me mostrará todas las que comienzan con esa letra, si sólo hubiera una con los caracteres ingresados la autocompletará.



### Comando `clear`

El comando `clear` nos permite limpiar la pantalla y esto también podemos hacerlo con `CTRL+L`



### Comando `history`

Con la flecha hacia arriba y hacia abajo podremos navegar los últimos comandos ingresados. 
Si queremos obtener un listado de todos los comandos ingresados podemos escribir el comando `history` y nos aparecerán todos los comandos asociados a un número, por ejemplo:

```bash
11  git status
12  git add .
13  git log
14  git commit -m "Cambios menores v4.4.6"
15  git push origin master
16  clear
```
Si quisiéramos ejecutar el comando `clear` podríamos poner `!16`

Esto lo hace gracias a un archivo llamado `history` (o `.bash.history` si trabajamos con Git Bash) y ubicado en `home`



### Comando `file`

El comando `file` nos dará información acerca de un archivo.
Por ejemplo si estamos en `Pictures` y tenemos un archivo `screenshot.png` con `file screenshot.png` podremos obtener sus características.



### Comando `tree`

El comando `tree` nos permite obtener un árbol de todos las carpetas  y archivos. 

Si queremos tener sólo una profundidad de dos niveles podríamos poner `tree -L 2`

> Para poder utilizar este comando debemos instalar `tree`
```bash
sudo apt install tree
```



### Comando `mkdir`

El comando `mkdir` nos permite crear una carpeta.
```bash
mkdir proyecto1
```



### Comando `touch`

El comando `touch` me permite crear archivos vacíos.
```bash
touch archivo .txt
```



### Comando `cat`

El comando `cat` nos permite mostrar el contenido de un archivo
```bash
cat archivo .txt
```



### Comando `rm`

El comando `rm` nos permite borrar un archivo. 
```bash
rm archivo .txt
```

Si queremos borrar una carpeta `rm -rf proyecto1` (recursivo y forzado)

> Debemos tener precaución a la hora de utilizar este comando.



## Obtener Ayuda

Si queremos obtener ayuda sobre un comando en particular podemos ingresar `rm --help`



## File System

Así como en Windows tenemos:

* C:/

  * Program Files

  * Windows



En el File System de Linux tenemos la siguiente jerarquía:

* `/` : es el directorio raíz.

  * `bin`: es donde tenemos los binarios o programas.

  * `boot`: es donde tenemos archivos relacionados con el booteo.

  * `dev` (devices): es donde tenemos archivos necesarios para acceder a dispositivos. 

  * `etc` (editable text configuration): es donde tenemos archivos de configuración.

  * `home`: es donde tenemos el directorio home de cada uno de los usuarios de la máquina.

  * `root`: es el home del usuario root.

  * `lib` (library) es donde almacenamos librerias de dependencias.

  * `var` (variable) es donde almacenamos archivos que cambian frecuentemente como ser archivos de aplicaciones, logs, etc. 

  * `proc` (process) es donde almacenamos archivos que representan programas en ejecución.

    

> En Linux todo es una archivo incluyendo dispositivos, directorios, sockets, etc 



> En  Linux utilizamos *forward slash* `/` para separar directorios y archivos mientras que en Windows utilizamos *backward slash* `\`
>





### Navegar el File System

`pwd` (*print working directory*) nos permite mostrar en qué directorio nos encontramos dentro del file system.

`ls` (*list*) nos permite listar los archivos y directorios donde nos encontremos. Por default mostrará varios elementos en la misma línea.

 * `ls -1` para mostrar un elemento por línea.
 * `ls -l` *long listing* para mostrar un elemento por línea y con información extra. Permisos, dueño, tamaño, fecha, etc.
 * `cd` admite un path relativo o absoluto.



## Manipulación de Archivos 