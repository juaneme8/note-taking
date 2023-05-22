# :shield: Recomendaciones de Seguridad

> Basado en [5 Easy Tweaks to increase your Linux Server's Security](https://youtu.be/OVsMaXQkktQ) de LearnLinuxTV.
>
> :link: [5 steps to secure Linux by NetworkChuck](https://youtu.be/ZhMw53Ud2tY)
>
> :link: [How to protect Linux by Christian Lempa](https://youtu.be/Bx_HkLVBz9M)
>
> :link: [How to protect your Linux server from hackers by LiveOverflow](https://youtu.be/fKuqYQdqRIs)



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

Editamos el archivo de configuración del servicio de ssh.

```
sudo nano /etc/ssh/sshd_config
```



## Deshabilitar Autenticación por ssh con contraseña



Donde dice `PasswordAuthentication yes` colocamos `no` de manera que sólo sea posible conectarnos por *public key authentication*.



### Reiniciar daemon ssh

En todos los casos los cambios tendrán efecto recién cuando reiniciemos el daemon de ssh:

```
sudo systemctl restart ssh
```

> :warning: Aunque hemos reiniciado el servicio y estamos conectados por ssh no se desconecta.
>
> :warning: Es importante abrir dos sesiones antes de editar este archivo. En caso de que cometamos algún error y no podamos volver a conectarnos, tendremos la otra sesión para solucionar el problema.





### :deciduous_tree: Comparativa SSH Keys vs Contraseñas

:neutral_face: **Analisis crítico by LiveOverflow**

Deshabilitar la autenticación mediante password y utilizar en su lugar llaves SSH suele ser una de las recomendaciones más recurrentes a la hora de hacer un hardening de servidores Linux. 

Sin embargo, debemos destactar que deshabilitar la autenticación mediante contraseña como indica [el video de LiveOverflow](https://youtu.be/fKuqYQdqRIs) no representa una mejora a la seguridad en sí mismo. 

Analizaremos a continuación el documento [The Secure Shell (SSH) Protocol Architecture](https://www.rfc-editor.org/rfc/rfc4251) para entender cómo nuestra máquina local habla con el servidor remoto.

En el apartado [9.4.5](https://www.rfc-editor.org/rfc/rfc4251#section-9.4.5) veremos que una autenticación mediante password no deberá utilizarse en caso de que el servidor esté comprometido pues revelará al atacante la combinación username/password.

> The password mechanism, as specified in the authentication protocol, assumes that the server has not been compromised.  If the server has been compromised, using password authentication will reveal a valid username/password combination to the attacker, which may lead to further compromises.

 No obstante, el uso de llaves SSH también asume algo parecido:

> The use of public key authentication assumes that the client host has not been compromised.  It also assumes that the private key of the server host has not been compromised.



En `sshd_config` veremos:

```
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication no
```

Esto podría asustarnos, pero hace referencia a que se se creará un canal encriptado con el server y allí se enviará ese clear text password. Es por esto que el servidor ssh tiene su propia private-key y es por eso que debemos verificar la fingerprint antes de conectarnos a un nuevo servidor. Luego nuestra máquina recordará la clave pública del server en el archivo known-hosts. En caso de que un atacante nos haga un Man-in-the-middle o escribamos mal la ip el ssh client nos avisará que algo no está bien mostrándonos el mensaje REMOTE HOST IDENTIFICATION HAS CHANGED. 



Sí es una buena recomendación pues evitaremos caer en tener que pegar passwords de un PM o lo que es peor reutilizar el mismo en varios lugares.

### :deciduous_tree: Analogía con HTTPS

Si bien HTTPS (SSL o TLS) es un protocolo distinto a SSH pero tiene un comportamiento similar cuando nos logueamos por ejemplo en Twitter, donde también enviaremos el cleartext password en HTTP request dentro del tunel TLS encriptado. En caso de que un network attacker nos haga man-in-the-middle el navegador nos advertirá y rechazará enviar la contraseña. Esto nos hace pensar que el riesgo asumido al utilizar passwords es similar al que tomamos cuando nos logueamos en estos servicios mediante contraseñas.



### :deciduous_tree:Conclusión

En caso de querer usar passwords asegurarnos que sean lo suficientemente fuertes (unique, random y largos que no puedan ser obtenidos por fuerza bruta) y no los reutilicemos en múltiples sitios. 

El uso de llaves SSH es mas conveniente porque nos evita tener que recordar estas passwords o copiarlas del password manager pero sin incrementar la seguridad.





## Impedir logueo directo como `root`

Es aconsejable crear usuarios con el mínimo de privilegios necesarios para llevar a cabo la tarea.

Donde dice `PermitRootLogin yes` podemos cambiarlo a `no` para impedir el logueo como `root`.



:neutral_face: **Ver analisis crítico by LiveOverflow sobre este punto**



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



:neutral_face: **Ver analisis crítico by LiveOverflow sobre este punto**



## Configurar ipv4 only

En lugar de `#AddressFamily any` indicamos que sólo queremos recibir solicitudes ipv4.

```
AddressFamily inet
```



:neutral_face: **Ver analisis crítico by LiveOverflow sobre este punto**

# Network security

En cuanto a las configuraciones de red contemplaremos lo relacionado al tráfico de entrada y salida del servidor, partiendo de la recomendación de solo exponer aquellos servicios que tengamos necesidad absoluta de hacerlo.



## Conocer puertos en uso

```
sudo ss -tupln
```

Con el comando `ss` obtendremos info sobre las conexiones de red y los sockets.

- `-t`: muestra únicamente los sockets TCP.
- `-u`: muestra únicamente los sockets UDP.
- `-p`: muestra el proceso asociado con cada socket.
-  
- `-n`: muestra los puertos en formato numérico en lugar de resolver los nombres de servicio.



Lo primero que podríamos hacer es indagar sobre cada uno de estos puertos si podemos configurar la aplicación que los utilice para que no lo haga o si no la necesitamos eliminarla.



## Firewall UFW

Otra opción de controlar el tráfico es mediante un firewall permitiendo sólo aquello especificado mediante reglas. Debemos tener presente que para los permitidos el firewall no agrega ninguna medida de protección extra. 



Utilizaremos UFW (uncomplicated firewall)



### :fire: Instalación

```
sudo apt install ufw
```



### :fire: Conocer el estado

```
sudo ufw status
```



### :fire:  Habilitar puertos

:bulb: Lo primero que debemos hacer es habilitar el puerto de SSH para que al activar el firewall no quedemos sin la posibilidad de ingresar al server.

```
sudo ufw allow 22
```



### :fire: Activar firewall

```
sudo ufw enable
```



:bulb: Luego de activar el firewall lo aconsejable es loguearnos desde otra terminal para asegurarnos de que todo está bien y que nos quedemos afuera de nuestro server.

En ocasiones tendremos que esperar unos minutos hasta que impacten los cambios, sobre todo si tenemos una conexión tcp abierta y debemos esperar a que se resetee.



### :fire: Ejemplos

Supongamos que tenemos un sitio web que queremos exponer, para esto instalamos el servidor web Apache:

```
sudo apt install apache2
```

Luego lo iniciamos

```
sudo systemctl start apache2
```

En este momento si ejecutamos `sudo ss -tupln` veremos que el puerto 80 está en uso pero eso no significa que sea accesible navegando a `<ip-address>` ya que el firewall lo estará bloqueando por lo que tendremos que ejecutar:

```
sudo ufw allow 80/tcp
```



### :fire: ufw con Docker

Además Docker no funciona por defecto con ufw, Ambos modifican la configuración `iptables` lo cual puede ocasionar inconvenientes exponiendo contenedores que se suponía que no serían públicos. Por ejemplo al intentar ingresar a las webs administrativas de determinados contenedores como portainer en el 9200 veremos que nos permitirá ingresar a ellas (asumiendo que las vemos con `sudo ss -tupln`) aunque no tengamos una regla establecida para tal fin.

Si visualizamos iptable chains veremos que ufw opera en la default chain y Docker crea una chain separada para sus redes que es ignorada por ufw.



### :fire: Conclusiones firewall

Es importante probar las configuraciones en nuestro setup tomando consideración en los puertos, aplicaciones y contenedores Docker que estamos corriendo y cómo están conectadas a la red. Luego debemos chequear que las reglas estén aplicándose correctamente.



### :fire: Deshabilitar ping 

Como otra medida de ocultamiento, es aconsejable deshabilitar la posibilidad de hacerle ping a la dirección IP del servidor de manera tal de dificultar el conocimiento de que el servidor está activo.

```
sudo nano /etc/ufw/before.rules
```

En el apartado donde dice `# ok icmp codes for INPUT` debemos colocar al comienzo:

```
-A ufw-before-input -p icmp --icmp-type echo-request -j DROP
```

The rule `-A ufw-before-input -p iccmp --icmp-type echo-request -j DROP` is used in UFW (Uncomplicated Firewall) to drop incoming ICMP echo-request packets, which are commonly associated with ping requests. This rule prevents the server from responding to ICMP echo requests, effectively blocking incoming pings.



```
sudo ufw reload
```



```
sudo reboot
```



:neutral_face: **Ver analisis crítico by LiveOverflow sobre este punto**

# Proxy Inverso

Luego de configurar nuestro firewall tendremos muchos servicios para los cuales tendremos que permitir el acceso como por ejemplo web server, mail, etc. Para proteger estos servicios expuestos debemos en primer lugar evitar utilizar protocolos inseguros. Esto significa que si tenemos una aplicación web que corre en puerto 80 (http) no vamos a querer autenticar con password ya que el tráfico no es encriptado y cualquier intermediario podría robar el password. Una manera de exponer aplicaciones inseguras via https y trusted ssl certs es utililizando un **proxy inverso**. 

Un proxy inverso es un servidor web que se coloca en frente de la aplicación y reenvía las solicitudes de clientes.

<img src="https://cylab.be/storage/blog/122/files/eSf2nkfQqEn8Z06zhAoCQYlIV3KU8r3ZsvHqnxRW.png" alt="Using HTTPS over a reverse proxy in Laravel | cylab.be" style="zoom: 33%;" />

Por ejemplo si queremos deployar una aplicación como bitwarden o portainer que no vienen con un servidor https propio podemos utilizar un proxy inverso como nginx para exponerlos seguramente con trusted ssl.

Podemos utilizar **Nginx Proxy Manager** que es un proxy inverso basado en el webserver nginx que tiene una interfaz amigable y también nos permite una capa básica de seguridad adicional bloqueando algunos common exploits. Esta herramienta es considerado un proyecto hobbista y existen otras alternativas que ofrecen una mayor protección, reciben el nombre de web application firewalls y suelen ser de pago.



# DMZ / VPN / Access gateways

Es aconsejable evitar exponer a internet interfaces administrativas por lo que podemos negar el acceso si proviene de una red que no es de confianza. Lo que haremos será limitar la escucha de ips de los servidores a una interfaz específica protegida por una DMZ (demilitarized zone) o VPN. Una DMZ es una red separada entre medio de internet y nuestra LAN, podemos colocar allí nuestra web app y controlar mejor el tráfico.



# Intrusion prevention system

Dentro de los IPS (intrusion prevention system) existen algunas alternativas open source, una de las mas conocidas es fail to ban.



## Implementación Fail2Ban

Fail2Ban es un framework escrito en python que protege los sistemas Linux de ataques de fuerza bruta. Entre las múltiples funcionalidades que tiene una de ellas es para proteger los ataques de fuerza bruta en un servidor por SSH. También nos permite monitorear la magnitud de los ataques basado en el número de intentos de autenticación que son realizados.

### :cactus: Instalación

```
sudo apt install fail2ban
```



### :cactus: Estado

Para verificar que está corriendo

```
sudo systemctl status fail2ban
```



### :cactus: Habilitar e Iniciar

```
sudo systemctl enable fail2ban --now
```



### :cactus: Chequear protecciones

Para chequear de qué nos está protegiendo inicialmente

```
sudo fail2ban-client status
```

Debemos verificar que aparezca `- Jail list: sshd`  lo cual significa que fail2ban está controlando ese servicio, chequeando los logs y si ve muchos fallos al ingresar por SSH baneará la IP que está intentando entrar al servidor. 

Podremos obtener detalles acerca de un servicio en particular con:

```
sudo fail2ban-client status sshd
```



### :cactus: Blanquear IP desarrollo

Debemos evitar que banee accidentalmente nuestra dirección IP. podríamos editar de manera directa `/etc/fail2ban/jail.conf` pero no es conveniente ya que en caso de actualizaciones este archivo se pisará. Por ese motivo realizamos una copia (desde el directorio de ese archivo):

```
cd /etc/fail2ban
sudo cp jail.conf jail.local
```

En caso de que exista `jail.local` fail2ban utilizará ese, caso contrario utilizará `jail.conf`. 

Por lo tanto ahora editamos `jail.local` sabiendo que no será sobrescrito:

```
sudo nano jail.local
```



En el campo `ignoreip = 127.0.0.1/8::1` lo aconsejable es descomentarlo y colocar la IP pública desde donde vamos a querer conectarnos de modo que nunca seamos baneados nosotros mismos. También podríamos poner la IP de una VPN desde la cual nos conectamos si fuera este el caso.



Dentro de las configuraciones veremos:

* `bantime = 10m` es decir que la IP será baneada por 10 minutos.

* `maxretry=5` es la cantidad de intentos que puede fallar.



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



