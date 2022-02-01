# Nginx

> [Instalando y configurando Nginx - Parte 1](https://youtu.be/_LQv96MdtCk?list=PLqRCtm0kbeHD7A5f8Yft-5qFg-sgXvGzR) x Pelado Nerd.

Nginx es una herramienta open-source capaz de actuar como servidor web, proxy inverso, balanceador de cargas y cache HTTP.

En su función de servidor web expone un puerto (generalmente el 80 y el 443) y nos permite poner código html en un directorio y mostrarlo como página cuando navegamos a esa dirección.



> Los servidores web por defecto interpretan HTML, para trabajar con PSP, Rails o Python serán necesarios plugins.



## Instalación en Linux

Si estamos utilizando una distribución Ubuntu por ejemplo lo instalamos con el siguiente comando:

```
apt-get install nginx
```

Una vez instalado en Ubuntu se inicia automáticamente. Si navegamos a la IP veremos una página de bienvenida de nginx que nos hará dar cuenta que la instalación fue exitosa.



Podemos verificar la versión instalada con:

```
nginx -v
```



Podremos obtener la ruta al archivo de configuración de nginx con:

```
nginx -t
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



### Instalación Ngnix como Contenedor

```
docker run -d
--name nginx
-p 8080:80
nginx
```

 

> En algunos casos como en el [tutorial de Liv4IT](https://www.youtube.com/watch?v=sGKSfFlKuEI) se usa `-P` para publicar todos los puertos expuestos por el contenedor en el host.



## Reverse Proxy con Docker

De manera similar a lo visto anteriormente queremos implementar un proxy inverso con nginx nos permitirá mostrar el contenido de varios contenedores con un único servidor. 

Para los contenedores utilizaremos la imagen `httpd` para contar con un servidor apache.

```
docker run -dit 
-p 8080:80
--name docker1
httpd:2.4
```

> Notar el uso de `-dit` es decir de `-it` a pesar de estar usando también `-d`. Esto es necesario, según [una respuesta en StackOverflow](https://stackoverflow.com/a/41918607/11384318) si ENTRYPOINT es `bash` o `sh` o si queremos usar `nano` o `vim` con un contenedor en el futuro.



Ingresando a `http://localhost:8080/` veremos la página de bienvenida. Ahora editamos el contenido a vuelo el `index.html` para que en lugar de decir "It Works" diga "Docker1"

```
docker exec 900b... sed -i "s/It works!/Docker1/" /usr/local/apache2/htdocs/index.html
```



Ahora hacemos repetimos todo lo mismo para Docker2 (correr imagen pero usando el `8081` del host y editar el `index.html`). Verificamos que todo está bien ingresando a `http://localhost:8081/`
