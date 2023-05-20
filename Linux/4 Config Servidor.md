# :shield: Recomendaciones de Seguridad

> Basado en [5 Easy Tweaks to increase your Linux Server's Security](https://youtu.be/OVsMaXQkktQ) de LearnLinuxTV.
>
> :link: [5 steps to secure Linux by NetworkChuck](https://youtu.be/ZhMw53Ud2tY)
>
> :link: [How to protect Linux by Christian Lempa](https://youtu.be/Bx_HkLVBz9M)



A continuación presentamos una serie de recomendaciones de seguridad para nuestros servidores Linux para protegernos de hackers, malware y todas aquellas cosas que podrían infectar nuestros sistemas.

Para este analisis partimos de la premisa que lograr un sistema a prueba de hackeos es prácticamente imposible pero intentaremos incluir buenas prácticas para disminuir la superficie de ataque cubriendo las amenazas mas comunes y más comunes.

# Crear usuario no root

Es aconsejable no utilizar `root` en un servidor Linux a menos que sea absolutamente necesario ya que ante un error puede que causemos grandes daños. Cuando instalamos Ubuntu en un servidor no tendremos este problema ya que en el proceso crearemos un usuario, pero cuando trabajamos con una instancia VPS comenzaremos trabajando con `root`. Es por eso que lo primero que debemos hacer es crear un usuario no root al que le daremos privilegios de `sudo` para poder ejecutar comandos como `root` desde un usuario normal.

En lo sucesivo asumimos que estamos logueados como `root` y por eso no ponemos `sudo`, en caso de no estarlo debemos ponerlo.

```
adduser juan
```

Luego tendremos que ingresar la contraseña y nos pedirá una serie de datos y podemos dejarlos en blanco e ir presionando `ENTER`. Por último nos preguntará si la información es correcta y nos mostrará `Y/n` el hecho de que tenga mayúscula la `Y` da cuenta que se trata de la **opción por defecto** por lo que podremos presionar nuevamente `ENTER` directamente.



Acabamos de utilizar el comando `adduser` que es interactivo y nos permite realizar configuraciones automáticas por lo que se lo considera de alto nivel si lo comparamos con otra forma de lograr lo mismo que es utilizando `useradd` que es considerado un comando más básico y de bajo nivel.

También podremos usar el comando `useradd`

```
useradd juan -m -s /bin/bash -c "administrative user"
```

`-m` crear directorio de inicio para el nuevo usuario será `/home/juan`

`-s` establecemos el intérprete de comandos (shell) para el nuevo usuario.

`-c` establecer comentario al nuevo usuario.

Para asignarle un password si usamos `useradd`  tendremos que ingresar:

```
passwd juan
```



El siguiente paso es agregar este usuario al grupo `sudo` ya que siendo miembro de este grupo podrá utilizar `sudo`.

```
usermod -aG sudo juan
```

Con `-aG` indicamos que queremos agregar un usuario a un grupo.



Si queremos agregarlo a más de un grupo podremos hacer `usermod -aG sudo,adm,docker juan` con `adm` podremos leer algunos logs específicos y con `docker` podremos ejecutar comandos de Docker sin usar Docker.





Podemos verificar que el usuario forma parte de ese grupo 

```
groups juan
```



:bulb: Para loguearnos con este usuario por SSH haremos: `ssh juan@<ip-address>` y nos pedirá la contraseña establecida para el usuario.



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



# Actualizaciones

Uno de los puntos mas importantes para mantener la seguridad de nuestros sistemas es realizar las actualizaciones del sistema operativo y de las aplicaciones que tenemos instaladas. Cuando se descubre que existe una nueva vulnerabilidad de seguridad suele haber parches disponibles para solucionarla.



## Actualizaciones Manuales

Para refrescar la lista de los paquetes del repositorio que tenemos disponibles para actualizar.

```
sudo apt update
```

Para instalar las actualizaciones disponibles

```
sudo apt dist-upgrade
```

Luego reiniciamos el servidor `sudo reboot`



## Actualizaciones Automáticas

En Linux el software es actualizado por el package manager propio de la distribución, en Ubuntu es posible utilizar unattended-upgrades para llevar a cabo actualizaciones de seguridad de manera automática.

En Ubuntu debería estar instalado pero podemos instalarlo con:

```
sudo apt install unattended-upgrades
```



Luego lo configuramos para que actualice automáticamente los parches de seguridad del sistema:

```
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

Luego nos aparecerá un menu y nos preguntará si queremos descargar e instalar actualizaciones estables automáticamente e indicamos que sí.



Como consecuencia de esto se escribirán dos archivos de configuración en `/etc/apt/apt.conf.d`:

* `20auto-upgrades`
* `50unattended-upgrades`



Luego yendo a `etc/apt/apt.conf.d` veremos que tenemos una serie de archivos con `ls -l` que editaremos.

En `20auto-upgrades`agregamos dos líneas, una para que el listado de paquetes se refresque automáticamente y otra para que instale automáticamente estas actualizaciones:

```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Unattended-Package-Lists "1";

```

En `50unattended-upgrades`veremos que tenemos habilitadas las actualizaciones relacionadas con **security** e **infra security**. 



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



Para verificar que la configuración fue aplicada correctamente podemos ejecutar:

```
sudo unattended-upgrade --dry-run --debug
```

> Notar que es upgrade en singular.

Veremos si hay paquetes que pueden ser actualizados automáticamente y además nos indicará que el script se ejecutará una vez por día.



## Actualizaciones contenedores  

Cuando tenemos aplicaciones basadas en contenedores Docker debemos evitar algunos inconvenientes.

En la práctica experimentaremos que no todas las imágenes se actualizan tanto como deberían por lo tanto antes de elegir una imagen debemos analizar quién es su creador y asegurarnos que sean proyectos mantenidos activamente. En ese caso es aconsejable ir a una fuente que las actualice y mantenga como las que podemos obtener en https://www.linuxserver.io/ Cuando existe una nueva imagen Docker que incluye una actualización de seguridad será necesario hacer el re-deploy del contenedor, ya que los contenedores son inmutables.

Es aconsejable la herramienta llamada [Watchtower](https://containrrr.dev/watchtower/) para automáticamente actualizar todos los contedores Docker en un horario donde un pequeño reinicio sea posible.



# Autenticación

SSH es un secure protocol para autenticarnos remotamente en un shell de Linux. Se trata de un protocolo muy seguro pero habrá que acompañarlo de buenas prácticas. Por ejemplo en caso de utilizar contraseña debemos asegurarnos que esta sea segura. Otra opción es no utilizar password sino una llave pública y privada, con lo cual no tendremos que recordar ninguna contraseña ni correremos riesgos por utilizar la misma en más de un sitio.

## Autenticación por ssh con llave pública

Queremos plantear una autenticación con llave pública en lugar de utilizar el password para conectarnos por ssh.

En primer lugar crearemos una llave pública y una privada. Luego almacenaremos en el servidor la llave pública que podemos pensarla como una candado que sólo puede ser abierta con la llave privada que conservaremos en nuestra pc. 

:stop_sign: Para más información ver el apartado de SSH donde se explica esta alternativa a la autenticación con password.

En el **servidor** creamos el directorio `.ssh` para almacenar las llaves públicas y cambiamos los permisos de modo que sólo el dueño pueda acceder y modificar estos archivos proporcionando una capa extra de seguridad.

```
mkdir ~/.ssh && chmod 700 ~/.ssh
```

> Tener en cuenta que `~` representa a `/home/juan`

Con 700 en octal estamos indicando con el 7 los permisos del dueño del archivo garantizando lectura (4), escritura (2) y ejecución (1). Luego con el segundo y el tercer dígito le estamos indicando que el  grupo y otros no tienen permisos.

En nuestra pc creamos las llave públicas y privadas:

```
ssh-keygen -b 4096
```

Como en ocasiones tendremos muchas llaves, es aconsejable asociarla a un comentario: `ssh-keygen -b 4096 -C juan@example.com`

Se recomienda colocar un passphrase especialmente en aquellos casos donde no tengamos las mejores condiciones de seguridad (como contar con device encryption o user authentication en la máquina cliente). El passpharase que nos protegerá en aquellos casos donde alguien acceda a nuestro file system  y copie la llave privada, con la cual podrá loguearse en nuestro server.

Asumiendo que estamos en **Windows** copiamos la llave pública al servidor:

```
scp $env:USERPROFILE/.ssh/id_rsa.pub juan@<ip-address>:~/.ssh/authorized_keys
```

scp = secure copy protocol



O desde `~/.ssh`

```
scp id_rsa.pub juan@<ip-address>:~/.ssh/authorized_keys
```



En **Linux** ejecutaríamos:

```
ssh-copy-id juan@<ip-address>
```



Si la carpeta `.ssh` la hubieramos creado con `root` y también el archivo lo hubieramos copiado con ese usuario, deberiamos cambiar el dueño a `juan` de modo que pueda leeerlo. Para eso debemos ejecutar `chown -R juan:juan .ssh`. Lo hacemos recursivo con `-R` para afectar a todos los archivos internos del directorio.

La próxima vez que nos conectemos por ssh no nos pedirá la contraseña.



# Configuración `sshd_config`

## Deshabilitar Autenticación por ssh con contraseña

Editamos el archivo de configuración del servicio de ssh.

```
sudo nano /etc/ssh/sshd_config
```

Donde dice `PasswordAuthentication yes` colocamos `no` de manera que sólo sea posible conectarnos por *public key authentication*

Los cambios tendrán efecto recién cuando reiniciemos el daemon de ssh:

```
sudo systemctl restart sshd
```

> :warning: Aunque hemos reiniciado el servicio y estamos conectados por ssh no se desconecta.
>
> :warning: Es importante abrir dos sesiones antes de editar este archivo. En caso de que cometamos algún error y no podamos volver a conectarnos, tendremos la otra sesión para solucionar el problema.



## Impedir logueo como `root`

Donde dice `PermitRootLogin yes` podemos cambiarlo a `no` para impedir el logueo como `root`.



## Cambiar puerto ssh

Cambiar el puerto predeterminado para SSH (el 22) es una práctica común para mejorar la seguridad de un servidor: Logrando reducir la exposición ya que muchos bots escanean el puerto 22 para intentar ataques de fuerza bruta o explotar vulnerabilidades conocidas. Está claro que esto no representa una medida de seguridad por sí sola pero obliga a un escaneo de puertos más amplio para identificar si el servidor tiene habilitado SSH y en qué puerto se encuentra.

```
sudo nano /etc/ssh/sshd_config
```

Descomentar la línea `#Port 22` colocando por ejemplo `Port 717`



Para acceder al servidor ahora tendremos que hacer:

```
ssh -p 717 juan@<ip-address>
```

 

## Configurar ipv4 only

En lugar de `#AddressFamily any` indicamos que sólo queremos recibir solicitudes ipv4.

```
AddressFamily inet
```



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



## Conocer puertos en uso

Con el comando `ss` obtendremos info sobre las conexiones de red y los sockets.

- `-t`: muestra únicamente los sockets TCP.
- `-u`: muestra únicamente los sockets UDP.
- `-p`: muestra el proceso asociado con cada socket.
- `-l`: muestra únicamente los sockets en estado de escucha (listening).
- `-n`: muestra los puertos en formato numérico en lugar de resolver los nombres de servicio.

```
sudo ss -tupln
```



## Firewall UFW

```
sudo apt install ufw
```

ufw = uncomplicated firewall



Conocer el estado

```
sudo ufw status
```



Habilitar puerto 717

```
sudo ufw allow 717
```



Activar firewall

```
sudo ufw enable
```



Luego de activar el firewall lo aconsejable es loguearnos desde otra terminal para asegurarnos de que todo está bien y que nos quedemos afuera de nuestro server.



Supongamos que tenemos un sitio web que queremos exponer, para esto instalamos el servidor web Apache:

```
sudo apt install apache2
```

Luego lo iniciamos

```
sudo systemctl start apache2
```

En este momento si ejecutamos `sudo ss -tupln` veremos qeu el puerto 80 está en uso pero eso no significa que sea accesible navegando a `<ip-address>` ya que el firewall lo estará bloqueando por lo que tendremos que ejecutar:

```
sudo ufw allow 80/tcp
```



## Deshabilitar ping 

Como otra medida de ocultamiento, es aconsejable deshabilitar la posibilidad de hacerle ping a la dirección IP del servidor de manera tal de dificultar el conocimiento de que el servidor está activo.

```
sudo nano /etc/ufw/before.rules
```

En el apartado donde dice `# ok icmp codes for INPUT` debemos colocar al comienzo:

```
-A ufw-before-input -p icmp --icmp-type echo-request -j DROP
```

The rule `-A ufw-before-input -p icmp --icmp-type echo-request -j DROP` is used in UFW (Uncomplicated Firewall) to drop incoming ICMP echo-request packets, which are commonly associated with ping requests. This rule prevents the server from responding to ICMP echo requests, effectively blocking incoming pings.



```
sudo ufw reload
```



```
sudo reboot
```



# Configuraciones

## Cambiar nombre de host

* Editar el archivo `/etc/hostname`  borrando el nombre antiguo y especificando el nuevo.

```
sudo nano /etc/hostname
```



* Edita el archivo `/etc/hosts` reemplazando cualquier aparición del nombre de la computadora existente por el nuevo.

```
sudo nano /etc/hosts
```



Reinicia el sistema para que los cambios surtan efecto: 

```
sudo reboot
```



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
