# Introducción a la terminal y Línea de Comandos
> :link: Basado en el Curso de Platzi de Enrique Devars - VIDEOS 2 COMPLETO
>
> :link: Basado en el video de [Learn Linux TV](https://www.youtube.com/watch?v=5EqL6Fc7NNw) sobre comandos `head` y `tail`.
>
> :link: Basado en el video de [Learn Linux TV](https://youtu.be/Tc_jntovCM0?list=PLT98CRl2KxKHKd_tH3ssq0HPrThx2hESW) sobre comand  `grep`.
>
> :link: Basado en el video de [Learn Linux TV](:link: Basado en el video de [Learn Linux TV](https://youtu.be/Tc_jntovCM0?list=PLT98CRl2KxKHKd_tH3ssq0HPrThx2hESW) sobre comand  `grep`.) sobre comand  `ping`.
>
> :link: Agregados sobre comandos de Linux de Ultimate Docker Course de Mosh Hamedani
>
> :link: Agregados video [30 Comandos Linux para manejar tu Server](https://youtu.be/0BA4k3jweaE) del Pelado Nerd 



## Motivaciones
La terminal se caracteriza por brindarnos **flexibilidad** para realizar todo tipo de tareas con unos pocos comandos, **velocidad** a la hora de realizarlos y es **fundamental a la hora de trabajar con servidores remotos** es la única opción ya que no contamos con una interfaz gráfica.

## ¿Qué es la terminal?

Linux cuenta con dos tipos de interfaces CLI (*Command Line Interface*) y GUI (**Graphical User Interface**) . La mayoría de los servidores no contarán con una GUI, por lo que será fundamental aprender cómo funciona la CLI para poder manejarnos cómodamente cuando tengamos que realizar alguna tarea.

La terminal es una **interfaz gráfica** que muestra el *prompt* y simula una **línea de comandos** o **shell**. 

## Shell

Recibe el nombre de **Linux Shell**  el programa que en función de los comandos que le pasamos le indica al **sistema operativo** la acción que debe realizar.

Existen distintos tipos de **shells** con comportamientos también distintos:

* Bourne Shell (sh)
* CShell (csh o tcsh)
* Z Shell (presente en macOS de 2019 en adelante, casi los mismos comandos que Bash Shell)
* Bourne Again Shell (bash) (el curso de Devars es basado en esta shell y utilizando la distribución de Linux liviana Arch Linux). Cuenta con más funcionalidades que shell



Si queremos conocer cual shell estamos utilizando 

```
echo $SHELL
```



**Un comando es un programa** que se puede ejecutar desde la terminal y puede recibir parámetros y opciones.

> En caso de trabajar con Windows para utilizar Bash Shell es posible utilizar **Windows Subsystem for Linux (WSL)** 

## Instalación WSL
1. Ir a **Add or remove programs -> Optional features --> More Windows Features** y agregar **Windows Subsystem for Linux** y presionar OK. Luego nos aparecerá un cartel de actualización y nos pedirá reiniciar.
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
Cuando abrimos la terminal nos encontramos en `~` que se conoce como *virgulilla* o *tilde* en inglés (`ALT+126`) y hace referencia al directorio personal del usuario que está logueado. Esto significa que si está logueado el usuario `foo` y hacemos `pwd` será `home/foo` mientras que si está logueado el usuario `root` será `/root`. 

> En Windows con Git Bash (Linux emulado) si estamos en `~` con el usuario `foo` obtendremos `/c/Users/foo`. 





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



## Teclas Rápidas

Limpiar Pantalla: `CONTROL+L`

Cancelar Comando: `CONTROL+C`

Búsqueda Inversa de Comandos: `CONTROL+R`

Borrar comando entero: `CONTROL+U`

Ir al final de comando: `CONTROL+E`

Ir al principio de comando: `CONTROL+A`

Eliminar la palabra donde tengo el cursor `CONTROL+W`

Intercambiar el caracter donde tengo el cursor por el anterior `CONTROL+T`

Intercambiar palabras (tengo que tener el cursor al terminar la palabra) `ESC+T`

Salir de un servidor `CONTROL+D` (es equivalente a escribir `exit`)

Maximizar terminal `F11` (no funciona en Ubuntu en Windows 10).

Aumentar fuente: `CONTROL+SHIFT++` (no funciona en Ubuntu en Windows 10)

Disminuir fuente: `CONTROL+-` (no funciona en Ubuntu en Windows 10)



## Comandos Básicos

### Comando `help`

El comando `help` nos permite obtener ayuda sobre cualquier comando, por ejemplo `rm --help`, `rsync --help` para obtener información sobre `rm` y `rsync` respectivamente.

>  Si queremos filtrar además del resultado de dicha ayuda (utilizando el comando `grep` y **pipes**) podemos ingresar `rsync --help | grep verbose` con lo cual mostraremos solo la/s línea/s donde aparece la palabra "verbose" en la ayuda.



### Comando `echo`

El comando `echo` se utiliza para mostrar líneas de texto que son pasadas como argumento en la línea de comandos. Por ejemplo podremos poner: `echo "Hola Mundo"` y mostraremos "Hola Mundo" en pantalla. 

>Luego veremos con el operador de redirección que es posible colocar ese mismo mensaje como contenido de un archivo en lugar de mostrarlo en la pantalla.

 

#### `echo` de variable

```
msg="Hello World"
echo $msg
```

Si queremos imprimir alguna variable de la sesión, podemos listarlas con 

```
env
```

```
echo $HOME
```



#### `echo` de proceso corriendo:

Con `echo $0` mostramos la ubicación del shell, con una imagen de Ubuntu en un contenedor de Docker veo `bash`  (lo mismo veo desde WSL) y si lo hago en Git Bash veo `/usr/bin/bash`

> El nombre bash es un acrónico de **Bourne Again Shell**, ya que es una versión mejorada de Bourne shell y además parafraseando la noción de "born again".



### Comando  `whoami`

El comando `whoami` nos permite ver cual es el usuario que está ejecutando la terminal, en nuestro caso veremos `root`.



### Comando `id`

El comando `id` nos permite obtener el uid, gid, groups y los grupos de los que forma parte el usuario.



### Comando `pwd`

Con el comando `pwd` *(present working directory)* podremos obtener la **ruta absoluta** donde nos encontramos dentro del file system.



### Comando `ls`

Con el comando `ls` *(list)* listamos el contenido del directorio donde estamos, podemos utilizar distintos **argumentos**:



Con `ls -1` listamos los archivos uno por fila.
Con `ls -l` (*long*) listamos los archivos uno por fila con su fecha de creación, peso (en bytes), permisos, etc.
Con `ls -lh` *(human)* vemos el peso de los archivos en M, K, etc.
Con `ls -a` (*all*) veremos todos los archivos inclusive los ocultos. En Linux los archivos ocultos son aquellos que comienzan con un punto.
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

* Si queremos navegar a `home` (`~`) bastará con poner `cd` sin parámetros.
* Si queremos navegar a la raíz (`/`) debemos poner `cd /`


> A diferencia de lo que ocurre cuando usamos Windows, debemos colocar un espacio entre `cd` y `..`.
>
> Linux es un sistema operativo **key sensitive** y los comandos debemos escribirlos en minúsculas. Por ejemplo no será lo mismo `Echo $0` que `echo $0`.  Los nombres de las carpeta también son **case sensitive**, esto significa que si tengo una carpeta llamada `carpeta` y otra llamada `Carpeta` y son dos distintas, mientras que en Windows si intento crear la segunda me dirá que ya existe una carpeta con ese nombre.
>
> El **autocompletado** me permite si quiero navegar a una carpeta llamada `Users` puedo poner `cd U` y presionar `TAB` y me mostrará todas las que comienzan con esa letra, si sólo hubiera una con los caracteres ingresados la autocompletará.

* Si queremos navegar al directorio en el que estuvimos anteriormente podemos hacer `cd -`



### Ejecutar Múltiples Comandos

Si queremos ejecutar varios comandos uno después del otro podemos separarlos por `;`.

```
cd new_directory; mkdir www; pwd
```



### Comando `clear`

El comando `clear` nos permite limpiar la pantalla y esto también podemos hacerlo con `CONTROL+L`. 



### Comando `reset`

Cuando usamos el comando `clear` o `CONTROL+L` si scrolleamos para arriba veremos los comandos ingresados anteriormente. En caso de no querer ver esto podemos ingresar el comando  `reset` y se borrará todo por completo.



### Comando `sed`

El comando `sed` (que significa **s**tring **ed**itor) nos permitirá hacer sustituciones de texto es decir buscar y reemplazar el texto deseado en múltiples archivos.

```
sed -i 's/Tony/Antonio/g' file.doc
```

Notar la importancia del flag `-i` para lograr el reemplazo in situ sino nos devolverá el archivo con el cambio pero no alterará el archivo original.



> Se trata de un comando de cuatro partes: `'s/texttobechanged/textwanted/g'` primera parte del comando `s` significa **substitution** y la última `g` **global** de modo que afecte las múltiples ocurrencias en caso de haberlas.



Si queremos modificar múltiples archivos debemos apoyarnos en el uso del comando `find`, por ejemplo para cambiar todas las ocurrencias en los archivos de tipo txt podemos hacer:

```
find . --name "*.txt" --exec sed -i "s/Tony/Antonio/g" {} \;
```

Con `{} \;` indicamos que queremos ejecutar el comando para cada archivo individualmente. `{}` se reemplazará por el nombre de archivo entregado por `find` y `\;` indica el fin del comando `exec`

#### Caso de uso con Docker

Un caso de uso interesante podría ser que tengamos que editar un archivo de configuración (por ejemplo dentro de un contenedor) y no tengamos ningún editor disponible  (`vi`, `vim`, `nano`).

Una solución podría ser instalar uno de estos paquetes, pero puede que no tengamos privilegios o aún teniéndolos si son cambios pequeños puede que nos convenga editar el archivo al vuelo.

```
sed -i "/s/texttobechanged/textwanted/g" filename
```



Ejemplo de uso:

```
docker exec 900b... sed -i "s/It works!/Docker1/" /usr/local/apache2/htdocs/index.html
```

> En Windows este comando utilizando comillas simples no funcionó.



### Comandos `pushd` y `popd`

Mediante la utilización del comando "**pushd**" podemos decirle a nuestro shell que guarde temporalmente la ubicación actual de mi directorio y acto seguido posicionarnos en una nueva carpeta, luego con el comando "**popd**" volveremos a la carpeta original sin necesidad de andar escribiendo toda la ruta original.



Si estamos en la carpeta `/home/juaneme8/trabajos` y luego hacemos `pushd /etc` nos dirigiremos a `/etc`, podremos navegar y hacer lo que necesitemos pero si ingresamos `popd` volveremos automáticamente a `/home/juaneme8/trabajos`



### Comando `history`

> [Video Explicativo](https://www.youtube.com/watch?v=Odo6leOY5Fw&t=385s)

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



Debemos tener presente que cada usuario tendrá su propio historial, por lo que en ocasiones puede ser interesante loguearnos para saber cómo se actúo ante una determinada situación.



#### Timestamp de Historial

Si queremos obtener el timestamp de cuándo fue ingresado cada comando debemos cargar la variable `HISTTIMEFORMAT` con el formato del timestamp que deseamos:

```
HISTTIMEFORMAT="%Y-%m-%d %T "
```

Luego cuando ingresemos `history` veremos el comando acompañado de la hora de cuando fue ingresado. En algunas distribuciones no nos mostrará la fecha correcta de los pasados pero sí trackeará la fecha a partir de ahora.

Esta variable estará almacenada sólo en la sesión, por lo que **si cerramos la ventana se perderán esos cambios**. Para hacerlo permanente editamos el archivo `~/.bashrc` por ejemplo con `nano ./.bashrc` colocando `HISTTIMEFORMAT="%Y-%m-%d %T "`  (notar el espacio después del timestamp y antes del comando).



> En ese mismo archivo (`.bashrc`) veremos el comando `HISTCONTROL=ignoreboth` que hace referencia a que no queremos guardar en el historial líneas duplicadas ni tampoco aquellas líneas que comienzan con un espacio. Esto puede ser **útil a la hora ocultar un comando con información sensible** (antecediendo un espacio.)



#### Repetir Comando `!`

Si quisiéramos ejecutar un comando en particular que sabemos que ya ejecutamos, por ejemplo `clear` podríamos ejecuctar `history` y de acuerdo al número que aparece al comienzo poner `!16`

Esto lo hace gracias a un archivo llamado `.bash_history` en el directorio home del usuario.



> En Linux normalmente al signo de exclamación se lo conoce como *bang*.



#### Repetir último Comando (`!!`) 

> Con `!!` referenciamos el último comando ejecutado. Esto puede tener un caso interesante de uso cuando ingresamos un comando y nos olvidamos de poner `sudo`, poniendo `sudo !!` (*bang bang*) evitamos tener que escribir el comando nuevamente.
>
> Por ejemplo si queremos actualizar el índice del repositorio de paquetes 
>
> ```
> apt update
> sudo !!
> ```

### 

#### Listar n comandos del historial

```
history 4
```



#### Filtrar comandos del historial

Si queremos filtrar los comandos que tengan una determinada cadena podemos encadenar el comando `grep` utilizando pipes.

```
history | grep apt
```



#### **Reverse Search** en el historial (`CONTROL+R` )

> Si sabemos que utilizamos un comando que tenía la cadena "hola" podemos hacer uso del comando `grep` y de un pipe: `history | grep hola`. Esto mismo podemos hacerlo también ingresando `CONTROL+R` y escribiendo por ejemplo "hola" y nos mostrará el último comando que ingresamos que tenía esa cadena y si presionamos `CONTROL+R` nuevamente iremos hacia atrás en el historial donde se reúne esa condición. 
>
> Si queremos ejecutarlo tal cual presionamos `ENTER` y sino la flecha hacia el costado para editarlo. Si queremos salir presionamos `CONTROL+C`



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



### Comando `du`

El comando `du` *disk usage* nos permite ver el uso de disco de un directorio:

```bash
du -sh /var/lib
```

Con `-s` o `--summarize` indicamos que queremos mostrar solo el total.
Con `-h` o `--human-readable` indicamos que queremos mostrar K, M, G en lugar de bytes.



Si queremos ver cuánto pesa cada uno de los archivos o directorios dentro de un directorio podemos hacerlo con:

```
du -sh /var/lib/*
```

Esto puede ser de utilidad cuando tenemos un directorio que ocupa mucho espacio y queremos individualizar que subdirectorio es el responsable.



### Comando `stat`

El comando `stat` nos muestra información sobre un archivo.

```bash
stat file1.txt
```

Obtendremos información sobre su tamaño, directorio en el cual está montado, permisos, fecha de modificación, de último acceso, de cambios, etc.



### Comando `zip`

El comando `zip` nos permite comprimir un directorio

```bash
zip -r images.zip images
```

Indicamos el argumento `-r` luego el nombre que queremos de salida y por último la carpeta de entrada.



### Comando `unzip`

El comando `unzip` nos permite descomprimir un directorio

```bash
unzip images.zip
```



### Comando `zipinfo`

El comando `zipinfo` nos permite ver el contenido de un zip pero sin descomprimirlo

```bash
zipinfo images.zip
```



### Comando `cal`

El comando `cal` muestra un calendario en pantalla del mes actual.

* Si queremos ver un mes y un año en particular podemos pasarlo como argumento. Por ejemplo si queremos ver Enero del 2022:

```
cal 01 2022
```



### Comando `date`

El comando `date`  nos permite mostrar la fecha y hora actual.

* Si queremos mostrar una fecha futura, por ejemplo dentro de 7 días:

```
date -d "+7 days"
```



### Comando `bc`

El comando `bc`  *(basic calculator)* nos permite abrir una aplicación de una calculadora simple.

> Para cerrar la aplicación escribir `quit` 

### Comando `curl`

El comando `curl` nos permite hacer un request HTTP, es decir que visitaremos una página y veremos su contenido en pantalla.

```
curl https://google.com.ar
```

Si hacemos `curl ifconfig.me` podremos visitar esa página y nos devolverá la ip pública desde donde la estamos visitando es decir la el servidor.



Si en lugar de escribir los datos recibidos en *stdout* queremos descargar el contenido como un archivo podemos usar la opción `-O`

```
curl fredrikson.com.ar/resume.pdf -O
```



### Comando `wget`

El comando `wget` nos permite descargar un archivo de internet:

```
wget fredrikson.com.ar/resume.pdf
```

De la misma manera vista para `curl` podremos en este caso cambiarle el nombre al archivo descargado (en realidad [es un concepto mas complejo](https://www.gnu.org/software/wget/manual/html_node/Download-Options.html#Download-Options)) con `-O`.

```
wget fredrikson.com.ar/resume.pdf -O cv.pdf
```



### Comando `df`

El comando `df` muestra el espacio usado y libre en cada disco.



### Comando `column`

El comando `column` nos permitirá mostrar en columnas y un poco mas ordenada,  la información de ciertos comandos.

```
mount | column -t
ip link | column -t
```



## Manipulación de Archivos y Directorios

### Comando `mkdir`

El comando `mkdir` nos permite crear una carpeta. Por ejemplo si queremos crear una carpeta "test".

```bash
mkdir test
```



#### Crear árbol de directorios

Si tenemos que crear un directorio `/tmp/asia/india/bangalore` una forma sería:

```
mkdir /tmp/asia
mkdir /tmp/asia/india
mkdir /tmp/asia/india/bangalore
```

Sin embargo podemos hacer esto de un solo paso con:

```
mkdir -p /tmp/asia/india/bangalore
```



### Comando `mv`

El comando `mv` nos permite renombrar archivos y carpetas o moverlos de ubicación. 

Por ejemplo para renombrar un directorio:

```bash
mv test docker
```

Mientras que si queremos renombrar un archivo:

```
mv hello.txt hello-docker.txt
```



### Comando `cp`

El comando `cp` nos permite copiar archivos o directorios.

Si queremos duplicar un archivo en el mismo directorio:

```
cp file1.txt file1.txt.copia
```



Si queremos copiar un archivo a otro directorio (previamente creado)

```
cp file1.txt documents/
```

Con `ls documents/` podremos verificar que ahora tenemos un archivo `file1.txt`



Si queremos copiar un directorio debemos utilizar `cp -r` 

```
cp -r myMusic/ myMedia/
```



### Comando `rsync`

El comando `rsync` es útil para cuando tenemos que copiar gran cantidad archivos de un directorio a otro. Si bien esto podríamos hacerlo utilizando `cp`, con `rsync` tenemos la ventaja de que es capaz dedetectar si algunos de los archivos ya existen en el destino y en ese caos no copiarlos. Esto será útil para realizar backups o si anteriormente hemos efectuado una copia que falló y parte de esos archivos ya están en el destino. En caso de que utilizáramos `cp` deberíamos realizar la copia entera.

Suponiendo que tenemos un directorio `origen` con los archivos que queremos copiar y otro `destino`:

```bash
rsync -av origen/ destino/
```

Con `-v` indicamos que muestre la progresión en pantalla.



### Comando `touch`

El comando `touch` nos  permite crear archivos vacíos.

```bash
touch file.txt
```

> Utilizando el operador de redirección podemos hacer esto mismo con `> file.txt`. *La **salida** del primer comando tirala a `file.txt`. Como no tengo nada como primer comando inserto 0*
>
> Si el archivo `file.txt` ya existe le cambia la fecha de modificación a la actual.



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



### `cat` para ingresar contenido

Podemos usar `cat` junto con el `>` *redirection symbol* si queremos cargar contenido en un archivo.

```
cat > new_file.txt
```

A patir de este momento podremos ingresar contenido al archivo (incluso podremos colocar saltos de línea presionando `ENTER`) y cuando hayamos terminado presionamos `CTRL+d`.

> Este método no es viable para cuando tenemos que lidiar con grandes cantidades de texto, en esos casos debemos usar alternativas como Vi, Vim, Nano

Luego con `cat new_file.txt` veremos el contenido agregado.

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

El comando `head` nos permite mostrar las primeras diez líneas de un archivo.

```
head /etc/adduser.conf
```

Si queremos especificar la cantidad de líneas que queremos mostrar: `head -n 12 /etc/adduser.conf`



> A la hora de revisar logs en sistemas Ubuntu trabajaremos con `/var/log/syslog` por lo que probablemente nos será de utilidad el comando `tail /var/log/syslog`



### Comando `wc`

El comando `wc` significa word count y nos permite por ejemplo contar la cantidad de líneas devueltas por un comando cuya salida se la pasamos como entrada mediante un pipe.

```
head /var/log/syslog | wc -l
```

Cuando hacemos esto decimos *we piped the head command into the word count command*.



### Comando `tail`

El comando `tail` nos permite mostrar las últimas 10 líneas de un archivo.

```
tail /etc/adduser.conf
```

> Si queremos especificar la cantidad de líneas que queremos mostrar: `tail -n 10 /etc/adduser.conf` o `tail -2 /etc/adduser.conf`





El comando `tail` es capaz de recibir la opción `-f` con la cual haremos un seguimiento a los logs, es decir que veremos el contenido casi en tiempo real:

```
tail -f /var/log/syslog
```

> Veremos que al ejecutarlo no retorna al command prompt sino tenemos el cursor parpareando. En ese caso podremos ejecutar varios ENTER para así separar la última información de lo que venga después. Por ejemplo si ejecutamos `sudo systemctl restart ssh` veremos una serie de logs nuevos.



#### Comentario sobre Logs

* `/var/log/syslog`

En `syslog` contiene todos los logs del sistema todo menos los mensajes relacionados con temas de autenticación.

Este archivo puede tener miles de líneas lo cual nos lleva a tener una serie de precauciones por un lado para evitar que se llene (utilizando técnicas como log rotation y compresión de archivos) y también a la hora de trabajar con él no hacerlo directamente sino utilizando copias.

Si queremos conocer la cantidad de líneas que tiene este archivo:

```
cat /var/log/syslog | wc -l
```

Si queremos ver los primeros 10 logs que utilizan la palabra ssh:

```
cat /var/log/syslog | grep ssh | head
```

> Podremos la cantidad recibida con `cat /var/log/syslog | grep ssh | head | wc -l`



* `/var/log/auth.log`

En `auth.log` vemos los intentos de logueo que han tenido los usuarios. Para simular esta situación en la que un usuario que intenta loguearse, lo hacemos de la misma máquina pero simularía que es otro host, ejecutamos:

```
ssh localhost
```


Si a continuación ingresamos mal el password se agregarán nuevos logs reportando esta situación.



### Comando `rm`

El comando `rm` nos permite borrar archivos y directorios.

Por ejemplo para eliminar un archivo: 

```bash
rm file.txt
```



#### Eliminar varios archivos

También es posible borrar más de un archivo a la vez:

```bash
rm file1.txt file2.txt file3.txt
```



#### Eliminar según patrón

O bien podemos borrar todos los archivos que tengan un cierto patrón, por ejemplo todos los archivos que comiencen con un cierto nombre:

```bash
rm file*
```



#### Eliminar directorio

Para eliminar un directorio debemos hacerlo con `rm -r test` con lo cual eliminaremos la carpeta y todo su contenido recursivamente.

> En ocasiones también tendremos que forzar el borrado lo cual hacemos con `rm -rf test` (recursivo y forzado, es decir sin preguntarnos)



### Comando `truncate`

El comando `truncate` nos permitirá truncar un archivo. Esto puede ser útil por ejemplo si tenemos un log muy largo y queremos limpiarlo. Si bien podríamos eliminarlo y confiar en que el servicio vuelva a crearlo en ocasiones puede que sea más seguro hacer esto para evitar pérdida de logs.

```
truncate -s 0 hello.txt
```

> Con `-s 0` hacemos referencia al tamaño del archivo resultante.



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



Si queremos crear un script que muestre este mensaje en pantalla:

```bash
echo echo Hello > deploy.sh
```



También podremos generar un archivo a partir de una lista de archivos:

```bash
ls -l /etc > list.txt
```



> De la misma manera que tenemos el operador de redirección del *standard output* `>` tenemos el operador de redirección del standard input `<`.



## Búsqueda de Texto

### Comando `grep`

El comando `grep` (*global regular expression print*) nos permite buscar texto en un archivo, por ejemplo si queremos buscar la cadena "hello" en el archivo `file.txt`

```bash
grep hello file.txt
```



##### Invert Match

En lugar de obtener todas las líneas que tienen una determinada cadena, podemos obtener las que no la tienen.

```
grep -v hello file.txt
```

En lugar de `-v` podemos usar `--invert-match`



##### Ignore Case

Como todo en Linux, la búsqueda es case sensitive, para hacerlo case insentitive debemos agregar la opción `-i`

```
grep -i hello file.txt
```

En lugar de `-i` podremos usar  `--ignore-case`.



##### Obtener Número de Línea

Utilizando la opción `-n`

```
grep -n hello file.txt
```

En lugar de `-n` podremos usar  `--line-number`.



##### Contar líneas

Con la opción `-c` podremos contar la **cantidad de líneas** (no de ocurrencias) en que encontramos la cadena indicada.

```
grep -c hello file.txt
```

> No importa que la palabra esté n veces en una misma línea, veremos como resultado 1.



#####  Búsqueda en múltiples archivos

La forma más simple de buscar en múltiples archivos es especificándolos uno a uno.

```bash
grep -i hello file1.txt file2.txt
```



Si queremos buscar en todos los archivos en la ruta en la que nos encontramos actualmente:

```
grep hello *
```



Otra forma es especificarlos mediante un patrón:

```bash
grep -i hello file*
```



##### Búsqueda recursiva

Utilizando la opción `-r` podemos buscar texto en los archivos de un determinado directorio (y todos sus subdirectorios)

```bash
grep -r hello /etc
```

En lugar de `-r` podemos usar `--recursive`



Otra opción sería `grep -r hello .` si queremos buscar en todos los directorios a partir de nuestra posición.



##### Búsqueda recursiva incluyendo sólo ciertos archivos

Si queremos buscar recursivamente dentro de un directorio pero sólo estamos interesados en determinado tipo de archivos:

```
grep -r --include="*.txt" hello . 
```



##### Filtrar Archivos

Utilizando la opción `-l` (junto con `-r`) es posible filtrar todos los archivos de un directorio que contienen una determinada cadena.

```
grep -rl hello .
```

En lugar de `-l` podemos usar `--files-with-matches`



##### Cantidad de Líneas devueltas

```
grep -A 5 hello file.txt
```

Con `-A 5` (de after) mostramos las 5 líneas siguientes a la aparición de la cadena "hello". 

Con `-B 5` (de before) mostramos las 5 líneas previas a la aparición de la cadena "hello". 

Con `-C 5` mostramos las 5 líneas previas y las 5 siguientes a la aparición de la cadena "hello". 



##### Utilizando Pipes

Si bien es un poco redundante, en ocasiones se suele utilizar el comando `cat` en conjunto con `grep` utilizando pipes.

Por ejemplo si queremos buscar los scripts que tenemos en un determinado proyecto con Node.js podemos hacerlo con

```
cat package.json | grep scripts
```

:sunglasses: Aunque podríamos haber hecho directamente `grep scripts package.json`



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



* Si queremos filtrar todos los archivos cuyo nombre comienza con "f" agregamos la opción `-name 'f*'`

```
find -type f -name "f*"
```



* Si queremos filtrar los archivos cuyo nombre comienza con "f" o con "f" agregamos la opción `-iname "f*"`

```
find -type f -iname "f*"
```



* Si queremos filtrar desde el root todos los archivos `.py` y grabar un archivo de texto con ese listado

```
find / -type f -name "*.py" > python-files.txt
```

> Notar que en la búsqueda queremos comenzar por el *root directory*.



* Si queremos buscar los archivos en el directorio actual con fecha de modificación mayor a 5 días:

```
find . -mtime + 5
```



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



Es posible encadenar el uso de pipes, por ejemplo si tenemos un archivo `readme.txt` podríamos hacer:

```bash
cat readme.txt | grep hola | grep juan
```

Es decir que estamos filtrando en primer lugar el contenido del archivo y luego filtrando nuevamente lo obtenido luego del primer filtro.

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



### Comando `export`

El comando `export` nos permite setear una variable de entorno en la sesión actual de la terminal, por lo que si cerramos y abrimos una nueva, no existirá.

```
export DB_USER=mosh
```

> Si estamos corriendo un Linux con Docker podremos cerrar la sesión actual de la terminal con `exit` y con `docker ps -a` veremos el proceso detenido. Si queremos reiniciarlo podemos hacerlo con `docker start -i 2f7` siendo `2f7` las primeras tres letras del contenedor.



Para setear una variable de entorno de manera persistente debemos hacerlo en `.bashrc` que encontramos en el directorio home del usuario, es decir  `/root` o `/home/foo` para el usuario `foo`.

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



> Si queremos mostrar los procesos padres e hijos vinculados en forma de árbol podemos utilizar el comando `ps` con estos tres argumentos:

```
ps fax
```



### Comandos `bg` y `fg`

Por ejemplo si ejecutamos `sleep 100` durante 100 segundos estará el prompt durmiendo y luego de ese tiempo volverá a estar activo. 

Si queremos lanzar la ejecución del proceso en segundo plano para poder ejecutar otros comandos mientras tanto, ejecutamos `sleep 100 &`. Si ejecutamos `ps` veremos ese proceso ya que en la columna `CMD` dirá `sleep`.

Otra forma de hacer esto es ingresar `sleep 1m`, luego `CONTROL+Z` pausamos la ejecución del proceso. 

* Con el comando `bg` enviaremos el proceso pausado a segundo plano (similar a ejectarlo con `&` al final, dejando el terminal libre.
* Con el comando `fg` lanzamos el proceso pausado a primer plano (monopolizando el terminal).

> Un caso útil de aplicación es cuando estamos editando un archivo por ejemplo con vim y generamos unos cambios pero no estamos listos para guardarlo todavía. En ese caso podemos presionar `CONTROL+Z` hacer lo que tengamos que hacer y luego volver a la edición con `fg` (lo queremos lanzar a primer plano)



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



### Comando `killall`

El comando `killall` nos permite matar todos los procesos que tengan en su nombre una cierta cadena de texto

```
killall cadena
```



### Comando `top`

El comando `top` nos permite ver una lista de procesos que están corriendo en el servidor ordenados por el uso de CPU y memoria.

> Con la tecla  `?` accederemos a una pantalla que nos muestra todos los atajos de teclado.
>
> Con la tecla `q` salimos



### Comando `htop`

El comando `htop` es un visor de procesos similar al comando `top` pero más visual.

> Si queremos pausar este programa o cualquier otro que esté en el frente (como por ejemplo vim cuando estamos editando un archivo y no queremos guararlo ni descartar los cambios) podemos hacerlo con `CONTROL+Z` y luego con `fg` lo traemos al frente nuevamente.



## Manejo de Usuarios

## Privilegios `sudo`

Cuando se trata de realizar tareas no todos los usuarios pueden realizar todas ellas. Todos los sistemas Linux tienen un **usuario root**, que no tiene ninguna restricción en cuanto a sus tareas. En cambio los **usuarios regulares** tienen restringidas sus acciones sobre el sistema. Si queremos que un usuario normal realice tareas privilegiadas como instalar y configurar software o servicios, el root user puede hacer esto posible asignándole permisos de sudo. Ese usuario será agregado al archivo `/etc/sudoers`. Cuando un usuario regular (que tiene privilegios de sudo) quiere elevar su nivel de permisos puede agregar el prefijo sudo al comando que desea ejecutar.

> En la mayoría de los entornos de producción se restringe el acceso al usuario root y casi nunca nos vamos a loguear con ellos, justamente por las capacidades que este tiene y la posibilidad de cometer algún error.

### Comando `su`

El comando `su` (switch user) nos permite pasar de un usuario a otro.

`su -` 

- logs you into root with the root environment
- (as opposed to `su` which logs you into root with your environment)

And I understand what `sudo` does

- you are root for one command



`su` requires the root password and `sudo` requires your user password

Therefore `sudo su -` will put you into a root environment but it will ask you for your user password instead of the root password (once sudo has given you root privileges, `su -` can be executed with no password).

Luego con `CTRL+D` nos desconectaremos de ese usuario.



### Comando `useradd`

El comando `useradd` nos permite agregar un nuevo usuario  

Si ingresamos `useradd` veremos las opciones con las cuales podemos utilizar este comando. 

> Veremos que existe una forma corta (con un guion) y una forma larga (con dos guiones) de ingresar estas opciones, por ejemplo podremos crear un usuario junto con su directorio home con `useradd -m nombre` o con `useradd --create-home nombre`



Si queremos crear un usuario john y su directorio home:

```
useradd -m john
```



Podremos constatar la creación de este usuario revisando el archivo de configuración que contiene información de las cuentas de usuario: `cat /etc/passwd`, que nos entregará entre otras cosas lo siguiente:

```
john:x:1000:1000::/home/john:/bin/sh
```

Veremos que tenemos múltiples campos separados por `:`.

Primero tenemos `john` que es el nombre de usuario, luego `x` que significa que el password está almacenado en otro lugar, luego `1000` es el userId, luego `1000` es el groupId, luego `/home/john` es el directorio home de este usuario y por último `/bin/sh` es el programa Shell (el original, que luego fuera mejorado por Bash) que será utilizado cuando el usuario se loguea. 



### Comando `usermod`

:vhs: Basado en el video de [LearnLinuxTV](https://youtu.be/p8QOnty6rSU?list=PLT98CRl2KxKHKd_tH3ssq0HPrThx2hESW)

El comando `usermod` nos permite modificar un usuario existente.

#### Agregar usuario a grupo

Si queremos agregar un usuario a un grupo, por ejemplo el user `juan` al grupo `admins`.

```
sudo usermod -aG admins juan
```

En Ubuntu para utilizar `sudo` debemos ser miembros del grupo sudo. En caso de que no lo seamos nos aparecerá un mensaje: **juan is not in the sudoers file. This incident will be reported**. 

> Además de ser miembros del grupo `sudo` para ejecutar el comando, **el grupo en cuestión debe existir** para que podamos agregar un usuario a el. En caso de que no exista debemos crearlo primero con el comando `groupadd`.

En ese caso debemos desconectarnos del servidor y loguearnos como root y ejecutar:

```
usermod -aG sudo juan
```

> Notar que no usamos sudo porque estamos logueados como `root`.

Luego podremos verificar que los cambios fueron realizados con:

```
groups juan
```

A pesar de que nos aparezca el grupo nuevo en el listado para que entre en efecto la nueva membresía debemos desloguearnos y loguearnos nuevamente.

#### Agregar usuario a mas de un grupo

```
sudo usermod -aG admins,developers juan
```



#### Utilizar Bash en lugar de Shell

Si queremos que al loguearnos en lugar de utilizar Shell utilicemos Bash debemos usar `usermod -s /bin/xxx usuario` o `usermod --shell /bin/xxx usuario`.

```
usermod -s /bin/bash john
```

> Con `cat /etc/passwd` veremos que los cambios fueron aplicados.

> :lock: Los **passwords** son almacenados encriptados en `/etc/shadow` archivo que podremos abrir con `cat /etc/shadow` pero sólo desde el **root** user.



#### Cambiar home directory

Si queremos cambiar el directorio home de modo que este pase a ser `/home/myhome` (no es necesario que este directorio exista con anterioridad) podemos utilizar el comando `usermod` con la opción `-d`.

No podemos hacerlo desde el mismo usuario con el que estamos logueados, suponemos a continuación que lo hacemos con `root`, por eso omitimos `sudo`

```
usermod -d /home/myhome juan
```

Si queremos mover el contenido del directorio actual al nuevo directorio:

```
usermod -d /home/myhome --move-home juan
```

Para verificar que el cambio fue aplicado cuando nos logueamos nuevamente:

* Si estamos en `~` si ejecutamos `pwd`.

* ```
  echo $HOME
  ```

  En ambos casos deberíamos ver el nuevo directorio.



#### Cambiar nombre de usuario

Es posible cambiarle el nombre a un usuario. En este caso lo haremos logueados como `root`:

```
usermod -l juan2 juan
```

Esto podremos verificarlo de esta forma:

```
cat /usr/passwd
```



#### Bloquear Usuario

Si queremos que un usuario no pueda ingresar, desde `root` podemos hacer lo siguiente:

```
usermod -L juan
```



#### Desbloquear Usuario

```
usermod -U juan
```



#### Programar Deshabilitación de Cuenta

Es posible establecer la fecha en la cual una cuenta será deshabilitada.

```
usermod -e "2022-03-21" juan 
```

Esto podremos verificarlo con:

```
chage -l juan
```



Para quitar la fecha de expiración

```
sudo usermod -e "" juan
```

Las fechas de expiración son almacenadas en el archivo  [`/etc/shadow`](https://linuxize.com/post/etc-shadow-file/) 



### Comando `userdel`

El comando `userdel` nos permite eliminar un usuario.

```bash
userdel john
```



### Comando `adduser`

Hasta ahora estuvimos trabajando con `useradd` que es la API original (todos los comandos comienzan con `user`), mientras que `adduser` es un *script de perl* más interactivo que usa `useradd` *under the hood*. 

Este comando debemos ejecutarlo desde root.

Si ejecutamos `adduser bob` veremos en pantalla que se crea el usuario `bob`, luego se crea un grupo `bob` y por último se inserta el usuario a ese grupo. También creará un directorio para dicho usuario `/home/bob`, copiará archivos de configuración y nos dará la posibilidad de ingresar un password y otros datos del usuario. 

Decimos que es más interactivo por todos los datos que nos solicita y como con Docker normalmente vamos a deployar nuestras aplicaciones no vamos a querer hacer uso de dicha interactividad y usaremos `useradd`.



### Comando `getent`

El comando `getent` nos permite obtener un listado de todos los usuarios que forman parte de un grupo.

```
getent group sudo
```



## Manejo de Grupos

El uso de grupos posibilita que todos los miembros de dichos grupos tengan los mismos permisos.

Cada usuario de Linux tiene un **grupo primario** (creado automáticamente al crear el usuario, con el mismo nombre del usuario) y opcionalmente puede tener uno o más **grupos secundarios**. 

Como cada archivo tiene un dueño y un grupo, si no existiera este grupo primario y un usuario miembro de varios grupos crea un archivo, sería imposible saber a qué grupo pertenece el archivo. 

De manera similar a lo visto para el manejo de usuarios, para el manejo de grupos tenemos tres comandos `groupadd`, `groupmod`y `groupdel`.

### Comando `groupadd`

El comando `groupadd`nos permite crear un grupo.

```bash
groupadd developers
```

Los grupos son almacenados en un archivo que podremos visualizar con `cat /etc/group` que nos entregará una lista donde veremos `developers:x:1001:` siendo `1001` el id del grupo.



Si queremos agregar un usuario a un grupo podemos hacerlo con `usermod` con `-G` para grupos suplementarios o con `-g` para establecerle el grupo primario.

Por ejemplo si queremos agregar el usuario `john` al grupo `developers` (desde el usuario `root`)

```bash
usermod -G developers john
```



Si revisamos en el archivo `/etc/passwd` lo relativo al usuario `john` como hemos cambiado los grupos secundarios y allí sólo almacenamos el primario, no veremos nada nuevo:

* `cat /etc/passwd | grep john`
* `grep john /etc/passwd`



### Comando `groups`

El comando `groups` nos permite conocer los grupos a los que pertenece un usuario (tanto primario como secundarios) 

```bash
groups john
```



### Comando `groupmod`

El comando `groupmod`nos permite modificar un grupo.

### Comando `groupdel`

El comando `groupdel`nos permite eliminar un grupo.



## Shell Scrips

Los archivos terminados con `.sh` reciben el nombre de Shell Scripts y en ellos podemos ingresar cualquiera de los comandos estudiados hasta ahora.

Por ejemplo si desde `/home` escribimos`echo echo Hello > deploy.sh` **(está bien tener dos veces seguidas el comando echo)**, vamos a crear un archivo `deploy.sh` y adentro de el dirá `echo Hello`

Para ejecutar el script debemos ingresar `./deploy.sh` pero nos aparecerá el mensaje:

```bash
bash: ./deploy.sh: Permission denied
```



## Permisos de Archivos

Si queremos obtener los permisos de un archivo debemos listarlos de manera larga con `ls -l`.

Suponiendo que estamos en `/home`  en la primera columna veremos:

```bash
-rw-r--r-- 1 root root   11 Aug 10 17:23 deploy.sh
drwxr-xr-x 2 john john 4096 Aug  9 11:40 john
```

* Si la primera letra es una `d` es un directorio y si es un guión `-` es un archivo.
* Luego tenemos 9 letras divididas en 3 grupos de 3 letras. En cada grupo tenemos permisos de lectura `r`, de escritura `w` y de ejecución `x`.  Si tenemos `rw-` significa que tenemos permiso de lectura, de escritura pero no de ejecución.  Los directorios por default tienen permiso de ejecución para poder dirigirnos a ellos utilizando el **comando** `cd`, es decir `cd directorio`.
* El primer grupo de letras representa los permisos para el usuario dueño del archivo, el segundo para el grupo dueño del archivo y el tercero para todos los demás.



### Comando `chmod`

El comando `chmod`nos permite cambiar los permisos de un archivo desde el usuario que es dueño del archivo.

Podemos cambiar los permisos relativos al usuario `u`, al grupo `g` o a todos los demás `o`. 

Cuando creamos un archivo los permisos que tiene inicialmente son son `-rw-r--r--`. Si queremos agregarle al usuario permisos de ejecución:

```
chmod u+x deploy.sh
```

> Si quisiéramos en cambio eliminar los permisos de ejecución debemos ejecutar `chmod u-x deploy.sh`

Ahora veremos que si hacemos `ls -l` el archivo cambió a color verde pues ahora es ejecutable y que en el primer grupo tenemos la `x` de ejecutable. Veremos que los permisos de este archivo pasaron a ser `-rwxr--r--` es decir que ahora es ejecutable pero sólo por este usuario.



> Si intentamos ejecutar este script desde otro usuario obtendremos **Permission denied**.  Si quisiéramos que cualquiera lo pueda ejecutar deberíamos correr `chmod o+x deploy.sh`



* Para aplicar permisos a mas de un grupo de usuarios

```
chmod og+x deploy.sh
```



* Para aplicar permisos a más de un grupo y agregar o quitar más de un tipo de permisos:

```
chmod og+x+w-r deploy.sh
```

En este caso por ejemplo estaremos agregando el permiso de ejecución, el de escritura y quitando el de lectura para los miembros del grupo y para el resto de los  usuarios.



* Para aplicar permisos a más de un archivo a la vez 

```
chmod u+x file1.sh file2.sh
```



* Para aplicar permisos a todos los archivos que cumplan un cierto patrón

```
chmod u+x *.sh
```



## Creación de Alias

### Comando `alias`

En muchas oportunidades debemos utilizar el mismo comando muchas veces  e ingresarlo manualmente reduce nuestra productividad y nos distrae de lo que verdaderamente estamos haciendo. Con el comando `alias`  es posible crear un alias para escribir más rápido esos comandos más utilizados.

* Si queremos ver los alias que tenemos actualmente definidos en nuestro perfil almacenados en `.bashrc` que tenemos en `~`:

```
alias
```

> Como parte de la salida veremos `alias ll='ls -alF'` lo cual significa que ingresar `ll` es equivalente a ingresar `ls -alF`



* Si queremos crear un alias temporario (que solo esté disponible mientras dure la sesión)

```
alias nombre_alias="comando"
```

* Si queremos crear un alias permanente debemos guardarlo en `.bashrc`

```
#My custom aliases
alias home=”ssh -i ~/.ssh/mykep.pem tecmint@192.168.0.100”
alias ll="ls -alF"
```

Luego debemos guardar el archivo y será automáticamente cargado en la próxima sesión, mientras que si queremos utilizar los alias en la sesión actual debemos ejecutar `source ~/.bashrc`



### Comando `unalias`

El comando `unalias` nos permite eliminar un alias agregado mediante la línea de comandos:

```bash
unalias nombre_alias
```



## Comandos Avanzados

### Comando `netstat`

> Basado en https://www.youtube.com/watch?v=8UZFpCQeXnM

> No lo pude probar con el contenedor con la imagen de Alpine Linux por dificultades de instalación en red corporativa.

El comando `netstat` *(network statistics)* nos permite conocer las conexiones de red actuales y la actividad a nivel puertos en el servidor. Nos permitirá saber qué servicios están corriendo y en qué puertos.

```
netstat
```



> Si queremos conocer las opciones con las cuales podemos usar este comando ejecutar `netstat -?`



Por ejemplo si tenemos abiertas las páginas web yahoo.com y example.com, un ftp y estamos conectados a dos máquinas en la red local veremos:

| Proto | Local Address       | Foreign Address    | State       |
| ----- | ------------------- | ------------------ | ----------- |
| TCP   | 192.168.0.12: 52913 | 74.125.44.25:https | ESTABLISHED |
| TCP   | 192.168.0.12: 62976 | 22.134.45.78:https | ESTABLISHED |
| TCP   | 192.168.0.12: 63510 | 42.33.44.55:ftp    | ESTABLISHED |
| TCP   | 192.168.0.12: 63562 | BobPC:445          | ESTABLISHED |
| TCP   | 192.168.0.12: 63037 | MikePC:65137       | ESTABLISHED |

> Si no estamos conectados a nada obtendremos una salida en blanco, mientras que si estamos conectados veremos una tabla con los siguientes datos: Proto, Local Address, Foreign Address, State. 

> Debemos tener presente que **la salida no será instantánea ya que está utilizando DNS para resolver números a nombres**. Por ejemplo para mostrar los nombres de las computadoras BobPC y MikePC a las cuales estamos conectados.



* Si queremos mostrar sólo números y no nombres, y obtener de esta manera una salida instantánea podemos ejecutar:

```
netstat -n
```



| Proto | Local Address       | Foreign Address        | State       |
| ----- | ------------------- | ---------------------- | ----------- |
| TCP   | 192.168.0.12: 52913 | 74.125.44.25:**443**   | ESTABLISHED |
| TCP   | 192.168.0.12: 62976 | 22.134.45.78:**443**   | ESTABLISHED |
| TCP   | 192.168.0.12: 63510 | 42.33.44.55:**21**     | ESTABLISHED |
| TCP   | 192.168.0.12: 63562 | **192.168.0.13**:445   | ESTABLISHED |
| TCP   | 192.168.0.12: 63037 | **192.168.0.14**:65137 | ESTABLISHED |



* Si queremos ver no sólo las conexiones actuales sino también los puertos TCP y UDP que están escuchando conexiones:

```
netstat -a
```



La dirección IP con todos ceros  `0.0.0.0` es nuestra propia computadora y esto hace referencia que el puerto no está escuchando en una dirección IP específica sino en todas las disponibles en todas 

```
TCP    0.0.0.0:135            DESKTOP-6FT7863:0      LISTENING
```



* Si queremos conocer qué programa está haciendo esas conexiones

```
netstat -b
```



Si queremos conocer el nombre completo de los dominios a los cuales estamos conectados:

```
netstat -f
```

 

* Si queremos ver nuestra tablas de ruteo del servidor

```
neststat -nr
```



### Comando `cmatrix`

Si ejecutamos `cmatrix` veremos un fondo de pantalla con caracteres estilo Matrix.



### Comando `tldr`

El comando `tldr` nos permite obtener ejemplos prácticos de cómo utilizar otros comandos.

```
sudo apt intall tldr
```

Luego si queremos obtener ejemplos sobre cómo utilizar el comando `find` podríamos poner:

```
tldr find
```



### Comando `ping`

El comando `ping` nos permite saber si un servidor se encuentra online y en caso de estarlo responderá con un nuevo ping.

Es posible hacerle un ping a un host name, domain name, ip, etc

```
ping google.com
```



##### Tráfico de Red

El tipo de tráfico de red que vemos con este comando se conoce como **icmp** (internet control message protocol).  Le estamos enviando un *icmp echo request* a la espera de que nos envíe un *icmp echo reply*.

Cuando un administrador implementa un firewall puede bloquear el tráfico icmp.



##### Cantidad de Pings

A diferencia de lo que ocurre en Windows donde al realizar el ping obtenemos cuatro pings y luego se detiene, en Linux por defecto continúa ejecutándose permanentemente por lo que podremos detenerlo con `CTRL+C`.

Sin embargo, esto puede configurarse utilizando la opción `-c`.

```
ping -c 3 google.com
```



##### Network Troubleshooting

Podemos pensar al comando `ping` como una utilidad básica de solución de problemas, ya que aporta datos como el tiempo que transcurrió entre la solicitud hasta obtener una respuesta (lo cual puede ser indicador de latencia), la cantidad de paquetes que se pierden, etc. Si bien estos datos no nos darán la causa en caso de tener un problema de red, pueden servirnos para tener una noción del estado de la misma.



###### Determinar Conexión a Internet

Es posible utilizar el comando `ping` para saber si tenemos conexión a internet en especial en servidores donde no contamos con un navegador (o aun teniendo un navegador puede que sea problema de DNS)

```
ping 8.8.8.8
```



Siendo `8.8.8.8` uno de los servidores de DNS de Google por lo que si obtenemos respuesta significará que estamos online. En caso de tener respuesta y continuar con problemas podrá deberse a problemas con el servidor de  dns local , servidor dhcp, etc



##### Reinicio Servidor

Cuando realizamos tareas de mantenimiento en un servidor y tenemos que reiniciarlo habrá un tiempo durante el cual no podremos conectarnos por ssh. Si bien podríamos intentar conectarnos hasta que eventualmente nos deje, otra forma es hacer ping a la ip del server sabiendo que cuando este vuelva a estar operativo obtendremos respuesta y ahí sí podremos conectarnos por ssh.



## Comando `awk`

El comando `awk` es una herramienta poderosa para el procesamiento de texto, su nombre proviene de los apellidos de sus creadores.



* Si queremos mostrar determinadas columnas:

  ```
  awk '{print $2, $4}' file.csv
  ```

  

* Si queremos mostrar un título inicial y luego imprimir el contenido de un archivo:

  ```
  awk 'BEGIN {print "Nombre\tApellido\tEdad\n"} {print}' file.csv
  ```

  Siendo `BEGIN` un bloque que se ejecuta al inicio del programa



* Si queremos conocer el número de líneas de un archivo:

  ```
  awk 'END {print NR}' file.csv
  ```

  Siendo `END` un bloque que se ejecuta al final del programa y `NR` el *number of records*.



* Si queremos mostrar el promedio de los valores de una determinada columna:

  ```
  awk '{sum+=$3} END {print "Promedio:" sum/NR}' file.csv
  ```



* Si queremos mostrar las filas que tengan una columna con valor mayor a un umbral.

  ```
  awk '$4>70' file.csv
  ```

  > Nota: Utilizar este comando en Linux directamente ya que si bien con Cmder en Windows, awk funciona para algunas cosas no funciona en esta línea (por el > con el que termina cerando un archivo) y tira error.



* Si queremos cambiar una palabra que aparece en el archivo:

  ```
  awk '{gsub(/Ariel/, "Burrito"); print}' file.csv
  ```





* 
* `NF` number of fields.

# Servicios

Un servicio de Linux es un programa que se ejecuta de fondo (como por ejemplo en web servers, database servers, Docker, etc) 

Cuando instalamos software que funciona de fondo es configurado automáticamente como un servicio y vamos a querer asegurarnos que esté corriendo y también que siga corriendo cuando el servidor se resetee.

Para iniciar este servicio ejecutamos:

```
service apache2 start
```

> Existe una forma más moderna de hacer esto utilizando `systemctl` en servidores que utilizan `systemd` para manejar sus servicios.
>
>  (notar que el start viene antes del nombre del servicio)
>
> ```
> systemctl start apache2 
> ```



## `systemd`

> Basado en el [video](https://www.youtube.com/watch?v=5JVBpXiYMKo)

**Systemd** nos permitirá manejar los servicios de nuestro servidor Linux pudiendo iniciarlos, detenerlos o reiniciarlos según sea el caso. Veremos el comando `systemctl` que es una utilidad de systemd.

>  El término servicio, unidad o daemons se utilizará como sinónimo.



Como necesitamos un servicio de ejemplo para manejar con `systemd` instalamos Apache.

```
sudo apt install apache2
```

:family: Este comando está presente en múltiples distribuciones CentOS, Debian, Ubuntu, Fedora, ArchLinux, etc.

Luego podremos conectarnos a `http://localhost` (no https porque no hemos añadido un certificado) o a la dirección IP en caso de que sea un servidor Linux y verificar si está funcionando. En algunas distribuciones comienzan el proceso automáticamente.

En ocasiones debemos configurar además el firewall para poder acceder a la página de muestra del servidor web.



### Comando `systemctl`

El comando `systemctl` es una utilidad de `systemd` responsable de controlar el sistema systemd en sí y el manejador de servicios.

Si ejecutamos `systemctl` veremos una larga lista con todos los servicios ejecutándose en *background*.

```
systemctl
```



#### Estado Servicio

Si queremos obtener el estado de un servicio en particular, utilizamos el nombre del paquete con el cual lo instalamos:

```
systemctl status apache2
```



Si una unidad nos aparece como **disabled e inactive**. Al estar **disabled** no iniciará automáticamente cuando la máquina bootee, lo que haría necesario que lo iniciemos manualmente cada vez. 

 

#### Habilitar Servicio

Para configurar este servicio para que arranque al iniciar.

```
sudo systemctl enable apache2
```

Si ahora verificamos el estado veremos que nos aparece como loaded pero continúa **inactive**. Si reiniciáramos el sistema iniciaría automáticamente.



#### Iniciar Servicio

Para iniciar este servicio:

```
sudo systemctl start apache2
```



> Notar que no necesitamos `sudo` para ver el estado de un servicio pero en algunos casos puede que nos muestre mas información si lo hacemos con `sudo`.



#### Habilitar e Iniciar Servicio

Si queremos habilitarlo e iniciar el servicio a la vez:

```
sudo systemctl enable --now apache2
```



#### Reiniciar Servicio

Cuando cambiamos la configuración muchas veces necesitaremos reiniciar la unidad para que tome los cambios.

```
sudo systemctl restart apache2
```



#### Recargar Configuración

Cuando reiniciamos el servicio este será detenido y luego iniciará nuevamente, en ocasiones puede que esto no sea necesario y que baste con recargar el servicio. **No todos los cambios en la configuración admiten esto** pero es aconsejable probar, ya que en caso de tratarse de un servidor web los clientes serán desconectados si reiniciamos.

```
sudo systemctl reload apache2
```



#### Detener Servicio

Si queremos detener un servicio

```
sudo systemctl stop apache2
```



#### Deshabilitar Servicio

```
sudo systemctl disable apache2
```



#### Servicios para un usuario

En ocasiones vamos a querer ejecutar un servicio sólo para un usuario en particular.

Utilizaremos como ejemplo el paquete **Sync** que es utilizado para sincronizar archivos entre dos o más computadoras.

```
sudo apt install syncthing
```



Para obtener el estado para un usuario en particular:

```
systemctl status syncthing@juan
```

Veremos que nos aparece como **disabled** e **inactive**



Para habilitarlo

```
sudo systemctl enable syncthing@juan
```

Para iniciar

```
sudo systemctl start syncthing@juan
```

Aunque podríamos haber hecho ambas cosas en un paso con `--now` como vimos anteriormente.

Este paquete trabaja con el puerto 8384 por lo que ingresando a `localhost:8384` podríamos ver una interfaz web (sin embargo esto puede que no esté disponible si estamos trabajando con un servidor).



#### Información Servicios

Podremos encontrar información sobre los servicios en:

```
cd /usr/lib/systemd/system
```

```
nano syncthing@.service
```



#### Listar Servicios

Vimos que con `systemctl` podemos listar los servicios.

También podemos hacerlo de manera mas clara con:

```
systemctl list-unit-files --type=service
```

En la primera columna veremos el nombre del servicio, en la segunda si está habilitado o no y en la tercera el vendor preset que indicará si el servicio debe iniciarse automáticamente una vez instalado.



## Configurar Programa como Servicio

Es posible configurar un programa propio (por ejemplo un servidor web que al visitarlo nos muestre un mensaje de bienvenida) o software de terceros como servicio.

Esto nos permitiría trabajar del modo que lo hacemos con los servicios:

```
systemctl start my_app
```

```
systemctl stop my_app
```

Como con `systemctl` podemos manejar los servicios `systemd` debemos configurar nuestro programa como un servicio de ese tipo. Esto lo hacemos creando un archivo de configuración en  la ruta `/etc/systemd/system` y le damos el nombre que queremos que tenga el servicio por ejemplo `my_app.service` y dentro del archivo creamos:

```
[Unit]
Description=My pyrhon web application

[Service]
ExecStart=/usr/bin/python3 /opt/code/my_app.py
```

Luego para indicarle a systemd que hay un nuevo servicio, debemos ejecutar:

```
systemctl daemon-reload
```

```
systemctl start my_app 
```

```
systemctl stop my_app
```

```
systemctl status my_app
```



### Inicio Automático

Si queremos que se inicie automáticamente cuando arranca el sistema:

```
[Service]
ExecStart=/usr/bin/python3 /opt/code/my_app.py

[Install]
WantedBy=multi-user.target
```

Con esto le decimos el orden en que queremos que se ejecute, es decir después de qué servicio y eso es lo que le estamos diciendo con `multi-user.target`

Ahora sí podremos ejecutar

```
systemctl enable my_app
```

de esta manera se iniciará automáticamente al reiniciar.



### Otras configuraciones:

* Si queremos incorporar una descripción del servicio.

```
[Unit]
Description=My pyrhon web application
```



* Si nuestra aplicación tiene otras dependencias y tenemos que ejecutar scripts o comandos antes o después del inicio de la aplicación, podemos indicarlos de esta forma:

```
[Service]
ExecStart=/usr/bin/python3 /opt/code/my_app.py
ExecStartPre=/opt/code/configure_db.sh
ExecStartPost=/opt/code/email_status.sh
```



* Si queremos que la aplicación reinicie automáticamente en caso de crasheo:

```
[Service]
ExecStart=/usr/bin/python3 /opt/code/my_app.py
Restart=always
```



* Si queremos ver un ejemplo real de estos archivos podemos analizar `/lib/systemd/system/docker.service`
