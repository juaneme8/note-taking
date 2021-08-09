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
Cuando abrimos la terminal nos encontramos en `~` que se conoce como *virgulilla* o *tilde* en inglés (`ALT+126`) y hace referencia al directorio personal del usuario que está logueado, por lo que si está logueado el usuario foo y hacemos `pwd` será `home/foo`. Si estamos en Windows con Git Bash (Linux emulado) será `/c/Users/foo`. Si estamos en el usuario root será `/root`



## Obtener Ayuda

Si queremos obtener ayuda sobre un comando en particular podemos ingresar `rm --help`



## File System

Así como en Windows tenemos:

* C:/

  * Program Files

  * Windows



En el File System de Linux tenemos la siguiente jerarquía:

* `/` : es el directorio raíz.

  * `bin`: es donde tenemos los binarios o programas. Si hacemos `ls /bin` veremos dentro programas como `echo`, `pwd`, etc.

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



## Comandos Básicos

### Comando `echo`

El comando `echo` se utiliza para mostrar líneas de texto que son pasadas como argumento en la línea de comandos. Por ejemplo podremos poner: `echo Hola Mundo` y mostraremos "Hola Mundo" en pantalla. 

>Luego veremos con el operador de redirección que es posible colocar ese mismo mensaje como contenido de un archivo en lugar de mostrarlo en la pantalla.

 

Con `echo $0` mostramos la ubicación del shell, con una imagen de Ubutu en un contenedor de Docker veo `bash`  (lo mismo veo desde WSL) y si lo hago en Git Bash veo `/usr/bin/bash`

> El nombre bash es un acrónico de **Bourne Again Shell**, ya que es una versión mejorada de Bourne shell y además parafraseando la noción de "born again".



### Comando  `whoiam`

El comando `whoami` nos permite ver cual es el usuario que está ejecutando la terminal, en nuestro caso veremos `root`.



### Comando `pwd`

Con el comando `pwd` *(print working directory)* podremos obtener la **ruta absoluta** donde nos encontramos dentro del file system.



### Comando `ls`

Con el comando `ls` *(list)* listamos el contenido del directorio donde estamos, podemos utilizar distintos **argumentos**:



Con `ls -1` listamos los archivos uno por fila.
Con `ls -l` (*long*) listamos los archivos uno por fila con su fecha de creación, peso (en bytes), permisos, etc.
Con `ls -lh` *(human)* vemos el peso de los archivos en M, K, etc.
Con `ls -a` (*all*) veremos todos los archivos inclusive los ocultos
Con `ls -S` (*size*) veremos todos los archivos y directorios ordenados por tamaño ordenado descendentemente.
Con `ls -r`*(reverse)* listamos los archivos en orden reverso al alfabético.

También es posible combinar opciones poniendo `ls -lahS` o `ls -lahr`



> Los elementos que nos aparecen en azul son directorios y los blancos son archivos.
>
> También es posible listar los archivos de otro directorio sin moverme de donde me encuentro, por ejemplo `ls /bin`.

### Comando `cd`
Con el comando `cd` *(change directory*) podremos navegar entre directorios.
Podemos utilizarlos con  **rutas relativas** (por ejemplo con `cd boot`) o **rutas absolutas** (aquellas que comienzan con el *root directory* `/`)

> **Operadores de ruta relativa**: Podemos utilizar `.` para referirnos al directorio actual y  `..` para referinos al directorio padre. Podemos utilizar `cd ..` para navegar al directorio padre. 

Si queremos navegar a `home` (`~`) bastará con poner `cd` sin parámetros.


> A diferencia de lo que ocurre cuando usamos Windows, debemos colocar un espacio entre `cd` y `..`.
>
> Si queremos **navegar al disco** c debemos poner `cd /c` (de la misma manera que en Windows pondríamos `cd C:`
>
> Linux es un sistema operativo **key sensitive** y los comandos debemos escribirlos en minúsculas. Por ejemplo no será lo mismo `Echo $0` que `echo $0`.  Los nombres de las carpeta también son **case sensitive**, esto significa que si tengo una carpeta llamada `carpeta` y otra llamada `Carpeta` y son dos distintas, mientras que en Windows si intento crear la segunda me dirá que ya existe una carpeta con ese nombre.
>
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



## Manipulación de Archivos y Directorios

### Comando `mkdir`

El comando `mkdir` nos permite crear una carpeta. Por ejemplo si queremos crear una carpeta "test".

```bash
mkdir test
```



### Comando `mv`

El comando `mv` nos permite renombrar archivos y carpetas o moverlo de ubicación. 

Por ejemplo para renombrar un directorio:

```bash
mv test docker
```

Mientras que si queremos renombrar un archivo:

```
mv hello.txt hello-docker.txt
```



### Comando `touch`

El comando `touch` me permite crear archivos vacíos.

```bash
touch file.txt
```



Si queremos crear múltiples archivos de una sola vez, podemos hacerlo con:

```bash
touch file1.txt file2.txt file3.txt
```



### Editar Archivos con Nano

> Si no lo tenemos instalado debemos ejecutar `apt install nano` y si no lo encuentra `apt update`

Si queremos crear el archivo `file1.txt` y a la vez abrirlo en el editor de texto nano podemos hacerlo con:

```bash
nano file1.txt
```

Luego realizamos los cambios y con `^X` indicamos que queremos salir por lo que luego nos preguntará si queremos guardarlo y para hacerlo ingresamos la letra `Y` y luego `ENTER` para confirmar el nombre sugerido.



### Comando `cat`

El comando `cat` (*concatenate*) nos permite mostrar el contenido de un archivo.

```bash
cat file.txt
```

Este comando es útil si el contenido del archivo es corto y entra en una página mientras que si es largo nos conviene utilizar el comando `more`.



El nombre del comando `cat` es una abreviatura de *concatenate*, lo cual nos da una pista de que podremos concatenar más de un archivo y mostrarlos juntos en pantalla:

```bash
cat file1.txt file2.txt
```



### Comando `more`

El comando `more` es útil cuando tenemos archivos largos y no queremos mostrar todo el contenido de una sino que lo queremos de manera paginada:

```
more /etc/adduser.conf
```

Con la tecla `ENTER` podremos avanzar una línea a la vez.
Con la tecla `SPACE` podremos avanzar una página a a la vez.
Con la tecla `Q` podremos salir del modo de navegación.



El problema con `more` es que sólo es posible hacer scroll hacia abajo, mientras que si queremos movernos en ambas direcciones debemos utilizar `less`.



### Comando `less`

El comando `less` viene a reemplazar a `more` y nos brinda la posibilidad de hacer scroll hacia arriba.

```
less /etc/adduser.conf
```

Con las `↑↓` navegamos el archivo hacia arriba y hacia abajo.
Con la tecla `ENTER` podremos avanzar una línea a la vez.
Con la tecla `SPACE` podremos avanzar una página a a la vez.



> Si no lo tenemos instalado `apt install less`



### Comando `head`

El comando `head` nos permite mostrar las primeras líneas de un archivo.

```
head /etc/adduser.conf
```

Si queremos especificar la cantidad de líneas que queremos mostrar: `head -n 10 /etc/adduser.conf`



### Comando `tail`

El comando `tail` nos permite mostrar las últimas líneas de un archivo.

```
tail /etc/adduser.conf
```

Si queremos especificar la cantidad de líneas que queremos mostrar: `tail-n 10 /etc/adduser.conf`

### Comando `rm`

El comando `rm` nos permite borrar archivos y directorios.

Por ejemplo para eliminar un archivo: 

```bash
rm file.txt
```



También es posible borrar más de un archivo a la vez:

```bash
rm file1.txt file2.txt file3.txt
```



O bien podemos borrar todos los archivos que tengan un cierto patrón, por ejemplo todos los archivos que comiencen con un cierto nombre:

```bash
rm file*
```



Para eliminar un directorio debemos hacerlo con `rm -r test` con lo cual eliminaremos la carpeta y todo su contenido recursivamente.

> En ocasiones también tendremos que forzar el borrado lo cual hacemos con `rm -rf test` (recursivo y forzado)



## Redirección

En Linux tenemos el concepto de *standard input/output*, donde *standard input* representa el teclado y *standard output* represente la pantalla. Se conoce como redirección al hecho de cambiar la fuente de entrada o de salida.



Cuando ejecutamos el comando `cat file1.txt`, este programa lee datos del archivo y los imprime en *standard output* es decir en la pantalla. Utilizando el operador de redirección del *standard output* `>`  podemos colocar el contenido leído en otro archivo:

```bash
cat file1.txt > file2.txt
```



Si queremos concatenar dos archivos en un archivo resultante:

```
cat file1.txt file2.txt > combined.txt
```



Esto no está liimitado a usarlo con `cat` sino que podremos utilizarlo en casi cualquier comando:

```bash
echo hello > hello.txt
```



También podremos generar un archivo a partir de una lista de archivos:

```bash
ls -l /etc > list.txt
```



> De la misma manera que tenemos el operador de redirección del *standard output* `>` tenemos el operador de redirección del standard input `<`.



## Búsqueda de Texto

### Comando `grep`

El comando `grep` (*global regular expression print*) nos permite buscar texto en un archivo, por ejemplo si queremos buscar la cadena "hello" en el archivo `file.txt`:

```
grep hello file.txt
```



Recordemos que como todo en Linux la búsqueda es case sensitive, para hacerlo case insentitive debemos agregar `-i`

```bash
grep -i hello file.txt
```

```bash
grep -i root /etc/passwd
```



Búsqueda en múltiples archivos:

* Podemos especificarlos todos:

```bash
grep -i hello file1.txt file2.txt
```



* Podemos especificar un patrón:

```bash
grep -i hello file*
```



Búsqueda texto en los archivos de un determinado directorio, pero para eso debemos usar también la opción `-r` de recursiva (para hacer una búsqueda en el directorio y todos su subdirectorios):

```bash
grep -i -r hello /etc
```

O lo que es lo mismo combinando opciones:

```
grep -ir hello /etc
```



Si queremos buscar en el directorio actual:

```
grep -ir hello .
```



## Búsqueda de Archivos y Directorio

### Comando `find`

Con el comando `find`  nos permite buscar archivos y directorios. 

Si lo utilizamos sin argumentos podremos mostrar todos los archivos y directorios en el directorio actual.

```bash
find
```



Si queremos buscar en un directorio en particular se lo pasamos como argumento:

```bash
find /etc
```

#### Filtros

* Si queremos filtrar sólo los directorios podemos agregarle la opción `-type d`

```
find -type d
```



* Si queremos filtrar sólo los archivos podemos agregarle la opción `-type f`

```
find -type f
```



* Si queremos filtrar todos los archivos cuyo nombre comienza con "f"

```
find -type f -name "f*"
```



* Si queremos filtrar los archivos cuyo nombre comienza con "f" o con "F"

```
find -type f -iname "f*"
```



* Si queremos filtrar todos los archivos `.py` y grabar un archivo de texto con ese listado

```
find / -type f -name "*.py" > python-files.txt
```

> Notar que en la búsqueda queremos comenzar por el *root directory*.



## Encadenar Comandos

### Operador semi-colon `;`

Es posible encadenar comandos separándolos por `;` y colocándolos en la misma línea y recién después del último presionar `ENTER`.

```bash
mkdir test ; cd test ; echo hello
```

Suponiendo que ya tenemos un directorio `test` nos tiraría error el primer comando pero los otros dos se ejecutarían igualmente. 

### Operador AND `&&`

El **operador AND** nos permite ejecutar varios comandos encadenados de izquierda a derecha deteniéndose cuando uno falle.

En este caso suponiendo que ya tenemos un directorio `test`  como va a fallar el primer comando no se ejecutarán los otros dos.

```bash
mkdir test && cd test && echo hello
```



### Operador OR `||`

El **operador OR** nos permite que si el comando de la izquierda falla se ejecute el de la derecha y en caso de que el de la izquierda no falle el de la derecha no se ejecute.

```bash
mkdir test || echo "El directorio ya existe"
```



### Operador Pipe `|`

El **operador pipe** nos permite tomar la salida de un comando y enviarla a otro comando. 

Anteriormente hemos utilizado el comando `less` para mostrar el contenido de un archivo de manera paginada por ejemplo `less /etc/adduser.conf` pero supongamos que ahora queremos listar los archivos de la carpeta `/bin` de manera paginada. Para eso haciendo uso del **operador pipe** podríamos hacer:

```bash
ls /bin | less
```



De manera similar si queremos motrar los primeros resultados:

```
ls /bin | head -n 5
```



## Comando Multi-línea

Es posible dividir un comando largo en múltiples líneas, de manera tal de evitar tener que hacer un scroll horizontal y favorecer la legibilidad.

En lugar de ingresar:

```
mkdir hello; cd hello; echo done
```



Ingresamos

```
root@2f75...: ~# mkdir hello;\
```

Cuando presionamos `ENTER` veremos que el prompt pasa a ser `>`

```
> cd hello;\
```

```
> echo done
```



## Variables de Entorno

Las **variables de entorno** nos permiten almacenar configuraciones que luego serán leídas por nuestras aplicaciones.

### Comando `printenv`

* Para listar todas las variables de entorno disponibles en esta máquina.

```bash
printenv
```



```bash
HOSTNAME=17fd53d105f6
PWD=/
HOME=/root

...

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```



La variable de entorno `PATH` es una lista de directorios separados por `:` y es en esos directorios en los cuales el sistema operativo buscará los programas que intentemos ejecutar.

Cuando intentamos ejecutar un programa y nos aparece un mensaje indicando que no fue encontrado, a pesar de que lo tenemos instalado esto sucede debido a que la ubicación de este programa no está en dicha variable.



* Para conocer el valor de una variable de entorno en particular podremos hacerlo de dos formas:

```bash
printenv PATH
```

```
echo $PATH
```



* Para setear una variable de entorno en la sesión actual de la terminal, por lo que si cerramos y abrimos una nueva, no existirá.

```
export DB_USER=mosh
```

> Si estamos corriendo un Linux con Docker podremos cerrar la sesión actual de la terminal con `exit` y con `docker ps -a` veremos el proceso detenido. Si queremos reiniciarlo podemos hacerlo con `docker start -i 2f7` siendo `2f7` las primeras tres letras del contenedor.



* Para setear una variable de entorno de manera persistente debemos hacerlo en `.bashrc` que encontramos en el root directory.

```bash
cd ~
nano .bashrc
```



Podemos editarlo con nano o hacer uso de una redirección:

```bash
echo DB_USER=mosh > .bashrc
```

Pero si hiciéramos esto estaríamos reemplazando la totalidad del archivo con el nuevo contenido, mientras que lo que queremos es agregar nuevo contenido, por lo que debemos utilizar `>>`

```bash
echo DB_USER=mosh >> .bashrc
```



> No debemos almacenar información sensible en variables de entorno, ya que estará almacenada en texto plano y podría ser accedida por cualquiera con acceso a la máquina.



Luego con `cat .bashrc` podemos verificar que los cambios en el archivo se hicieron exitosamente. Sin embargo estos serán cargados recién en la próxima sesión de la terminal, dado que el archivo `.bashrc` se carga sólo al iniciar. Por lo que si hacemos `echo $DB_USER` no nos devolverá nada hasta que iniciemos otra. 



### Comando `source`

Si bien para obtener los nuevos valores del archivo `.bashrc` podemos ejecutar `exit` y luego `docker start -i 2f7` otra opción es hacerlo con el comando `source .bashrc` desde el directorio home (`~`) o bien ejecutar `source ~/.bashrc` desde cualquier ubicación.



## Procesos

Los procesos son instancias de un programa en ejecución.



### Comando `ps`

El comando `ps` nos permite ver todas los procesos en ejecución.

```bash
  PID TTY          TIME CMD
    1 pts/0    00:00:00 bash
    9 pts/0    00:00:00 ps
```

En la columna **PID** vemos el identificador único de cada proceso.

En la columna **TTY** que significa *teletypewriter*  vemos una referencia al tipo de terminal en la que está logueado el usuario. Vemos `pts/0` donde `pts` significa  *pseudo terminal slave* y el `0` hace referencia a la primera ventana de la terminal. Si abrimos otra ventana y ejecutamos el mismo comando veremos `pts/1`

En la columna **CMD** con PID 9 vemos un proceso `ps` que solo existió mientras preparamos ese comando para mostrar la salida con esos datos en pantalla, es por esto que si lo volvemos a ejecutar veremos nuevamente a `ps` pero con un nuevo PID.

Por otra parte `bash` es el programa con el cual interactuamos, que toma nuestros comandos y se los pasa a Linux para que sean ejecutados.

En la columna **TIME** vemos la cantidad de tiempo de CPU consumido por el proceso, que en nuestro caso al ser muy livianos es prácticamente 0. En ocasiones notaremos que el sistema se vuelve lento a causa de un proceso y debemos proceder a matarlo.

Por ejemplo si ejecutamos `sleep 100` durante 100 segundos estará el prompt durmiendo y luego de ese tiempo volverá a estar activo. Si queremos en cambio enviar ese proceso al *background* para poder ejecutar otros comandos mientras tanto, ejecutamos `sleep 100 &`. Si ejecuutamos `ps` veremos ese proceso ya que bajo la columna `CMD` dirá `sleep`.



### Comando `kill`

El comando `kill` nos permite eliminar un proceso determinado. Por ejemplo suponiendo que con `ps` obtenemos la siguiente salida:

```bash
  PID TTY          TIME CMD
    1 pts/0    00:00:00 bash
   15 pts/0    00:00:00 sleep
   17 pts/0    00:00:00 ps
```

Si queremos eliminar el proceso `sleep` tendremos que hacer

```bash
kill 15
```



## Manejo de Usuarios

### Comando `useradd`

El comando `useradd` nos permite agregar un nuevo usuario  

Si ingresamos `useradd` veremos las opciones con las cuales podemos utilizar este comando. 

> Veremos que existe una forma corta (con un guion) y una forma larga (con dos guiones) de ingresar estas opciones, por ejemplo podremos crear un usuario junto con su directorio home con `useradd -m` o con `useradd --create-home`



Si queremos crear un usuario john y su directorio home:

```
useradd -m john
```



Podremos constatar esta acción revisando el archivo de configuración que contiene información de las cuentas de usuario: `cat /etc/passwd`, que nos entregará entre otras cosas lo siguiente:

```
john:x:1000:1000::/home/john:/bin/sh
```

Veremos que tenemos múltiples campos separados por `:`.

Primero tenemos `john` que es el nombre de usuario, luego `x` que significa que el password está almacenado en otro lugar, luego `1000` es el userId, luego `1000` es el groupId, luego `/home/john` es el directorio home de este usuario y por último `/bin/sh` es el programa de shell utilizado. 

El programa`/bin/sh` hace referencia al programa Shell original (que luego fuera mejorado por Bash). Si quiséramos utilizar Bash en lugar Shell deberíamos utilizar el comando `usermod`

### Comando `usermod`

El comando `usermod` nos permite modificar un usuario existente.



### Comando `userdel`

El comando `userdel` nos permite eliminar un usuario.
