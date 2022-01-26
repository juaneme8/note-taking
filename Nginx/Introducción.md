# Nginx

> [Instalando y configurando Nginx - Parte 1](https://youtu.be/_LQv96MdtCk?list=PLqRCtm0kbeHD7A5f8Yft-5qFg-sgXvGzR) x Pelado Nerd.

Es un servidor web, entendiendo como tal un servicio que corre y expone un puerto (generalmente el 80 y el 443) y nos permite poner código html en un directorio y mostrarlo como página cuando navegamos a esa dirección.

Los servidores web por defecto interpretan HTML, para trabajar con php, rails o python serán necesarios plugins para que funcionen.



## Instalación en Linux

Si estamos utilizando una distribución Ubuntu por ejemplo lo instalamos con el siguiente comando:

```
apt-get install nginx
```

Una vez instalado en Ubuntu se inicia automáticamente. Si navegamos a la IP veremos una página de bienvenida de nginx que nos hará dar cuenta que la instalación fue exitosa.

Para configurarlo para que muestre nuestro sitio `/etc/nginx` (recordemos que en la mayoría de programas tenemos los archivos de configuración en `/etc`)

Veremos los directorios `sites-available` y `sites-enabled`

En la carpeta `sites-available` donde almacenamos todos los sitios que vamos a correr en este servidor nginx (podrán ser de distinto tipo HTML puro, php, python, etc). Estos sitios van a estar diferenciados por lo que se llama **virtual host**. Si bien todo apunta al mismo servidor, nginx es capaz de diferenciarlo.

Por ejemplo podemos tener `hola.juaneme8.com` y `chau.juaneme8.com` ambo estarán apuntando al mismo servidor pero mostrando distinto contenido.

En `sites-available` tenemos un archivo `default` que viene por defecto configurado para mostrar la página de bienvenida.

Creamos un nuevo archivo y si bien puede tener cualquier nombre, es práctica común ponerle el mismo nombre del subdominio por ejemplo `hola.domain.com` de modo que sea más fácil saber sus funciones. 

Creamos estos dos archivos sobre la base de `default`.

```
cp default hola.juaneme8.com
cp default chau.juaneme8.com
```



1. Donde dice `listen 80 default_server;` como sólo podemos tener un default server dejamos `listen 80;` 

   > El servidor default es aquel que se muestra si entramos por ejemplo a `hi.domain.com` cuando no tenemos ningún sitio que matchee con esa dirección.

2. Donde tenemos`server name _;` debemos poner `hola.domain.com`

2. Donde tenemos `root /var/www/html` debemos indicar donde estarán los archivos por ejemplo `/var/www/hola`

> Modificamos los dos archivos con este criterio.



Si ahora vamos a `sites-enabled` veremos un **link simbólico**, es decir un puntero a un archivo. La idea es crear links simbólicos de aquellos sitios habilitados para no tener que borrar el archivo cada vez que los deshabilitemos.

La forma de crear links simbólicos es:

```
ln -s /ruta/origen/archivo /ruta/destino/archivo
```



Desde `sites-enabled`:

```
ln -s ../sites-available/hola.juaneme8.com .
```

> Repetimos lo mismo para `chau.domain.com`



Recargamos nginx para que tome la nueva configuración.

```
nginx -s reload
```



Es importante lograr que `hola.juaneme8.com` y `chau.juaneme8.com` **apunten a la IP del servidor**.
Esto lo podemos simular en nuestra PC para que piense que resuelve esta IP, modificando el archivo hosts en `/etc/hosts` en Linux o en `C:\Windows\System32\drivers\etc`  en Windows.



```
94.237.84... hola.juaneme8.com chau.juaneme8.com
```



Si visitamos `hola.juaneme8.com` pero no tenemos creada la carpeta `/var/www/hola` nos aparecerá un 404. Si en cambio tenemos la carpeta creada pero no tenemos un archivo `index.html` veremos un **403 Forbidden** y esto se debe a que encontró el directorio pero al no haber encontrado el archivo `index.html` quiso listar los archivos de ese directorio y por defecto Nginx no hace esto a menos que lo configuremos.

Si luego agrego estos archivos no tendré que recargar Nginx porque no estoy cambiando el archivo de configuración.