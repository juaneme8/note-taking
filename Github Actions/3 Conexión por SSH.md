# Conexión por SSH

El propósito es conectarnos a un servidor mediante SSH y ejecutar en el algún comando desde un simple echo, copiar los archivos obtenidos luego del build o descargar una imagen de Docker.

> :information_source: Basado en https://www.youtube.com/watch?v=gW1TDirJ5E4&ab_channel=omgneering

## Acción a utilizar

Utilizaremos [esta acción](https://github.com/marketplace/actions/ssh-remote-commands), en la propia documentación de la acción nos encontramos con un snippet de ejemplo.

> Con la última versión disponible al momento de escribir estas notas v0.1.15, obteníamos múltiples líneas con mensajes de salida (err:, out:). Para solucionarlo debemos cambiar lo siguiente respecto a lo indicado en la documentación: `uses: appleboy/ssh-action@dce9d565de8d876c11d93fa4fe677c0285a66d78` como se sugiere en el [issue](https://github.com/appleboy/ssh-action/issues/174)

```yaml
name: remote ssh command
on: [push]
jobs:

  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
    - name: executing remote ssh commands using password
      uses: appleboy/ssh-action@dce9d565de8d876c11d93fa4fe677c0285a66d78
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



# Workflow con Password

touch first_workflow.yml

```
---
name: remote ssh command
on:
  push:
    branches:
      - main
jobs:
  job_one:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - name: executing remote ssh commands using password
        uses: appleboy/ssh-action@dce9d565de8d876c11d93fa4fe677c0285a66d78
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          password: ${{ secrets.PASSWORD }}
          script: whoami
```



# Workflow con SSH Key

Vamos a seguir los pasos establecidos en la [documentación de la acción](https://github.com/marketplace/actions/ssh-remote-commands).

## Creación SSH Key

```
ssh-keygen -t ed25519 -a 200 -C "juaneme8@gmail.com"
```

> `-a 200`  es el número de rondas KDF (key derivation function o [función de derivación clave](https://es.wikipedia.org/wiki/Función_de_derivación_de_clave)). Esto aumenta la resistencia al descifrado de contraseñas en caso de que se tenga acceso a la clave privada. Por defecto son 16

> `-C`  es un comentario



Luego nos preguntará mostrará el siguiente mensaje: **"Enter file in which to save the key (C:\Users\juan.ocho/.ssh/id_ed25519)"**

En ese momento ingresamos:

```
C:\Users\juan.ocho/.ssh/id_ed25519_testing
```

Agregar la llave generada en Authorized Keys

```
cat .ssh/id_ed25519.pub | ssh b@B "cat >> .ssh/authorized_keys"
```

Ejemplo trabajando desde Windows:

```
cat C:\Users\juan.ocho\.ssh\id_ed25519_testing.pub | ssh sgf@testing-external "cat >> .ssh/authorized_keys"
```

Copiar la llave privada en Github Secrets

```
clip < ~/.ssh/id_ed25519
```

```
clip < C:\Users\juan.ocho\.ssh\id_ed25519_sgf_testing
```



Actualizar el workflow para que utilice la clave indicada:

```
---
name: remote ssh command
on:
  push:
    branches:
      - main
jobs:
  job_one:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - name: executing remote ssh commands using ssh key
        uses: appleboy/ssh-action@dce9d565de8d876c11d93fa4fe677c0285a66d78
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.KEY }}
          script: whoami
```

