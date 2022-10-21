# GitHub Actions

> :link: Basado de el curso de Platzi dictado por Oscar Barajas (gndx) :writing_hand: - VIDEO 1 COMPLETO.
>
> :link: Basado en el [directo de Fazt Code](https://youtu.be/azzRDem_p5k) :ok_hand:
>
> En el video se abordan temas de Instalación de exa (alternativa a ls), bat (alternativa a cat), Zsh, Oh My Zsh cuyas notas fueron agregadas a la carpeta de Linux.

[GitHub Actions](https://github.com/features/actions) es una herramienta que nos permitirá automatizar los pasos previos para llevar nuestra aplicación a prod ucción. Podremos crear un flujo de CI/CD logrando pasar por las etapas de Build, Test y Deploy desde GitHub.

Es una herramienta que cuenta con un plan gratuito.



## Características

* Ejecutar un flujo ante cualquier evento de GitHub (push a una rama en particular, creación de un issue, nuevo release, pull request, etc).

* Probar aplicación en distintos sistemas operativos.

* Flujos de trabajo provistos por la comunidad.



## :rocket: Despliegue de Aplicaciones

GitHub Actions nos proporciona un runner (un programa que corre en segundo plano) que instalamos en el servidor Linux que estará escuchando eventos. Como consecuencia de esto cuando hagamos un push a GitHub, recibirá este evento y ejecutará tareas en el servidor. 

En el repositorio debemos añadir un archivo de configuración con la extensión `*.yaml` como veremos más adelante.



## Creación de Proyecto

<img src="1 Intro y Deploy con Vite.assets/image-20221021083653960.png" alt="image-20221021083653960" style="zoom:50%;" />

```
npm create vite
```

Luego continuamos con la configuración, por ejemplo dándole el nombre **github-actions-frontend**, configurando **Vanilla** y **TypeScript**.

```
cd github-actions-frontend
npm i
npm run dev
```

Luego vamos http://127.0.0.1:5173/ y veremos el programa de ejemplo de Vite.

> Con `npm run build` convertiremos el código para producción y nos creará la carpeta `dist`.



### Creación de Repositorio

Creamos un repositorio en GitHub y pusheamos a el los cambios de la manera habitual.



### Creación VPS 

Es posible que utilizar un servidor propio o un servicio en la nube como Digital Ocean, AWS, Google Cloud Platform, etc. 

En Digital Ocean primero vamos a crear un proyecto que podría agrupar varios servidores  para ellos vamos  a New Project le damos un nombre (luego le daremos un nombre al server en sí), una descripción y por  último vamos a Create -> Droplet. A continuación elegimos el Data Center mas cercano a nuestra ubicación, la versión de Linux, el monto mensual a destinar según la CPU elegida (por ser un proyecto de Next.js se recomienda mínimo 1GB / 1CPU).



### Creación de Llave SSH

Creamos la llave SSH de acuerdo a las instrucciones que nos aparecen en esa sección y cargamos el valor de la llave pública (`id_rsa.pub`) y le asignamos un nombre.



### Conexión a Servidor

 Nos conectamos por SSH a la dirección IP provista por Digital Ocean. Como ya hemos establecido la llave pública podremos conectarnos sin la necesidad de colocar contraseñas.

```
ssh root@<direccion_ip>
```



### Actualizar Servidor

```
sudo apt update && sudo apt upgrade -y
```



## :penguin: Usuario No Root

En ocasiones al conectarnos por SSH estaremos trabajando con el usuario root. Como sabemos esto no es una buena práctica y debemos crear un usuario distinto menos privilegiado.

```
adduser juaneme
```

> Podremos verificar el usuario que estamos usando con `whoami`, o al ver que nos aparece un # en el prompt o al ejecutar `pwd` y ver estamos en la carpeta /root.



Luego modificamos el usuario para agregarlo al grupo sudo:

```
usermod -aG sudo juaneme
```



Luego podemos cambiar a este usuario con el comando:

```
su - juaneme
```



# Creación Workflow

Vamos a crear un archivo de configuración en el cual especificamos los eventos que queremos escuchar en el servidor y las tareas que queremos que sean ejecutadas cuando esto suceda. 

Esto podemos hacerlo de manera manual o automática. Para hacerlo **de manera manual** tenemos que crear un directorio `.github/workflows/` y adentro el archivo yml.

```
mkdir -p .github/workflows
```

> Usamos el `-p` ya que queremos crear un directorio y un subdirectorio a la vez.

Luego creamos un archivo con la configuración en sí que puede tener cualquier nombre pero debe tener la extensión `.yml`, por ejemplo `push.yml`



Si queremos hacerlo **de manera automática** debemos hacer click en el botón **Actions** ubicado en la barra central y donde dice Workflows podremos ver una serie de configuraciones rápidas para el lenguaje que usemos. 

En  nuestro caso como como se trata de un proyecto de Node debemos buscar **"Node.js"** y presionamos **Configure**. Nos creará una carpeta `.github/workflows/node.js.yml` una práctica habitual podría ser cambiarle el nombre al mismo nombre de la rama (en nuestro caso `main.yml`).



Al archivo autogenerado le hacemos algunos cambios:

* Modificamos el `name: Frontend CI`
* Edtamos el archivo de manera tal que sólo estemos escuchando push y quitamos la escucha de pull requests.

```yaml
on:
  push:
    branches: [ "main" ]
```

* En `node-version: [14.x, 16.x, 18.x]` con lo cual indicamos que queremos que sea probado en distintas versiones de Node y como no queremos que lo ejecute en todas las versiones dejamos solo`[16.x]`. Tener presente que al momento de escribir este tutorial [la versión 16 es la LTS](https://nodejs.org/en/).
* En el `runs-on: ubuntu-latest` nos da la pauta de que se va a crear un contenedor Ubuntu y allí va a ejecutar el proyecto. Pero en nuestro caso queremos que los comandos se ejecuten en el servidor por lo que ponemos  `runs-on: self-hosted` .
* En la parte de run modificamos `npm ci` por `npm i` y quitamos `npm test` (podemos comentarlo con un `#`) ya que por el momento no tenemos tests.

```yaml
name: Frontend CI

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
```



A continuación creamos desde la interfaz gráfica un commit con este archivo haciendo click en **Start commit**. Luego hacemos un pull desde la máquina de desarrollo para traernos estos cambios. 

# Runner

En GitHub si vamos al apartado Settings :arrow_right: Actions :arrow_right: Runners :arrow_right: **New self-hosted runner** podremos descargar, configurar y ejecutar el runner que estará escuchando a los eventos relacionados con GitHub de acuerdo al archivo de configuración recien creado.

Elegimos Linux como sistema operativo y vamos ingresando uno a uno los comandos indicados por GitHub en ese apartado. 

Cuando llegamos a la parte de ejecutar `./config.sh` nos hará una serie de preguntas con respecto al runner que podemos dejar los valores por defecto o bien cambiarlos. Nos preguntará por ejemplo si queremos agregar este runner a un grupo, el nombre que queremos darle, tags, etc.

Por último nos preguntará si queremos que la carpeta de trabajo se llame  `_work` (lo ideal es cambiarlo por el nombre del proyecto, ejemplo a `frontend`). Es en esa carpeta donde clonará el repositorio.

> Nuestro código estará luego del clonado en `actions-runner/_work/nombreRepo/nombreRepo`

> En este momento en el repositorio del proyecto nos indicará que estamos usando GitHub Actions con un circulito. Por ahora estará en  color amarillo y en el apartado de Runners lo veremos como **Offline**.

Si luego ejecutamos `./run.sh` como nos indica en el tercer paso en el apartado de runners lo veremos como **Active**. Sin embargo si cerramos esta ventana ya no estará escuchando.

## Ejecución Permanente

Para lograr que el runner se mantenga escuchando eventos de manera permanente debemos ejecutar:

```bash
./svc.sh
```

Nos aparecerá un mensaje "Must run as sudo" por lo que ejecutamos:

```bash
sudo ./svc.sh
```

 En ese momento nos dirá que los comandos que puede instalar son `install`, `start`, `stop`, `status` y `uninstall` por queremos conocer su estado:

```bash
sudo ./svc.sh status
```

En se momento veremos "not installed" por lo que lo instalamos:

```bash
sudo ./svc.sh install
```

Si verificamos su estado con `sudo ./svc.sh status` nos aparecerá como "inactive", por lo que vamos a iniciarlo:

```bash
sudo ./svc.sh start
```

A partir de ahora quedará corriendo de fondo en GitHub lo veremos como "Idle" mientras que si ejecutamos `sudo ./svc.sh stop` lo veremos como "Offline".

## Conclusión

Tenemos el runner corriendo cada vez que hagamos cambios en el código y los pushiemos al repositorio los veremos reflejados localmente en el servidor y si tenemos un webserver que sirva el contenido de nuestra carpeta `dist` podremos tener una página web que se actualiza automáticamente como veremos a continuación.



## Webserver con Nginx

> Ver Parte 2 para su funcionamiento como servidor proxy.

Queremos que al visitar la IP nos muestre el contenido de nuestra aplicación frontend, entonces debemos configurar un webserver. Vamos a utilizar Nginx.

Para instalar nginx ejecutamos:

```
sudo apt install nginx
```

Para verificar el estado:

```
sudo service nginx status
```

```
sudo systemctl status nginx
```

En este momento veremos que nginx está activo por lo que si entramos con el navegador a la dirección IP del servidor, nos aparecerá la página de bienvenida.



## Servir Archivos Estáticos

Anteriormente hemos realizado el build de nuestra aplicación de Vite y ahora queremos que nos muestre ese contenido en lugar de la página de bienvenida. Para ello debemos modificar el siguiente archivo:

```
sudo vim /etc/nginx/sites-available/default     
```



Cambiar la línea `root /var/www/html` colocando la ruta de nuestro proyecto ya buildeado `/home/fazt/actions-runner/frontend/twitch-gha/twitch-gha/dist;`



Verificar que no haya errores con

```
sudo nginx -t
```



Reiniciamos nginx

```
sudo systemctl restart nginx
```

Veremos el estado con

```
sudo systemctl status nginx
```

> :no_entry: Si vemos un **404** esto se debe a que nginx no tiene permisos de lectura de la carpeta en la cual tenemos el proyecto buildeado. Debemos asignarle permisos cosa que hacemos con:
>
> ```
> chmod 755 /home/fazt
> ```
>
> 





## Variables de Entorno

Como podemos ver en la [documentación](https://vitejs.dev/guide/env-and-mode.html) de Vite para crear variables de entorno debemos crear un archivo `.env.local` (con esto además logramos que sea ignorado por git cosa que con un `.env` habría que añadirlo al `.gitignore`) y dentro de el las variables de entorno que queremos tener:

```
VITE_APP_TITLE=My App
VITE_APP_API=https://jsonplaceholder.typicode.com/users
```

Luego en el código podremos acceder a estos datos por ejemplo con `import.meta.env.VITE_APP_API`.



Para configurar los valores que queremos que tengan las variables de entorno para producción lo hacemos desde el apartado Settings :arrow_right: Secrets :arrow_right: Actions y luego hacemos click en New Repository Secret. Supongamos que le damos el nombre `ViteAppApi`



Luego en el `workflow` modificamos agregando la variable de entorno:

```yaml
    steps:
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    - run: npm i
    - run: npm run build --if-present
      env:
           VITE_APP_API: ${{ secrets.ViteAppApi }}
```

