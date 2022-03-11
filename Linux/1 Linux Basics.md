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

> Siempre que trabajemos con cron jobs debemos indicar el *full path*.

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



> En la página https://crontab.guru/ podemos simular las distintas expresiones y ver cuando se ejecutarán.



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

