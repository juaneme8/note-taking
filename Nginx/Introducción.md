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



## Introducción

Nginx es una herramienta open-source capaz de actuar como servidor web, proxy inverso, balanceador de cargas y cache HTTP.

En su función de servidor web expone un puerto (generalmente el 80 y el 443) y nos permite poner código html en un directorio y mostrarlo como página cuando navegamos a esa dirección.



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
