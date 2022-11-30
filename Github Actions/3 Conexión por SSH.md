# Conexión por SSH

El propósito es conectarnos a un servidor mediante SSH y ejecutar en el algún comando desde un simple echo, copiar los archivos obtenidos luego del build o descargar una imagen de Docker.

> :information_source: Basado en https://www.youtube.com/watch?v=gW1TDirJ5E4&ab_channel=omgneering

## Acción a utilizar

Utilizaremos [esta acción](https://github.com/marketplace/actions/ssh-remote-commands), en la propia documentación de la acción nos encontramos con un snippet de ejemplo:

```yaml
name: remote ssh command
on: [push]
jobs:

  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
    - name: executing remote ssh commands using password
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.HOST }}
        username: ${{ secrets.USERNAME }}
        password: ${{ secrets.PASSWORD }}
        port: ${{ secrets.PORT }}
        script: whoami
```

## Creación Repositorio

Vamos a crear un repositorio de ejemplo llamado `ga-ssh-example` ya que queremos que esto se ejecute cuando recibamos un push.

## Creación Secretos

Una vez dentro del repositorio vamos a Settings :arrow_right: Secrets :arrow_right: Actions y allí cargamos las variables de entorno:

```
HOST
PASSWORD
PORT
USERNAME
```



## Creación del Workflow

Luego hacemos click en la pestaña **Actions** y elegimos **Skip this and set up a workflow yourself**

```
mkdir -p .github/workflows
```

touch first_workflow.yml

```
name: remote ssh command
on:
  push:
    branches: [main]
jobs:
  job_one:
    name: Build
    runs-on: ubuntu-latest
    steps:
    - name: executing remote ssh commands using password
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.HOST }}
        username: ${{ secrets.USERNAME }}
        password: ${{ secrets.PASSWORD }}
        port: ${{ secrets.PORT }}
        script: whoami
```

