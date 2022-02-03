# Nginx

* Video introductorio a nginx:
  [Instalando y configurando Nginx - Parte 1](https://youtu.be/_LQv96MdtCk?list=PLqRCtm0kbeHD7A5f8Yft-5qFg-sgXvGzR) x Pelado Nerd.

* Instalación de Nginx como contenedor Docker:
  [How To Deploy NGINX With Docker On Ubuntu Linux](https://youtu.be/sGKSfFlKuEI)

* Blog post sobre Nginx en Docker y flujo habitual de proxy inverso:
  https://www.docker.com/blog/how-to-use-the-official-nginx-docker-image/

* Video de Traversy Media origentado en deployar una app Node.js:
  [Full Node.js Deployment - NGINX, SSL With Lets Encrypt](https://youtu.be/oykl1Ih9pMg)

* Video Similar al anterior pero usando Docker instalable en Ubuntu:

  [Deploy Web App with Docker, Nginx and SSL](https://www.youtube.com/watch?v=zJPlyjfV4C0)

* Nginx beginner's guide

  http://nginx.org/en/docs/beginners_guide.html

* Información sobre el achivo de configuración

  https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/

## Introducción

Nginx es una herramienta open-source capaz de actuar como servidor web, proxy inverso, balanceador de cargas y cache HTTP.



> *master procesess* y *worker processes*

Nginx tiene un *master process* y varios *worker processes*. El *master process* se encargar de leer y evaluar la configuración y de mantener a los *worker processes* siendo estos quienes se encargan de procesar los requests. Nginx tiene un mecanismo que le permite distribuir eficientemente los requets entre los *worker processes*. El número de *worker processes* está definido en el archivo de configuración y puede ser fijo o variable según la cantidad de CPU cores disponibles.



> Archivo de configuración

El modo en que nginx funciona está determinado en su archivo de configuración. Por defecto recibe el nombre de `nginx.conf`.



> Señales de control

Para iniciarlo ejecutamos el ejecutable `nginx` y una vez iniciado podemos controlarlo con señales.

```
nginx -s signal
```

Las señales pueden ser las siguientes:

* `stop` — fast shutdown

* `quit` — para detener los procesos esperando a que los *worker processes* terminen de servir los requets actuales.

* `reload` — para recargar el archivo de configuración. Los cambios realizados en el archivo de configuración no se aplicarán hasta que no enviemos el comando de recargar. 

  > Once the master process receives the signal to reload configuration, it checks the syntax validity of the new configuration file and tries to apply the configuration provided in it. If this is a success, the master process starts new worker processes and sends messages to old worker processes, requesting them to shut down. Otherwise, the master process rolls back the changes and continues to work with the old configuration. Old worker processes, receiving a command to shut down, stop accepting new connections and continue to service current requests until all such requests are serviced. After that, the old worker processes exit.


* `reopen` — reopening the log files



> Servidor Web

Funcionando como **servidor web** expone un puerto (generalmente el 80 y el 443) y nos permite poner código html en un directorio y mostrarlo como página cuando navegamos a esa dirección.



> Los servidores web por defecto interpretan HTML, para trabajar con PSP, Rails o Python serán necesarios plugins.



## Instalación en Linux

A la hora de trabajar con Nginx una de las alterrnativas es instalarlo en el host, para ello si estamos utilizando una distribución Ubuntu podemos instalarlo con el siguiente comando:

```
apt-get install nginx
```

Una vez instalado en Ubuntu se inicia automáticamente. Si navegamos a la `http://localhost:8080` veremos una página de bienvenida de nginx que nos hará dar cuenta que la instalación fue exitosa.



Podemos verificar la versión instalada con:

```
nginx -v
```



Podremos obtener la ruta al archivo de configuración de nginx con:

```
nginx -t
```



## Instalación en Windows

Debemos dirigirnos a la página de descarga de nginx para windows: http://nginx.org/en/docs/windows.html

```
cd c:\
unzip nginx-1.21.6.zip
cd nginx-1.21.6
start nginx
```

Luego nos dirigimos a `localhost` y veremos la página de bienvenida.



### Posibles Problemas

El puerto 80 entra en conflicto con algún servicio y esto hace que no sea posible iniciar nginx.

En [StackOverflow](https://stackoverflow.com/questions/1430141/port-80-is-being-used-by-system-pid-4-what-is-that) nos encontramos con esta solución para detener y deshabilitar W3SVC.

```
sc stop w3svc
sc config w3svc start= disabled
```



## Archivo de Configuración

Como dijimos anteriormente el modo en que nginx funciona está determinado en su archivo de configuración `nginx.conf` y dependiendo del sistema de paquetes usado para instalar nginx podrá estar en:

* `/etc/nginx`
* `/usr/local/nginx/conf`
* `/usr/local/etc/nginx`

Los comentarios se hacen con `#`.

Para simplificar el mantenimiento se recomienda dividir en distintos archivos la configuración y ubicarlos en `/etc/nginx/conf.d` y usar la directiva `include` en `nginx.conf`. 

```
include conf.d/http
```

Nginx consiste en módulos que son controlados las **directivas** ubicadas en este archivo. Las directivas se dividen en **directivas simples** y **directivas de bloque**. 

* Las simples son de una línea, consisten en nombre y parámetro separadas por espacios y que terminan en punto y coma `;`. 

* Las directivas de bloque tienen la misma estructura que las simples pero en lugar del punto y coma, terminan con un set adicional de instrucciones rodeadas por llaves `{}`. Si una directiva de bloque puede tener otras directivas dentro de las llaves se llama **contexto** (por ejemplo `http`, `events`, `server`, `location`). Las directivas ubicadas fuera de todo contexto se considera que estan en el contexto principal (main context). Las directivas `events` y `http` reciden en el main context, `server` en `http` y `location` en `server`.

  ```
  http {
      server {
      	location / {
      	
      	}
      }
  }
  ```




#### Configuración Server

An important web server task is serving out files (such as images or static HTML pages). You will implement an example where, depending on the request, files will be served from different local directories: `/data/www` (which may contain HTML files) and `/data/images` (containing images). This will require editing of the configuration file and setting up of a [server](http://nginx.org/en/docs/http/ngx_http_core_module.html#server) block inside the [http](http://nginx.org/en/docs/http/ngx_http_core_module.html#http) block with two [location](http://nginx.org/en/docs/http/ngx_http_core_module.html#location) blocks.

```
http{
    server {
        location / {
        	root /data/www;
        }

        location /images/ {
        	root /data;
        }
	}
}
```

This is already a working configuration of a server that **listens on the standard port 80** (that's why the `listen` directive is not specified) and is accessible on the local machine at `http://localhost/`. In response to requests with URIs starting with `/images/`, the server will send files from the `/data/images` directory. For example, in response to the `http://localhost/images/example.png` request nginx will send the `/data/images/example.png` file. If such file does not exist, nginx will send a response indicating the 404 error. Requests with URIs not starting with `/images/` will be mapped onto the `/data/www` directory. For example, in response to the `http://localhost/some/example.html` request nginx will send the `/data/www/some/example.html` file.

> 

In case something does not work as expected, you may try to find out the reason in `access.log` and `error.log` files in the directory `/usr/local/nginx/logs` or `/var/log/nginx`.



#### Configuración Proxy Server

One of the frequent uses of nginx is setting it up as a proxy server, which means a server that receives requests, passes them to the proxied servers, retrieves responses from them, and sends them to the clients.

We will configure a basic proxy server, which serves requests of images with files from the local directory and sends all other requests to a proxied server. In this example, both servers will be defined on a single nginx instance.

```
http{
    server {
    	location / {
    		proxy_pass http://localhost:8080;
    	}

        location ~ \.(gif|jpg|png)$ {
        	root /data/images;
        }
	}
}
```

This server will filter requests ending with `.gif`, `.jpg`, or `.png` and map them to the `/data/images` directory (by adding URI to the `root` directive’s parameter) and pass all other requests to the proxied server configured above.



## Configuración 

La configuración de nginx la veremos en `/etc/nginx`:

* `sites-available` los sitios disponibles.
* `sites-enabled` los sitios habilitados.



## Reverse Proxy

Queremos contar con un proxy inverso que nos permita tener dos **server blocks** o **virtual hosts**. Tendremos por ejemplo `hola.juaneme8.com` y `chau.juaneme8.com` ambos apuntando al mismo servidor pero nginx es capaz de diferenciarlos y mostrar el contenido correspondiente para cada uno.



#### Modificamos `sites-available` 

En la carpeta `sites-available` donde almacenamos todos los sitios que vamos a correr en este servidor. 

Tenemos un archivo `default` que viene por defecto configurado para mostrar la página de bienvenida. 

Creamos un nuevo archivo para cada virtual host (sobre la base de `default` y la práctica común es ponerle el mismo nombre del subdominio por ejemplo `hola.domain.com` de modo que sea más fácil identificarlo. 

```
cp default hola.juaneme8.com
cp default chau.juaneme8.com
```

Luego en ambos archivos hacemos los siguientes cambios:

1. Donde dice `listen 80 default_server;` como sólo podemos tener un default server dejamos `listen 80;` 

   > El servidor default es aquel que se muestra si entramos por ejemplo a `hi.domain.com` cuando no tenemos ningún sitio que matchee con esa dirección.

2. Donde tenemos`server name _;` debemos poner `hola.domain.com`

2. Donde tenemos `root /var/www/html` debemos indicar donde estarán los archivos de ese virtual host por ejemplo `/var/www/hola`.



#### Modificamos `sites-enabled`

Si ahora vamos a `sites-enabled` veremos un **link simbólico**, es decir un puntero a un archivo. La idea es crear links simbólicos de aquellos sitios habilitados para no tener que borrar el archivo cada vez que los deshabilitemos.

La forma de crear links simbólicos es:

```
ln -s /ruta/origen/archivo /ruta/destino/archivo
```



Desde `sites-enabled`:

```
ln -s ../sites-available/hola.juaneme8.com .
```

```
ln -s ../sites-available/chau.juaneme8.com .
```



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



## Ngnix en Contenedor

Anteriormente analizamos la instalación de Nginx en el host, otra opción es ejecutarlo en un contenedor.

```
docker run -d
--name nginx
-p 8080:80
nginx
```

 

> En algunos casos como en el [tutorial de Liv4IT](https://www.youtube.com/watch?v=sGKSfFlKuEI) se usa `-P` para publicar todos los puertos expuestos en el contenedor en puertos random del host. Esto puede ser útil si vamos a ir ejecutando `docker run ...` varias veces cambiando el nombre y queremos obtener un puerto distinto para evitar tener que estar deteniendo y eliminado los contenedores (o asignándole un nuevo puerto).



En ese momento si visitamos `http://localhost:8080` veremos la página de bienvenida. Esto también podemos verificarlo desde la terminal:

```
curl http://localhost:8080
```



## Contenido personalizado

Por defecto Nginx se fija en `/usr/share/nginx/html` los archivos que va a servir.

Si queremos mostrar  con contenido personalizado podemos hacerlo de diferentes formas.

1. Una forma es con `docker exec` y el **stream editor** `sed` para editarlo al vuelo con el siguiente patrón `"s/regexp/replacement/flags"`:

```
docker exec 2e8 sed -i "s/Welcome to nginx!/Nuevo contenido/" /usr/share/nginx/html/index.html
```

> [Más información sobre sed stream editor](https://stackoverflow.com/questions/12696125/sed-edit-file-in-place)



2. Otra forma es con un **volumen**, suponemos que tenemos nuestro archivo `index.html` en `C:\Users\JuaNeMe\Documents\Code\ngnix-example`

```
docker run -d 
--name nginx 
-p 8080:80 
-v C:\Users\JuaNeMe\Documents\Code\ngnix-example:/usr/share/nginx/html 
nginx
```



### Imagen Custom de Nginx

Lo que hicimos anteriormente con un volumen es válido para trabajar localmente y compartir archivos con el contenedor, pero es probable que queramos enviar la imagen con los archivos html incluidos.

Vamos a crear un imagen personalizada a partir de la imagen base de `nginx` creamos un `Dockerfile` .

```
FROM nginx:latest
COPY ./index.html /usr/share/nginx/html/index.html
```

> Este `Dockerfile` suponemos que lo tenemos en una carpeta del proyecto y que a su vez tiene en la raíz el `index.html` personalizado.



Luego creamos la imagen de acuerdo al `Dockerfile`

```
docker build -t nginx .
```



Por último ejecutamos la imagen pero sin necesitar el volumen.

```
docker run -d 
--name nginx 
-p 8080:80 
nginx
```



## Reverse Proxy con Docker

Queremos implementar un proxy inverso con nginx nos permitirá mostrar el contenido de varios contenedores con un único servidor. 

Para los contenedores utilizaremos la imagen `httpd` para contar con un servidor Apache.

```
docker run -dit 
-p 8081:80
--name docker1
httpd:2.4
```

> Notar el uso de `-dit` es decir de `-it` a pesar de estar usando también `-d`. Esto es necesario, según [una respuesta en StackOverflow](https://stackoverflow.com/a/41918607/11384318) si ENTRYPOINT es `bash` o `sh` o si queremos usar `nano` o `vim` con un contenedor en el futuro.



Ingresando a `http://localhost:8080/` veremos la página de bienvenida. Ahora editamos el contenido a vuelo el `index.html` para que en lugar de decir "It Works" diga "Docker1"

```
docker exec 900b... sed -i "s/It works!/Docker1/" /usr/local/apache2/htdocs/index.html
```



Ahora hacemos repetimos todo lo mismo para Docker2 (correr imagen pero usando el `8082` del host y editar el `index.html`). Verificamos que todo está bien ingresando a `http://localhost:8082/`



#### Chequear flujo de trabajo en Windows Reverse Proxy

En lugar de manejarnos con `sites-enabled` y `sites-available`, por practicidad para no hacer una estrucutura espcial para Windows editamos en principio el archivo `nginx.conf` ubicado en el directorio `conf`, agregando lo siguiente:

```
 server{
        listen      80;
        server_name docker1.jnm.com;

        location /{
            proxy_pass http://localhost:8081;
        }
    }
    server{
        listen 80;
        server_name docker2.jnm.com;

        location /{
            proxy_pass http://localhost:8082;
        }
    }
```

Luego de modificarlo tendremos qeu volver a cargar nginx, lo cual hacemos con:

```
.\nginx.exe -s reload
```

Debemos recargarlo en el mismo usuario que lo inició.



En el `C:\Windows\System32\drivers\etc` buscamos el archivo `hosts` y con privilegios de root agregamos lo siguiente:

```
181... docker1.jnm.com docker2.jnm.com
```
