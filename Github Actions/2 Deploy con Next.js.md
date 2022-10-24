# CI/CD con Next.js

> :link: Basado en el [video de Fazt](https://youtu.be/pgoZFKgoiDk) :ok_hand:

## Esquema del Proyecto

Vamos a crear un proyecto de Next.js que luego vamos a subirlo a un repositorio de GitHub. A continuación vamos a clonarlo en nuestro servidor de producción que será el encargado de estar corriendo siempre nuestra aplicación. 



## Despliegue Manual

> En el caso del curso un utilizaremos un VPS (virtual private server) del cloud provider Digital Ocean, pero aplica lo mismo para nuestor propio servidor.

<img src="2 Deploy con Next.js.assets/image-20221020093535015.png" alt="image-20221020093535015" style="zoom:50%;" />



Queremos que este flujo se ejecute de manera automática con GitHub Actions para así no perder tiempo haciendo el despligue manualmente cada vez que hagamos modificaciones al código.



## Despliegue Automático

La automatización mediante GitHub Actions la logramos creando un workflow que consiste en un archivo de configuración con la extensión `yml` en el cual le daremos indicaciones de los pasos que queremos que realice en el servidor. En el servidor estaremos pendientes de los cambios del código (un nuevo push por ejemplo) mediante un un programa que nos proporciona GitHub llamado runner.

<img src="2 Deploy con Next.js.assets/image-20221020093616654.png" alt="image-20221020093616654" style="zoom:50%;" />



## Project Setup

```bash
npx create-next-app nextga-tutorial
```

```bash
cd nextga-tutorial
```

```bash
npm run dev
```



> En el video también se aborda la creación del repositorio, la configuración de la VPS con Digital Ocean y de usuarios menos privilegiados. Muchos de estos conceptos ya habían sido incluídos en la introducción por lo que se complementan esas notas. 
>
> La creación del workflow también es idéntica a la vista anteriormente.



# pm2

Una vez que hemos hecho el buildeo por estar utilizando Next.js debemos ejecutar `npm start` pero queremos que se mantenga la ejecución de la aplicación de manera permanente sin que nos quede bloqueada la terminal y no podamos cerrarla.

Para ello vamos a utilizar [pm2](https://pm2.keymetrics.io/) que nos permite ejecutar en segundo plano nuestros programas. 

## Instalación nvm

Primero vamos a instalar [nvm](https://github.com/nvm-sh/nvm) que nos permitirá instalar múltiples versiones de node.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash
```

A continuación debemos ejecutar `nvm` y si nos muesta "nvm not found" debemos cerrar sesión y volver a iniciarla. Otra opción es ejecutar `source ~/.bashrc` o el archivo de configuración del shell que estemos usando. Si hora ejecutamos `nvm` veremos una salida que incluirá la versión que tenemos instalada.

```bash
nvm install --lts
```

> Con lts nos aseguramos la versión long term service.

Luego podemos verificar que la instalación fue exitosa con 

```bash
node --version
npm --version
```



## Instalación pm2

```bash
npm install -g pm2
```

Verificamos que la instalación fue correcta con:

```bash
pm2 --version
```



La idea será ejecutar `npm start` de Next.js de manera tal que continúe ejecutándose siempre, para eso primero ponemos el comando de pm2 `pm2 start` y a continuación `npm -- start`

```
pm2 start npm -- start
```


Para listar los procesos que se están ejecutando:

```
pm2 list
```

Para conocer los logs

```
pm2 logs
```



## Modificar Workflow para Recargar

 Queremos que cada vez que hagamos un push se instalen las dependencias, se realice el build y además que se recargue el proceso.

Esto podríamos hacerlo desde la terminal con:

```
pm2 reload 0
```



Ahora queremos hacer esto mismo de manera automatizada por lo que lo colocamos en el workflow:

```yaml
name: node.js

on:
  push:
    branches: [ "main" ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16.x]
        # See supported Node.js release schedule at https://nodejs.org/en/about/releases/

    steps:
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    - run: npm i
    - run: npm run build --if-present
    - run: pm2 0 reload
```

> Sabemos que el programa tiene un id 0 cosa que hemos verificado con `pm2 list`



Cuando ejecuta el último paso nos aparecerá **"pm2: command not found"** a pesar de que si en la consola escribimos `pm2 --version` vemos que lo tenemos instalado.



Debemos recurrir [a la siguiente solución de StackOverflow](https://stackoverflow.com/questions/69644460/github-actions-pm2-command-not-found) que nos indica que está faltando el symbolic link entre node y pm2. Para solucionarlo debemos ejecutar:

```yaml
sudo ln -s "$NVM_DIR/versions/node/$(nvm version)/bin/node" "/usr/local/bin/node"

sudo ln -s "$NVM_DIR/versions/node/$(nvm version)/bin/npm" "/usr/local/bin/npm"

sudo ln -s "$NVM_DIR/versions/node/$(nvm version)/bin/pm2" "/usr/local/bin/pm2"
```



## Re-run jobs

En ocasiones vamos a querer volver a ejecutar las tareas aunque no hagamos ningún push. En ese caso podemos presionar el botón **Re-run jobs :arrow_right: Re run failed jobs** que nos encontramos dentro del repositorio en el apartado del menú central Actions.



## Webserver con Nginx (Parte 2)

> **Ver Parte 1 para la instalación.**

Si ahora nos metemos a la dirección_ip:3000 veremos la aplicación funcionando y aunque cerremos la terminal continuará ejecutándose. Queremos que en lugar de usar el puerto 3000 podamos utilizar el 80 (entrando a través de la dirección ip de manera directa).



Vamos a utilizar nginx como servidor proxy y lo instalamos de manera similar a lo visto.



## Creación de archivo

```
cd /etc/nginx/sites-available
```

Veremos que allí dentro tenemos el archivo `default` que hemos editado en la **Parte 1**



```
sudo nano next
```

> Le damos a modo de referencia el nombre next pero podría haber sido cualquier otro.



```
server{
 listen 80;
 listen [::]:80;
 server_name _;
 
 location /{
  proxy_pass http://localhost:3000;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Host $host;
  proxy_cache_bypass $http_upgrade;
 }
}
```



Con el comando anterior lo que estamos diciendo es que cuando se ejecute el puerto 80  vamos a estar sirviendo en la dirección por defecto del servidor y vamos a mostrar lo que esté en el puerto 3000.

Con `listen [::]:80;` nos aseguramos el funcionamiento para direcciones ipv6, de modo que funcionará tanto para ipv4 como para ipv6.

Con `sudo nginx -t` verificamos que la configuración sea exitosa, luego `sudo service nginx restart` y `sudo nginx status` para verificar que esté activo.



## Creación Symbolic Link

A continuación debemos copiar el archivo que hemos creado de `sites_available` a `sites_enabled pero para evitar tener que estar copiando y pegando cada vez que hacemos un cambio, vamos a crear un **symlink**, que se comporta como un archivo que está en dos lugares.

```bash
sudo ln -s /etc/nginx/sites-available/next /etc/nginx/sites-enabled/next
```

A continuación debemos editar el archivo `default` y comentar la línea:

```nginx
# server_name _;
```



## Editar default

En el archivo `default` (en `sites_available` y con una referencia mediante symlink en `sites_enabled`) también estamos escucuchando en el puerto 80 (como en el archivo `next`) lo cual va a hacer que no funcione. Cambiamos este valor a `8080` para solucionarlo.



Reiniciamos el servicio con `sudo service nginx restart` y vamos a la dirección IP y todo debería funcionar.



## Creación de Dominio

Podemos utilizar un provedor de dominios como namecheap   

En Digital Ocean creamos los DNS records.

```
@
www
```

Luego obtenemos las tres direcciones de DNS y los cargamos en namecheap.



## Reconfigurar Nginx

Ahora no estaremos sirviendo desde una IP sino que lo haremos desde un dominio.



```
sudo nano /etc/nginx/sites-available/next
```

```nginx
server{
 listen 80;
 listen [::]:80;
 server_name www.fazt.site fazt.site;
 
 location /{
  proxy_pass http://localhost:3000;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Host $host;
  proxy_cache_bypass $http_upgrade;
 }
}
```

Modificamos `server_name _;` ya que no queremos que accedan ahora a través de la dirección IP sino a través del nombre del dominio.

Luego verificamos con `sudo nginx -t` y reiniciamos `sudo service nginx restart`



## Certificado SSL

Vamos a utilizar [https://letsencrypt.org/getting-started/](https://letsencrypt.org/getting-started/) para generar un certificado que nos permitirá encriptar nuestras comunicaciones. Lo vamos a hacer utilizando [certbot](https://certbot.eff.org/) que nos permitirá generar el certificado de manera automática.

Elegimos nginx en Ubuntu y seguimos los pasos indicados.

Se encargará el bot también de alterar la configuración de nginx que tenemos actualmente.

```
sudo certbot --nginx -d fazt.site -d www.fazt.site
```

Ingresamos un correo para que nos notifique el vencimiento y luego aceptamos los TOS.