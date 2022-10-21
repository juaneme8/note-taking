# CI/CD con Next.js

> :link: Basado en el [video de Fazt](https://youtu.be/pgoZFKgoiDk)

## Esquema del Proyecto

Vamos a crear un proyecto de Next.js que luego vamos a subirlo a un repositorio de GitHub. A continuación vamos a clonarlo en nuestro servidor de producción que será el encargado de estar corriendo siempre nuestra aplicación. 



## Despliegue Manual

> En el caso del curso un utilizaremos un VPS (virtual private server) del cloud provider Digital Ocean, pero aplica lo mismo para nuestor propio servidor.

<img src="1 Intro y Deploy con Vite/image-20221020093535015.png" alt="image-20221020093535015" style="zoom:50%;" />



Queremos que este flujo se ejecute de manera automática con GitHub Actions para así no perder tiempo haciendo el despligue manualmente cada vez que hagamos modificaciones al código.



## Despliegue Automático

La automatización mediante GitHub Actions la logramos creando un workflow que consiste en un archivo de configuración con la extensión `yml` en el cual le daremos indicaciones de los pasos que queremos que realice en el servidor. En el servidor estaremos pendientes de los cambios del código (un nuevo push por ejemplo) mediante un un programa que nos proporciona GitHub llamado runner.

<img src="1 Intro y Deploy con Vite/image-20221020093616654.png" alt="image-20221020093616654" style="zoom:50%;" />



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

## Navegación 

Si ahora nos metemos a la dirección_ip:3000 veremos la aplicación funcionando y aunque cerremos la terminal continuará ejecutándose. Queremos que en lugar de usar el puerto 3000 podamos utilizar el 80.