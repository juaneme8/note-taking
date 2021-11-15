# `git init`
## Iniciar Repositorio y crear directorio
El comando `git init` nos permite inicializar un nuevo repositorio. Supongamos que en la carpeta `CursoGit` queremos inicializar un repositorio Git dentro de la carpeta o _working directory_ llamada `Proyecto1`.

```bash
mkdir CursoGit
cd CursoGit
git init Proyecto1
```

Luego veremos `(main)` lo cual nos da la pauta de que estamos en el main branch.

>  Si queremos que al crear el repositorio la rama princiapl tenga otro nombre podemos hacerlo con:
>
> ```
> git init Proyecto1 --initial-branch=main
> ```
    > Otra opción es cambiar `init.defaultBranch` con cuyo valor podemos obtener con `git config --list | grep branch`

> Con `ls -al` veremos el repositorio `.git`

## Iniciar Repositorio con directorio existente
Si queremos agregar el sistema de control de versiones a un proyecto existente, simplemente navegamos hasta la carpeta del proyecto, supongamos `Proyecto2`

```bash
cd CursoGit/
cd Proyecto2/
git init
```

# `git add`

El comando `git add` nos permite incorporar uno o mas archivos al _staging area_ o área de preparación.

> Si queremos agregar todos los archivos con cambios de la carpeta en la cual estamos en lugar de `git add biografia.txt` podríamos ejecutar `git add .`
> Cuando modificamos un archivo lo veremos en rojo y luego de haberlo añadido al *staging area* lo veremos en verde.

# `git commit`

El comando `git commit` nos permite agregar el contenido del staging area al repositorio local, consiste en agregar un punto en la línea de tiempo al cual podremos volver luego. Este comando agrega todo el contenido del staging area al repositorio.

> En este caso estamos poniendo `commit -m "Mensaje asociado"` pero podríamos poner directamente `git commit` y nos abrirá el editor de código por defecto donde podremos ponerlo, luego guardar y cerrar el archivo.
> Si no establecimos ningún editor con `git config --global core.editor "code --wait" ` abriremos el archivo en Vim. En ese caso presionando `i` pasaremos al modo insertar y luego de ingresar el mensaje deseado presionamos `ESC` `SHIFT+Z` `SHIFT+Z` para guardar y cerrar Vim.
> Si tenemos establecido el Visual Studio Code como editor nos abrirá ese editor para ingresar allí el mensaje que podrá ser multilínea y una vez concluido guardamos dicho archivo `CTRL+S` y cerramos la ventana `CTRL+W`
> Podemos pensar al commit más reciente como la cabecera y lo veremos como `HEAD`, es un apuntador a la rama en la cual nos encontramos trabajando.

> Si queremos condensar los comandos de add y commit en uno sólo podemos poner `git add . && git commit -m "Mensaje"`

## `git commit -a`

El comando `git commit -a` se conoce como **Express Commit** ya que toma los *tracked files* (que obtuvimos anteriormente con `git ls-files`) agrega aquellos con cambios al *staging area* y procede a realizar el commit.

Como queremos agregar también un mensaje en línea agregamos el parámetro `-m` por lo que nos queda `git commit -am "mensaje asociado al commit"`

## `git commit --amend`
Cuando realizo un commit y luego me doy cuenta que me hubiera gustado agregarle más cambios a ese commit, puedo realizar dichos cambios localmente y luego agregar ese archivo al staging area `git add file.txt` y por último ejecutar `git commit --amend`, luego me preguntará si quiero cambiar el mensaje del commit. De esta manera los cambios realizados los pegará en el commit anterior sin realizar uno nuevo.



## `git restore`

En caso de modificar un archivo que ya fue commiteado podemos restaurar la versión previa de ese fichero con:

```
git restore index.html
```

* Si queremos restaurar todo el directorio

```
git restore .
```

* Si queremos restaurar todos los archivos terminados en `*.js`

```
git restore '*.js'
```

Si bien la forma recomendada actualmente es con `git restore`, históricamente esto mismo era realizado utilizando `git checkout` pero como es un comando que sirve también para otras cosas por lo que puede resultar confuso.

* Si queremos restaurar un archivo `index.html`

```
git checkout -- index.html
```

* Si queremos restaurar todo el directorio

```
git checkout .
```

* Si queremos restaurar todos los archivos terminados en `*.js`

```
git checkout -- '*.js'
```



## `git clean`

Si hemos realizado una serie de cambios en un archivo que todavía no fue commiteado y queremos deshacer, podemos eliminarlo manualmente o bien usar el comando 

* Si queremos ejecutar un *dry-run* indicándonos qué archivos borraría pero sin hacerlo

```bash
git clean -n
```

* Si queremos efectuar el borrado de esos archivos

```bash
git clean -f
```

* Si queremos borrar también los directorios que no existían antes

```bash
git clean -d
```

* Si queremos que nos pregunte antes de borrar cada archivo

```bash
git clean -i
```



# `git status`

El comando `git status` compara el directorio de trabajo con el repositorio buscando archivos que hayamos agregado (_untracked files_) o cambios en archivos que ya tiene el repositorio (_changes not staged_).

La salida que nos devuelta `git status` cambiará de acuerdo a la situación en la cual estemos. Suponiendo que solo tenemos un archivo `biografia.txt`:
Si no lo agregamos lo veremos como **Untracked file**.
Si agregamos al archivo (pero no hacemos un commit) veremos: **Changes to be committed** (new file)
Si ahora lo modificamos veremos: **Changes to be committed** (new file) y **Changes not staged for commit** (modified)
Si ahora hacemos el commit veremos: **nothing to commit, working tree clean**
Si ahora hacemos un cambio veremos nuevamente: **Changes not staged for commit:**

# Resumiendo
Suponiendo que tenemos un archivo `biografia.txt` lo primero que hacemos es inicializar el repositorio local, agregar el archivo al staging area y guardar los cambios haciendo un commit.
`git init`
`git add biografia.txt`
`git commit -m "versión 1"`.

Supongamos que luego efectúo cambios en `biografia.txt` para guardarlos en el repositorio, primero tenemos que agregarlos y luego hacer el commit:
`git add biografia.txt`
`git commit -m "versión 2"`.

> Una vez que hicimos `git init` si ejecutamos `ls -al` (`a` para ver los archivos ocultos y `l` para verlos en forma de lista) veremos que nos ha agregado un directorio oculto `.git`.

# Conceptos Básicos
## Creación Archivos
Con el comando `touch biografia.txt` podemos crear un archivo vacío.

> Otra forma de hacerlo es con `code biografia.txt` que tiene la ventaja que además nos abrirá el editor.