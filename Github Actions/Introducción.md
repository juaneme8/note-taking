# GitHub Actions

> Basado de el curso de Platzi dictado por Oscar Barajas (gndx)

[GitHub Actions](https://github.com/features/actions) es una herramienta que nos permitirá automatizar los pasos previos para llevar nuestra aplicación a prod ucción. Podremos crear un flujo de CI/CD logrando pasar por las etapas de Build, Test y Deploy desde GitHub.

Es una herramienta que cuenta con un plan gratuito.



## Características

* Ejecutar un flujo ante cualquier evento de GitHub (push a una rama en particular, creación de un issue, nuevo release, pull request, etc).

* Probar aplicación en distintos sistemas operativos.

* Flujos de trabajo provistos por la comunidad.



VIDEO 1 COMPLETO



## :rocket: Despliegue de Aplicaciones

> Basado en el [directo de Fazt Code](https://youtu.be/azzRDem_p5k) :ok_hand:

GitHub Actions nos proporciona un runner (un programa que corre en segundo plano) que instalamos en el servidor Linux que estará escuchando eventos. Como consecuencia de esto cuando hagamos un push a GitHub, recibirá este evento y ejecutará tareas en el servidor. 

En el repositorio debemos añadir un archivo de configuración con la extensión `*.yaml` como veremos más adelante.



## Creación de Proyecto

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

A continuación vamos a desplegar este proyecto, para eso lo primero que hacemos será crear un repositorio y subir el código a GitHub de la manera habitual.



### Servidor

Es posible que utilizar un servidor propio o un servicio en la nube como Digital Ocean, AWS, Google Cloud Platflorm, etc. Por ejemplo podemos crear un Droplet en DO y conectarnos por ssh. 



### :penguin: IInstalación Zsh 

Vamos a instalar el shell zsh. Recordemos que los shells son programas que nos permiten interactuar con el sistema operativo sin la necesidad de utilizar interfaces gráficas.

Para la instalación de zsh nos basamos en el [siguiente instructivo](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH).

En Windows abrimos WSL2 o Ubuntu y ejecutamos `sudo apt update` y `sudo apt upgrade`.

1. Ejecutar `sudo apt install zsh`

2. Verificar que la instalación se haya efectuado correctamente con `zsh --version` 
3. Convertir a Zsh en el shell por defecto `chsh -s $(which zsh)`.
4. Desloguearnos y volver a loguearnos. Cuando iniciemos nos aparecerá un mensaje de configuración de del archivo de arranque de Z Shell, elegimos la opción 2.
5. Verificar que haya funcionado con `echo $SHELL` que nos arroja algo como `/usr/bin/zsh`.
6. Verificar con `$SHELL --version` que nos arroja algo como `zsh 5.8 (x86_64-ubuntu-linux-gnu)`.



### :penguin: IInstalación Oh My Zsh

Vamos a personalizar a Zsh con Oh My Zsh [de acuerdo a lo indicado en su página web](https://ohmyz.sh/#install)



# Workflow Configuration

Vamos a crear un archivo de configuración en el cual especificamos las tareas que queremos que sean ejecutadas en el servidor. 

Para hacerlo de manera manual podremos crear un directorio `.github/workflows/`

```
mkdir -p .github/workflows
```

> Usamos el `-p` ya que queremos crear un directorio y un subdirectorio a la vez.

Luego creamos un archivo con la configuración en sí que puede tener cualquier nombre pero debe tener la extensión `.yml`, por ejemplo `push.yml`



Podemos hacerlo de manera automática haciendo click en el botón **Actions** ubicado en la barra central y donde dice Workflows podremos ver una serie de configuraciones rápidas para el lenguaje que usemos. 

En  nuestro caso como como se trata de un proyecto de Node debemos buscar "Node.js" y presionamos Configure. Nos creará una carpeta `.github/workflows/node.js.yml` una práctica habitual podría ser cambiarle el nombre al mismo nombre de la rama (en nuestro caso `main.yml`).



Al archivo autogenerado le hacemos algunos cambios:

* Modificamos el `name: Frontend CI`
* Dejamos sólo el on push.
* En `node-version: [14.x, 16.x, 18.x]` como no queremos que lo ejecute en todas las versiones dejamos solo`[16.x]`.
* En el `runs-on: ubuntu-latest` nos da la pauta de que se va a crear un contenedor Ubuntu y allí va a ejecutar el proyecto. Pero en nuestro caso como queremos simplificarlo ponemos `runs-on: self-hosted` ya que queremos que ejecute los comandos directamnete.
* En la parte de run modificamos `npm ci` por `npm i` y quitamos `npm test` (podemos comentarlo con un `#`)

```yaml
# This workflow will do a clean installation of node dependencies, cache/restore them, build the source code and run tests across different versions of node
# For more information see: https://help.github.com/actions/language-and-framework-guides/using-nodejs-with-github-actions

name: Frontend CI

on:
  push:
    branches: [ "main" ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [14.x, 16.x, 18.x]
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



A continuación creamos desde la interfaz gráfica un commit con este archivo. Luego hacemos un pull desde la máquina de desarrollo.



## :penguin: Linux User

Cuando creamos un Droplet (VM de Linux) en DigitalOcean y nos conectamos por ssh, lo haremos con el comando `ssh root@<direccion_ip>` esto significa que estaremos trabajando con el usuario root. Como sabemos esto no es una buena práctica y debemos crear un usuario distinto menos privilegiado.

```
adduser juaneme
```

> Podremos verificar el usuario que estamos usando con `whoami`, o al ver que nos aparece un # en el prompt o al ejecutar `pwd` y ver estamos en la carpeta /root.



Luego modificamos el usuario para agregarlo al grupo sudo:

```
usermod -aG sudo juaneme
```



Luego iniciamos como este usuario con el comando:

su - juaneme



## :penguin: Instalación exa

Exa es una alternativa al comando `ls` **escrita en Rust** con algunas opciones adicionales.

```
sudo apt install exa
```

Una de sus [características](https://the.exa.website/features/icons) principales es la posibilidad de mostrar íconos a la salida.

```
exa --icons
```



## :penguin: Alias en ZSH

Podemos crear un alias agregándolo en `~/.zshrc`.

```
alias l='exa -1 --icons'
alias cat='batcat'
```

 Podremos **recargar la configuración** de dicho archivo con `source ~/.zshrc`.



# Runner

En GitHub si vamos al apartado Settings :arrow_right: Actions :arrow_right: Runners :arrow_right: **New self-hosted runner** podremos descargar, configurar y ejecutar el runner que estará escuchando a los eventos relacionados con GitHub de acuerdo al archivo de configuración recien creado.

Ejecutamos uno a uno los comandos indicados por GitHub en ese apartado hasta completar los tres pasos antes mencionados.

Nos hará una serie de preguntas con respecto al runner que podemos dejar los valores por defecto o bien cambiarle el nombre al runner o al directorio de trabajo que por defecto sugiere `_work` (podemos cambiarlo a `frontend`).

En este momento en el repositorio del proyecto nos indicará que estamos usando GitHub Actions con un circulito. Por ahora estará en  color amarillo y en el apartado de Runners lo veremos como **Offline**.

Si luego ejecutamos `./run.sh` como nos indica en el tercer paso en el apartado de runners lo veremos como **Active**. Sin embargo si cerramos esta ventana ya no estará escuchando.

 En cambio debemos ejecutar `sudo ./svc.sh install` y luego `sudo ./svc.sh start` y quedará corriendo de fondo.

A partir de ahora que tenemos el runner corriendo cada vez que hagamos cambios en el código y los pushiemos al repositorio los veremos reflejados localmente en el servidor y si tenemos un webserver que sirva el contenido de nuestra carpeta `dist` podremos tener una página web que se actualiza automáticamente como veremos a continuación.



### :penguin:Instalación de bat 

bat es una alternativa a cat **escrita en Rust** para mostrar el contenido de un archivo de texto con un formato un poco mas claro en el que se verá un recuadro, números de línea, un formateo con colores, etc.

```
sudo apt install bat
```



## Webserver con Nginx

Queremos que al visitar la IP nos muestre el contenido de nuestra aplicación frontend, entonces debemos configurar un webserver. Vamos a utilizar Nginx.

```
sudo apt install nginx
```



```
sudo systemctl status nginx
```



```
sudo vim /etc/nginx/sites-available/default     
```

Modificar  la línea `root /var/www/html` colocando la ruta de nuestro proyecto ya buildeado `/home/fazt/actions-runner/frontend/twitch-gha/twitch-gha/dist;`



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

> :no_entry: Si vemos un **404** esto se debe a que nginx no tiene permisos de lectura de la carpeta en la cual tenemos el proyecto buildeado. Debemos asignarle permisos `chmod /home/fazt`





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

