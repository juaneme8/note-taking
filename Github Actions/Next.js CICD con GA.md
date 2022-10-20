# CI/CD con Next.js

> :link: Basado en el [video de Fazt](https://youtu.be/pgoZFKgoiDk)

## Esquema del Proyecto

Vamos a crear un proyecto de Next.js que luego vamos a subirlo a un repositorio de GitHub. A continuación vamos a clonarlo en nuestro servidor de producción que será el encargado de estar corriendo siempre nuestra aplicación. 



## Despliegue Manual

> En el caso del curso un utilizaremos un VPS (virtual private server) del cloud provider Digital Ocean, pero aplica lo mismo para nuestor propio servidor.

<img src="Next.js CICD con GA.assets/image-20221020093535015.png" alt="image-20221020093535015" style="zoom:50%;" />



Queremos que este flujo se ejecute de manera automática con GitHub Actions para así no perder tiempo haciendo el despligue manualmente cada vez que hagamos modificaciones al código.



## Despliegue Automático

La automatización mediante GitHub Actions la logramos creando un workflow que consiste en un archivo de configuración con la extensión `yml` en el cual le daremos indicaciones de los pasos que queremos que realice en el servidor. En el servidor estaremos pendientes de los cambios del código (un nuevo push por ejemplo) mediante un un programa que nos proporciona GitHub llamado runner.

<img src="Next.js CICD con GA.assets/image-20221020093616654.png" alt="image-20221020093616654" style="zoom:50%;" />



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