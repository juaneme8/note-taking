# Edit in Place

En ocasiones puede que tengamos que editar un archivo de configuración (por ejemplo dentro de un contenedor) y no tengamos ningún editor disponible  (`vi`, `vim`, `nano`).

Una solución podría ser instalar uno de estos paquetes, pero puede que no tengamos privilegios o aún teniéndolos si son cambios pequeños puede que nos convenga editar el archivo al vuelo.

```
sed -i "/s/texttobechanged/textwanted/g" filename
```



Por ejemplo

```
docker exec 900b... sed -i "s/It works!/Docker1/" /usr/local/apache2/htdocs/index.html
```

> En Windows este comando utilizando comillas simples no funcionó.



## Cambiar nombre de host

The procedure to change the computer name on Ubuntu Linux:

1. Type the following command to edit /etc/hostname using nano or vi text editor:
   `sudo nano /etc/hostname`
   Delete the old name and setup new name.

   

2. Next Edit the /etc/hosts file:
   `sudo nano /etc/hosts`
   Replace any occurrence of the existing computer name with your new one.

   

3. Reboot the system to changes take effect:
   `sudo reboot`



## Aumentar Tamaño Fuente Terminal

Para aumentar el tamaño de fuente, debemos editar el archivo `/etc/default/console-setup`

```
sudo nano /etc/default/console-setup
```

Cambiando los siguientes valores:

```
FONTFACE="TER"
FONTSIZE="16x32"
```

Luego aplicamos los cambios con el siguiente comando:

```
sudo update-initramfs -u
```

Luego podemos reiniciar la máquina para ver los cambios `reboot`.



# Users

`su -` 

- logs you into root with the root environment
- (as opposed to `su` which logs you into root with your environment)

And I understand what `sudo` does

- you are root for one command



`su` requires the root password and `sudo` requires your user password

Therefore `sudo su -` will put you into a root environment but it will ask you for your user password instead of the root password (once sudo has given you root privileges, `su -` can be executed with no password).

Luego con `CTRL+D` nos desconectaremos de ese usuario.



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



# Recomendaciones de Seguridad

## Crear usuario no root

Es aconsejable no utilizar `root` en un servidor Linux a menos que sea absolutamente necesario ya que ante un error puede que causemos grandes daños. Cuando instalamos Ubuntu en un servidor no tendremos este problema ya que en el proceso crearemos un usuario, pero cuando trabajamos con una instancia VPS comenzaremos trabajando con `root`. Es por eso que lo primero que debemos hacer es crear un usuario no root al que le daremos privilegios de `sudo` para poder ejecutar comandos como `root` desde un usuario normal.

En lo sucesivo asumimos que estamos logueados como `root` y por eso no ponemos `sudo`, en caso de no estarlo debemos ponerlo.

```
adduser juan
```

Luego tendremos que ingresar la contraseña y nos pedirá una serie de datos y podemos dejarlos en blanco e ir presionando `ENTER`. Por último nos preguntará si la información es correcta y nos mostrará `Y/n` el hecho de que tenga mayúscula la `Y` da cuenta que se trata de la opción por defecto por lo que podremos presionar nuevamente `ENTER` directamente.

El siguiente paso es agregar este usuario al grupo `sudu` ya que siendo miembro de este grupo podrá utilizar `sudo`.

```
usermod -aG sudo juan
```

> Con `-aG` indicamos que queremos agregar un usuario a un grupo.

Podemos verificar que el usuario forma parte de ese grupo 

```
groups juan
```



### Cambiar usuario

```
su - juan
```

> Si estamos logueados como root no nos pedirá el password.

Para verificar que `sudo` funciona

```
sudo ls /etc
```

Luego nos pedirá el password.



## Actualizaciones

Realizar una actualización completa de los parches de seguridad del servidor antes de colocarlo en producción.

Para refrescar la lista de los paquetes que tenemos disponibles para actualizar.

```
sudo apt update
```

Para instalar las actualizaciones disponibles

```
sudo apt dist-upgrade
```

Luego reiniciamos el servidor `sudo reboot`



## Actualizaciones Automáticas

```
sudo apt install unattended-upgrades
```

Luego yendo a `etc/apt/apt.conf.d` veremos que tenemos una serie de archivos con `ls -l` que mas adelante editaremos.

Para asegurarnos que las actualizaciones automáticas están instaladas:

```
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

Luego nos preguntará si queremos descargar e instalar actualizaciones estables automáticamente e indicamos que sí.



En `20auto-upgrades`agregamos dos líneas, una para que el listado de paquetes se refresque automáticamente y otra para que instale automáticamente estas actualizaciones:

```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Package-Lists "1";

```

En `50unattended-upgrades`veremos que tenemos habilitadas las actualizaciones relacionadas con **security** y **infra security**



Si queremos recibir un correo cada vez que unattended upgrades actualiza algo, debemos descomentar esta línea y colocar el mail:

```
Unattended-Upgrade::Mail ""
```



En otra opción veremos `Unattended-Upgrade::Mail Report "on-change";` pero esto puede ser molesto si tenemos muchos servidores, por lo que podemos cambiarlo a:

```
Unattended-Upgrade::Mail Report "only-on-error"
```



Para evitar que se llene el disco con versiones viejas del kernel descomentamos esta línea:

```
Unattended-Upgrade::Remove-Unused-Kernel-Packages "true"
```

En la misma dirección para evitar tener muchos paquetes que no usamos (lo cual también atenta con la seguridad ya que lo aconsejable es tener el menor número de paquetes) también debemos descomentar la línea:

```
Unattended-Upgrade::Remove-New-Unused-Dependencies "true"
```

También descomentamos:

```
Unattended-Upgrade::Remove-Unused-Dependencies "fase"
```



Si tenemos la posibilidad de contar con una ventana de mantenimiento que habilita a reiniciar el servidor (y no un servicio 24x7) también podemos descomentar y cambiar a `true`:

```
Unattended-Upgrade::Automatic-Reboot "false";
```

En la misma dirección si queremos que se reinicie aunque haya un usuario conectado:

```
Unattended-Upgrade::Automatic-Reboot-WithUsers "true";
```

En relación a lo anterior tenemos:

```
Unattended-Upgrade::Automatic-Reboot-Time "02:00";
```



**En todos estos casos debemos analizar la situación propia de nuestro servidor y evaluar si deseamos contar con esa opción o no.**
