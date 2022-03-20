# :shield: Recomendaciones de Seguridad

> Basado en [5 Easy Tweaks to increase your Linux Server's Security](https://youtu.be/OVsMaXQkktQ) de LearnLinuxTV.

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



## Configuraciones en `sshd_config`

### Autenticación por ssh con llave pública

Queremos plantear una autenticación con llave pública en lugar de utilizar el password para conectarnos por ssh.

:stop_sign: Ver el apartado de SSH donde se explica esta alternativa a la autenticación con password.



### Deshabilitar Autenticación por ssh con contraseña

Editamos el archivo de configuración del servicio de ssh.

```
sudo nano /etc/ssh/sshd_config
```

Donde dice `PermitRootLogin yes` podemos cambiarlo a `no` para impedir el logueo como `root`.

Donde dice `PasswordAuthentication yes` colocamos `no` de manera que sólo sea posible conectarnos por *public key authentication*

Los cambios tendrán efecto recién cuando reiniciemos el servicio de ssh:

```
sudo systemctl restart sshd
```

> Aunque hemos reiniciado el servicio y estamos conectados por ssh no se desconecta.



:warning: Es importante abrir dos sesiones antes de editar este archivo. En caso de que cometamos algún error y no podamos volver a conectarnos, tendremos la otra sesión para solucionar el problema.



## Implementación Fail2Ban

Fail2Ban es un framework escrito en python que protege los sistemas Linux de ataques de fuerza bruta. Entre las múltiples funcionalidades que tiene una de ellas es para proteger los ataques de fuerza bruta en un servidor por SSH. También nos permite monitorear la magnitud de los ataques basado en el número de intentos de autenticación que son realizados.

Para instalarlo

```
sudo apt install fail2ban
```

Para verificar que está corriendo

```
systemctl status fail2ban
```

Para chequear de qué nos está protegiendo inicialmente

```
sudo fail2ban-client status
```

Debemos verificar que aparezca `- Jail list: sshd`  lo cual significa que fail2ban está controlando ese servicio, chequeando los logs y si ve muchos fallos al ingresar por SSH baneará la IP que está intentando entrar al servidor. 

Debemos evitar que banee accidentalmente nuestra dirección IP. podríamos editar de manera directa `/etc/fail2ban/jail.conf` pero no es conveniente ya que en caso de actualizaciones este archivo se pisará. Por ese motivo realizamos una copia (desde el directorio de ese archivo):

```
sudo cp jail.conf jail.local
```

En caso de que exista `jail.local` fail2ban utilizará ese, caso contrario utilizará `jail.conf`. 

Por lo tanto ahora editamos `jail.local` sabiendo que no será sobrescrito:

```
sudo nano jail.local
```



En el campo `ignoreip = 127.0.0.1/8::1` lo aconsejable es descomentarlo y colocar la IP pública desde donde vamos a querer conectarnos de modo que nunca seamos baneados nosotros mismos. También podríamos poner la IP de una VPN desde la cual nos conectamos si fuera este el caso.



Dentro de las configuraciones veremos:

`bantime = 10m` es decir que la IP será baneada por 10 minutos.

`maxretry=5` es la cantidad de intentos que puede fallar.



# Cambiar nombre de host

The procedure to change the computer name on Ubuntu Linux:

1. Type the following command to edit /etc/hostname using nano or vi text editor:
   `sudo nano /etc/hostname`
   Delete the old name and setup new name.

   

2. Next Edit the /etc/hosts file:
   `sudo nano /etc/hosts`
   Replace any occurrence of the existing computer name with your new one.

   

3. Reboot the system to changes take effect:
   `sudo reboot`



# Aumentar Tamaño Fuente Terminal

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



# Configuración Servidor

> Basado en [este artículo](https://www.redeszone.net/tutoriales/servidores/servidor-openssh-linux-configuracion-maxima-seguridad/)
>

En primer lugar debemos instalar el servidor SSH.

```bash
sudo apt-get install openssh-server
```
Para editar la configuración del servidor SSH:

```
sudo nano /etc/ssh/sshd_config
```



**Para arrancar el servidor:**

```
sudo /etc/init.d/ssh start
```

**Para parar el servidor:**

```
sudo /etc/init.d/ssh stop
```

**Para reiniciar el servidor:**

```
sudo /etc/init.d/ssh restart
```
**Para conocer el estado del servidor:**

```        bash
sudo service ssh status
```



## Agregar Usuario al Grupo root:

```bash
usermod -aG sudo username 
```



## Información OS

Si queremos obtener información sobre el sistema operativo, podremos ejecutar el siguiente comando:

```
cat /etc/*release*
```

> Notar el uso de *wildcard* ya que el archivo puede llamarse de diferentes formas segun la distribución con la que estemos trabajando.
