# Cron Jobs

Los Cron jobs son tareas programables que queremos que se ejecuten a una determinada hora y con una determinada periodicidad.

Cada usuario tendrá su propio set de cron jobs y la lista de estas tareas para un usuario es considerado un **crontab**, por lo tanto para listar todas las tareas de ese usuario:

```
crontab -l
```



Para agregar una tarea:

```
crontab -e
```

Luego nos dará a elegir el editor que queremos usar (1 para usar nano). Esta elección permanecerá para ese usuario.

Editaremos un archivo temporal y en caso de que todo está bien se copiará al crontab de este usuario.



## Crear Cron Jobs en otro Usuario

La mayoría de las veces no vamos a querer ejecutar las cron jobs como nuestro usuario, sino que vamos a querer dividirlas en una cuenta especial para tal propósito. Por ejemplo si tenemos un servidor web nginx o Apache, en Ubuntu o Debian contamos con el usuario `www-data` que podemos usar si tenemos que hacer una tarea en el website.

Para editar el crontab para un usuario diferente `crontab -u root -e` y nos dirá que para ello debemos estar logueados como `root` o tener acceso a `sudo`.

```
sudo crontab -u root -e
```

A continuación veremos cómo confeccionar las cron expressions, pero si queremos que algo se ejecute en cada minuto ponemos:

```
* * * * * apt install -y tmux
```

Luego con `CTRL+O` y `ENTER` guardamos y con `CTRL+X` salimos. Con `date` podremos ver la hora sabiendo que cuando cambie el minuto se ejecutará ese comando. 



## Verificar Ejecución

Con `which tmux` nos damos cuenta que primero no está instalado y pasando un minuto pasa a estarlo. Obviamente este no es el mejor ejemplo ya que el paquete sólo queremos instalarlo una vez.



Podemos verificar si se ejecutó esta tarea:

```
sudo cat /var/log/syslog | grep cron
```

Pero como en nuestro usuario actual no la estamos ejecutando no veremos nada. En cambio con `CRON` con mayúscula veremos si logró ejecutarse:

```
sudo cat /var/log/syslog | grep CRON
```

> Nos aparecerá vinculado a `root`e indicando el comando que ejecutó.



Otro ejemplo para guardar la fecha cada un minuto en un archivo:

```
* * * * * date >> /root/date.txt
```

Luego `sudo su -` y con `ls` veremos que se creó ese archivo. Luego con `crontab -e` podremos editar (ya estamos en root por lo que con ese comando editaremos el cronjob de ese usuario y no hace falta especificarlo).

## Listar Cron Jobs de otro usuario

```
sudo crontab -u root -l
```



## Cron Job y webserver

Instalamos `sudo apt install apache2` si vamos a `localhost` veremos la página de bienvenida del web server.

Con `cat /etc/passwd | grep www` veremos que tenemos el usuario `www-data`. Si el web server está sirviendo un sitio web podría ser un cron job el encargado de realizar un backup o de estar vinculado con algún script.

**Atención:** No podremos ejecutar `sudo su - www-data` porque ese usuario tiene el shell seteado a `:usr/sbin/no-login` un usuario de sistema por defecto no se puede loguearse al sistema. Si permitiéramos que alguien se loguee como ese usuario podría borrar el sitio, insertar malware, etc. Aunque no puedo loguearme puedo agregar un cron job a ese usuario:

```
sudo crontab -u www-data -e
```

Por ejemplo si queremos ejecutar un script que realice un backup todos los días a las 3 am:

```
0 3 * * * /usr/local/bin/website_backup.sh
```

> Siempre que trabajemos con cron jobs debemos indicar el *full path*. Si bien normalmente para ejecutarlo manualemente podríamos ingresar sólamente el nombre del script `website_backup`, esto es porque en nuestra distribución el path incluye `usr/local/bin` por defecto y nuestra shell sabe que debe buscar allí los comandos. Sin embargo, en el usuario con el cual estamos ejecutando los cron jobs puede no suceder lo mismo y queremos asegurarnos que no haya inconvenientes.



## Cron Expressions

Las *cron expressions* estan formados por 6 campos y   tienen la siguiente estructura:

```
<minute> <hour> <day-of-month> <month> <day-of-week> <command>
```

* minute (0-59)
* hour (0-23)
* day of month (1-31)
* month (1-12) (también podemos poner `JAN` - `DEC`)
* day of the week (0 - 7 pudiendo representar al domingo como `0` o como `7`) (también podemos poner `SUN` - `SAT`)



> En la páginas https://crontab.guru/ o https://crontab-generator.org/ podemos simular las distintas expresiones y ver cuando se ejecutarán.

  

* Si queremos que algo se ejecute en todos los minutos, de todas las horas, de todos los días del mes, de todos los meses y de todos los días de la semana tendremos: `* * * * *`
* Si queremos que algo se ejecute **a las y 5** de cada hora: `5 * * * *`
* Si queremos que algo se ejecute **cada 5 minutos** `*/5 * * * *`
* Si queremos que algo se ejecute cada 4 horas `0 */4 * * *`
* Si queremos que un comando se ejecute todos los días a las 9:05 `5 9 * * *`
* Si queremos que un comando se ejecute todos días 15 del mes a las 9:05hs `5 9 15 * *`.
* Si queremos que un comando se ejecute una vez por año el 15 de agosto a las 9:05hs: `5 9 15 8 *`.
* Si queremos que un comando se ejecute el 15 de agosto a las 9:05hs los días que sean viernes: `5 9 15 8 5`
* Si queremos ejecutar algo a las 12 del mediodía: `0 12 * * *`
* Si queremos ejecutar algo en cada minuto desde las 13hs a 13:05hs todos los días: `0-5 13 * * * ` (los rangos los indicamos con `-`)
* Si queremos que algo se ejecute de 13:15 **y** a las 13:45 todos los jueves de junio: `15,45 13 * 6 Tue` (notar que podemos poner `TUE` con cualquier capitalización) (como son dos valores tipo lista los indicamos con `,`)
* Si queremos que algo se ejecute el día 15 de todos los meses a las 9:30hs: `30 9 15 * *`
* Si queremos que algo se ejecute cada 2 minutos `*/2 * * * *`



## Forma Simplificada

También podemos crear cron jobs de manera simplificada:

```
@hourly
daily
@weekly
@monthly
@annually
@yearly
```

Si queremos que un comando se ejecute cada vez que el servidor se reinice:

```
@reboot
```



## Output Redirection

Si queremos mutear la salida o ejecutar un determinado comando de manera silenciosa:

```
echo Hola >/dev/null 2>&1
```



> Si queremos agregar contenido "append to a file" debemos usar el redirection operator `>>`
>
> ```
> touch file.txt
> echo Hola > file.txt
> echo Chau >> file.txt
> ```
>
> El contenido del archivo será:
>
> ```
> Hola
> Chau
> ```
>
> Si usáramos `>` en la última línea el contenido del archivo sería sólo Chau.



# Nano

Nano es un editor de texto, muy simple de usar y por eso normalmente elegido por los principiantes. En muchas distribuciones ya viene instalado, lo cual podemos chequear con:

```
which nano
```

Con lo cual obtenemos `/usr/bin/nano` indicándonos donde se encuentra el binario. En caso de no tenerlo, podemos instalarlo en Debian y Ubuntu usando `apt` con:

```
sudo apt install nano
```



## Crear nuevo 

Si queremos crear un nuevo docuemento podemos ingresar `nano` y nos aparecerá **New Buffer** en la parte superior.



## Guardar como...

Luego de editarlo con `CTRL+O` indicamos que queremos guardarlo, le damos un nombre presionamos `ENTER` y con `CTRL+X` cerramos el editor. 

> Si no indicamos una ubicación guardará el archivo en la ubicación actual.



## Editar Documento

Si queremos editar un documento que se encuentra en el directorio de trabajo local:

```
nano file.txt
```



Mientras que si se encuentra en otra ubicación debemos colocar la ruta:

```
nano /etc/ssh/sshd_config
```



## Buscar

Con `CTRL+W` buscamos la cadena de texto deseada (si estamos en windows y no queremos que nos cierre la terminal podemos hacerlo con `F6`).



## Cortar Línea 

```
CTRL+K
```



## Pegar Línea

```
CTRL+U
```



## Iniciar en línea específica

```
nano +15 sshd_config
```



## Abrir archivo en modo lectura

Si estamos trabajando con un archivo y no queremos editarlo accidentalmente, podemos abrirlo en *view only*.

```
nano -v sshd_config
```



## Ir a línea

Si presionamos `CTRL+W` o `F6` veremos que el menú cambia y nos indica que si presionamos `CTRL+T` podremos navegar a la línea deseada:



## Chequear Ortografía

Para chequar la sintaxis de una línea en particular debemos instalar el paquete **spell**

```
sudo apt install spell
```

Una vez instalado este paquete podremos chequar la ortografía dentro de nano:

```
CTRL+T
```

Nos seleccionará la palabra mal escrita y podremos ingresar una palabra de reemplazo, presionamos `ENTER` y luego `Y` para confirmar. Por último `CTRL+C` para salir del modo de chequeo de escritura.



# Versiones de Linux 

Se recomienda instalar la versión **LTS** ya que las actualizaciones de seguridad están disponibles durante más tiempo que en otras versiones.

## Versión Instalada

Para conocer la distribución y su versión instalada en nuestro servidor

```
cat /etc/os-release
```



Como este archivo puede llamarse de diferentes formas según la distribución con la que estemos trabajando, podemos recurrir a un *wildcard* a la hora de mostrarlo en pantalla:

```
cat /etc/*release*
```

 

## Package Managerss

Los manejadores de paquetes nos permiten instalar software en sistemas Linux.

### rpm

CentOS utiliza **RPM** (Red Hat Package Manager) al igual que Red Hat Enterprise Linux y Fedora. El software es empaquetado en un bundle con la extensión rpm.

* Instalación:

```
rpm -i telnet.rpm
```
* Desinstalación:

```
rpm -e telnet.rpm
```

* Obtener información sobre un paquete instalado (*query the database*)

```
rpm -q telnet.rpm
```



rpm requiere que indiquemos la ubicación exacta del paquete rpm y no instalará las dependencia que tenga el paquete. Existe una solución que utilizando un único comando encuentra la ubicación del paquete, lo instala e instala también todas sus dependencias. 



### Yum

Yum es un manejador de paquetes que utiliza rpm por debajo

```
yum install ansible
```

Este comando buscará en los repositorios, instalará el paquete y también sus dependencias.

La información sobre los repositorios está en `/etc/yum.repos.d`



#### Obtener lista de repositorios

```
yum repolist
```

Luego con 

```
ls /etc/yum-repos.d
```

Podremos ver los archivos con los que estos repositorios están configurados con la dirección donde están todos los paquetes `.rpm`.



Para listar los paquetes instalados o disponibles:

```
yum list ansible
```

Para eliminar un paquete

```
yum remove ansible
```

Instalar version específica

```
yum install ansible-2.4.2.0
```



## :penguin: Instalación exa

Exa es una alternativa al comando `ls` **escrita en Rust** con algunas opciones adicionales.

```
sudo apt install exa
```

Una de sus [características](https://the.exa.website/features/icons) principales es la posibilidad de mostrar íconos a la salida.

```
exa --icons
```



## :penguin:Instalación de bat 

bat es una alternativa a cat **escrita en Rust** para mostrar el contenido de un archivo de texto con un formato un poco mas claro en el que se verá un recuadro, números de línea, un formateo con colores, etc.

```
sudo apt install bat
```



# Zsh

### :penguin: Instalación Zsh

Vamos a instalar el shell zsh. Recordemos que los shells son programas que nos permiten interactuar con el sistema operativo sin la necesidad de utilizar interfaces gráficas.

Para la instalación de zsh nos basamos en el [siguiente instructivo](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH).

En Windows abrimos WSL2 o Ubuntu y ejecutamos `sudo apt update` y `sudo apt upgrade`.

1. Ejecutar `sudo apt install zsh`

2. Verificar que la instalación se haya efectuado correctamente con `zsh --version` 
3. Convertir a Zsh en el shell por defecto `chsh -s $(which zsh)`.
4. Desloguearnos y volver a loguearnos. Cuando iniciemos nos aparecerá un mensaje de configuración de del archivo de arranque de Z Shell, elegimos la opción 2.
5. Verificar que haya funcionado con `echo $SHELL` que nos arroja algo como `/usr/bin/zsh`.
6. Verificar con `$SHELL --version` que nos arroja algo como `zsh 5.8 (x86_64-ubuntu-linux-gnu)`.



### :penguin: Instalación Oh My Zsh

Vamos a personalizar a Zsh con Oh My Zsh [de acuerdo a lo indicado en su página web](https://ohmyz.sh/#install)



### :penguin: Alias en ZSH

Podemos crear un alias agregándolo en `~/.zshrc`.

```
alias l='exa -1 --icons'
alias cat='batcat'
```

 Podremos **recargar la configuración** de dicho archivo con `source ~/.zshrc`.



