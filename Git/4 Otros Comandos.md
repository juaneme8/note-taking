# `git rm`
El comando `git rm` nos permite sacar de lo que será commiteado a un archivo (esto se conoce como *unstage*).
De esta manera podremos eliminar archivos de Git sin eliminar su historial del sistema de versiones.

Recuerda que `git rm` puede usarse acompañado de los flags para indicarle a Git cómo eliminar los archivos que ya no necesitamos en la última versión del proyecto:

-   `git rm --cached`: Elimina los archivos del área de staging y del próximo commit pero los mantiene en nuestro disco duro. Si sólo queremos eliminar un archivo `git rm --cached index.html`. 

-   `git rm --force`: Elimina los archivos de Git y del disco duro.

Suponiendo que tenemos un archivo `biografia.txt` y hacemos `git add biografia.txt` y luego:
```bash
git rm --cached biografia.txt
```
Luego de esto el archivo seguirá estando disponible y si hacemos `git status` nos aparecerá como *untracked*.

# `git show`
El comando `git show` nos permite obtener los cambios introducidos por el último commit y quién los ha efectuado.

# `git help`
El comando `git help` nos permite obtener ayuda sobre cualquiera de los comandos, por ejemplo si ejecutamos `git help show` nos abrirá en el navegador un documento de ayuda sobre el comando `git show`, `git help commit`, etc.

# `git log`
El comando `git log` nos permite obtener la lista de los commits del repositorio. En la parte superior veremos los últimos commits y en la inferior los primeros.

Veremos el id completo del commit que es un identificador **SHA-1** que nos permite identificar univocamente los commits dentro de un repositorio.

> Veremos que el último commit realizado dice `(HEAD -> main)` por lo que `HEAD` apunta a ese commit.
> Si queremos obtener sólo la historia de un archivo podemos hacer `git log biografia.txt`
> Cuando la lista de commits es más larga que lo que entra en la pantalla podemos navegarla con las flechas hacia arriba y hacia abajo. Presionando la letra `q` podemos salir de dicha navegación.

Con `git log --all` para mostrar todos los commits en todos los branches, tags y otras refs.
Con  `git log -S "cabecera"` puedo mostrar los commits donde aparece la palabra cabecera (ya sea en el mensaje o como nombre de rama).

## Opciones `git log`
Con el comando `git help log` podremos ver todas las opciones que podemos utilizar junto con este comando:
Por ejemplo `git log --oneline --graph --decorate --all`

-   `--oneline`: queremos la información condensada en una única línea (id reducido y mensaje)
-   `--graph`: para obtener un gráfico basado en asteríscos denotando la jerarquía de las ramas.
-   `--decorate`: para saber de qué rama son los *commits*
-   `--all`: para obtener el historial de todos las ramas

-   `--stat`: veo los cambios `+` y `-` que se hicieron en cada archivo con cada commit.
-   `--author="Juan"` me aparecerán todos los commits realizados por un usuario que coincida con la cadena ingresada. No hace falta poner el nombre exacto.

# `git diff`
Con `git diff` obtenemos las diferencias entre el contenido que tenemos en staging y lo que tenemos en el **directorio de trabajo** o sino tenemos nada en el staging, entre lo que tenemos en el repositorio (en la posición `HEAD`) y lo que tenemos en el **directorio de trabajo**.

Resumiendo, con `git diff` obtenemos las diferencias respecto del **directorio de trabajo**,en cambio si queremos obtener las diferencias entre el contenido de staging y el repositorio debemos ejecutar con `git diff --staged`

Con `git diff f91d408 HEAD` comparamos un commit con el commit de la posición `HEAD`.
Con `git diff f91d408 HEAD hello.txt` realizamos la comparación relativa a un archivo en particular.
Con `git diff f91d408 ab7a420 ` vemos los cambios entre ambos commits.
Con `git diff feature main` utilizamos el nombre de las ramas para comparar en lugar de *commit IDs*.

> Con `git log` obtenemos los ids de cada commit.
> Debemos poner primero el id de un commit viejo y luego de uno nuevo, de lo contrario los agregados que hayamos hecho (`+`) los tomará como eliminaciones (`-`).

# `git restore`
El comando `git restore` nos permite quitar del staging area un archivo dejándolo como cambios en el working directory. Por ejemplo si tenemos un archivo `bio.txt`, al cual le generamos cambios y después hicimos `git add .` podremos quitarlo del staging con `git restore --staged bio.txt`. Si en ese momento ejecutamos `git diff bio.txt` veremos las diferencias entre working directory y repositorio local.

# `git ls-files`
El comando `git ls-files` nos permite obtener los archivos que están siendo trackeados por Git.

> Si creamos un nuevo archivo con `touch newfile.txt` y luego ejecutamos `git ls-files` veremos que dicho archivo como es de esperarse no aparece dentro de los *tracked*. Para eliminar ese archivo de prueba `rm newfile.txt`

# `git checkout`
El comando `git checkout` nos permite traer al *working directory* los cambios que tenemos en un determinado commit.
Si obtener el archivo `historia.txt` que teníamos en el commit cuyo id es `6ab4581...` debemos ingresar `git checkout 6ab4581... historia.txt`. En ese momento si hacemos `git status` nos aparecerá como que lo hemos modificado y si hiciéramos commit volveríamos al valor que tenía anteriormente. Si en cambio una vez que lo vimos queremos volver a `main` hacemos `git checkout main historia.txt`

> Cuando hacemos `git checkout` veremos un mensaje *dettached HEAD* lo cual nos da la pauta que estamos viendo un commit viejo.

También `git checkout` es útil si a su vez queremos revertir cambios y llevar al archivo a su último estado presente en el repositorio Git, debemos ingresar `git checkout -- README.md`

# `git reset`
## Volver a una versión anterior

El comando `git reset` nos permite volver a una versión anterior, para ello debemos poner el id del commit al cual queremos volver. Existen dos tipos de reset el `--hard` (todo vuelve al estado anterior y los commits subsiguientes desaparecen) y `--soft` (el contenido del directorio de trabajo vuelve a la versión anterior pero lo que tengamos en el área de staging se conserva).
La forma de ejecutar estos comandos sería `git reset ab97... --soft` o `git reset ab97... --hard`

## Quitar de cambios staging area

En ocasiones puede que hayamos agregado al *staging area* ciertos cambios que nos dimos cuenta luego que no queremos.
Supongamos que modificamos el archivo `README.md` luego hicimos `git add .` y luego nos damos cuenta que no nos interesan esos cambios. Para quitarlo del *staging area* (*unstage changes*) para eso debemos poner `git reset HEAD README.md`

# Eliminar Repositorio
### `rm -rf .git`

Con el comando `rm -rf .git` eliminamos el repositorio.

```bash
rm -rf .git
```

Utilizamos los flags `-rf` (`r`: recursiva, `f`: forzada)

# Renombrar archivos
Esto podemos hacerlo de dos formas y cambiará el modo en que lo percibe Git.

## `git mv`
Con el comando `git mv` podremos cambiarle el nombre a un archivo que hayamos commiteado. Suponiendo que tenemos un archivo `archivo.txt` y queremos que pase a llamarse `nuevo.txt`, debemos hacer lo siguiente: `git mv archivo.txt nuevo.txt`.
Si a continuación hacemos `git status` veremos que *the change has been staged*.

```bash
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        renamed:    archivo.txt -> nuevo.txt
```

Para completar el renombramiento debemos hacer el commit.

## Renombrar con bash Command
Si le cambiamos el nombre al archivo `archivo.txt` y le ponemos `nuevo.txt` utilizando el *bash command* `mv archivo.txt nuevo.txt`, nos aparecerá `archivo.txt` como *deleted* y `nuevo.txt` como *untracked*. Si hacemos `git add .` nos aparecerá `renamed:    archivo.txt -> nuevo.txt`

# Borrar archivos
Al igual que a la hora de renombrar archivos, para borrarlos también podemos hacerlo de dos formas: con `git rm` o con un comando de bash.

## `git rm`

Con el comando `git rm` podremos eliminar un archivo (tanto del repositorio como de la copia local). Si en cambio lo borramos con el sistema operativo con `rm` no estaríamos trackeando esa eliminación.

Suponiendo que queremos eliminar `demo.txt` ejecutamos `git rm demo.txt` y al hacer `git status` veremos que la eliminación ya está en la zona de staging y debemos hacer el commit para completarla.

```bash
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        deleted:    demo.txt
```

-   `git rm --cached`: Elimina los archivos del área de Staging y del próximo commit pero los mantiene en nuestro disco duro. Por ejemplo si queremos eliminar del staging al archivo `bio3.txt` ejecutamos `git rm --cached bio3.txt`

-   `git rm --force` o `git rm -f`: Elimina los archivos de Git y del disco duro.

## Eliminar con comando de Bash
Si lo eliminamos usando el sistema operativo con el *bash command*`rm demo.txt`, nos aparecería como *deleted*. Luego con `git add -u` añadimos el archivo eliminado al staging area y luego realizamos el commit.

# Special Markers
Podemos pensar en los **markers** como si fueran punteros, uno especial es el llamado `HEAD` normalmente es el último commit del branch actual. Al cambiar de branch `HEAD` cambiará de ubicación de modo tal de coincidir con el último commit de dicho branch. Sin embargo es posible mover manualmente `HEAD` para que apunte a otro lado. Por ejemplo si en el `main` branch tenemos varios commits y nos movemos hacia uno con `git checkout` `HEAD` apuntará a dicho commit.

# Ramas
Los **commits** son guardados en una línea de tiempo conocida como **branch**. Un **repositorio** tendrá por lo menos una rama que por default recibe el nombre de `main`.

> Desde el 1 de octubre de 2020 GitHub cambió el nombre de la rama principal: ya no es `master` sino `main`. Este derivado de una profunda reflexión ocasionada por el movimiento #BlackLivesMatter. La industria de la tecnología lleva muchos años usando términos como master, slave, blacklist o whitelist y de a poco se intenta quitarlos.

Hasta el momento hemos trabajado en el branch default `main`. En ocasiones nos convendrá trabajar con otra rama que es un entorno aislado (línea temporal alternativa que posibilita la división lógicas del trabajo) sin comprometer la rama principal.  Por ejemplo el uso de ramas será útil si queremos solucionar un bug, desarrollar un nuevo feature, experimentar con una librería, etc y luego volverlas a unir a `main` mediante lo que se conoce como merge si así lo quisiéramos.

> Entre paréntesis podremos ver el branch dentro del cual nos encontramos. Otra forma es con `git status` ya que dirá por ejemplo "On branch main"


## `git branch`
Con `git branch features` creamos una nueva rama llamada `features` . Si lo hacemos desde `main` veremos con `git show` que `HEAD`, `main` y `features` están asociados al mismo commit.
Con `git branch` obtenemos los branches con los que contamos y veremos un asterisco al lado del branch actual.
Con `git branch -vv` obtenemos información extra.
Con `git branch -a` podemos listar todas las ramas (locales y remotas). El que aparece con un `*` es en el cual estoy trabajando actualmente.
Con `git branch -r` podemos listar las ramas remotas.

Con `git branch -d features` eliminamos localmente el branch `features` localmente si ya fue mergeado. De lo contrario nos aparecerá el error **The branch 'features' is not fully merged.**
Con `git branch -D features` eliminamos localmente el branch `features` haya sido mergeado o no.
Con `git push origin --delete features` eliminamos el branch `features` en el repositorio remoto.

Con `git fetch --all` obtenemos todas las las ramas remotas.

Con `git checkout features` es posible cambiar al branch `features`.
Con `git checkout -b features2` creamos el branch `features2` y saltamos a él a la vez.

Con `git show-branch` mostramos las ramas locales y sus cambios.
Con `git show-branch -all` mostramos las ramas locales y remotas y sus cambios.

Con `gitk` abrimos un entorno gráfico en el cual podemos ver la historia (debemos estar en Git Bash para poder hacerlo).

Con `git push origin features` enviamos la rama `features` al repositorio remoto. Sin embargo, puede que no queramos enviar la rama en la que hemos trabajando localmente.

> Es interesante tener en cuenta que si realizamos modificaciones en el *main branch* (las cuales podremos ver lógicamente con `git status`) y nos damos cuenta que esos cambios deberían estar aislados, podremos crear un nuevo branch y esas modificaciones serán llevadas a ese nuevo branch.

> Usando el comando `git checkout main` para switchear al main branch. Luego de hacer esto podremos ver que `(HEAD, main)` apuntan al mismo commit ID. Cuando hacemos `git checkout features` veremos que el apuntador `HEAD` se mueve al último commit de dicha rama.

## `main` branch
Cuando creamos un repositorio se crea el `main` branch (antiguamente llamado `master`). Este representa la versión estable de nuestro código (normalmente nos referiremos a el como *stable*,*released*, *production* o *published*) es por eso que a la hora de implementar nuevas funcionalidades no queremos hacerlo en esta rama sino en otra.
Los branches nos permiten trabajar con múltiples funcionalidades (probablemente de la mano de distintos programadores) a la vez y luego ir mergeandolos al `main` en la medida que los terminamos.

# Merge
Se conoce como merge al proceso de unir o fusionar los cambios de una rama con otra rama. Por ejemplo si tenemos la `main` y detectamos un bug, creamos un branch `hotfix` luego efectuamos en esa rama los commits que sean necesarios y finalmente cuando estamos satisfechos con el resultado mergeamos esos cambios en `main` donde tenemos el código de producción.

## `git merge`
Con el comando `git merge hotfix` realizamos el merge del branch `hotfix` en el branch en el cual nos encontremos actualmente.

> Antes debemos hacer `git checkout main` para switchear al branch en el cual queremos unir con `hotfix`. Pero tener presente que el merge puede realizarse en ambos sentidos, no necesariamente siempre hacia el `main`.

Git intentará hacer este merge de manera automática cuando esto sea posible, por lo que vamos a lidiar con distintos tipos de escenarios:

### Merge con Fast-Forward
Es el caso más simple y se da cuando no se detecta trabajo adicional en el *parent branch*, git simplemente aplicará los commits de `hotfix` al `main` branch, por lo que será como si el branch secundario nunca hubiera existido. En ese momento con `git log` que `(HEAD-> main, hotfix)` apuntan al mismo commit id. En este caso veremos que no estamos efectuando un nuevo commit producto del merge.

### Merge con Estrategia Recursiva
Cuando Git detecta cambios no conflictivos en el *parent branch* se preservan los dos *timelines* y se crea un *merge commit* automático para mostrar el merge de los dos branches. Esto sucederá por ejemplo cuando en el nuevo branch modificamos un archivo y en el `main` modificamos otro (o líneas distintas) y luego hacemos el merge. Nos creará un nuevo commit y con Vim nos indicará que ingresemos el mensaje de dicho commit.

### Merge Manual
Ocurre cuando Git no es capaz de solucionar los conflictos automáticamente , una vez resueltos son guardados en un *merge commit*. Esto sucede por ejemplo si creamos un branch modificamos una línea, realizamos el commit y luego en el branch `main` también modificamos y commiteamos cambios en la misma línea ==o en el mismo selector por ejemplo en caso de CSS==.
Cuando hacemos `git merge hotfix` desde `main`, nos aparecerá:

```bash
CONFLICT (content): Merge conflict in bio5.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Automáticamente nos abre el editor por defecto y podremos aceptar los cambios entrantes, los de la rama actual, ambos, etc. Una vez hecho esto debemos hacer `git add .` y `git commit` para concluir el merge. Como consecuencia de este merge nos aparecerán también los commits que tenía la rama con la cual mergeamos.

> En el editor veremos `<<<<<<<< HEAD` (posición donde nos encontramos en la rama actual) y `>>>>>>>> hotfix` rama con la que estamos fusionando. De hecho si abrimos el archivo con el block de notas veremos que se han agregado esas marcas en las zonas conflictivas. Visual Studio Code me permite solucionarlas de manera simple haciendo click en sus links pero también podría borrarlas manualmente y quedarme con el cambio que quiera.

Luego si hacemos `git log` veremos:-

```
commit 7b919d5b1b779f92fdcb1bf7a55e93946306d6c2 (HEAD -> main)
Merge: 3adde80 a544554
Author: Juan Ocho <juaneme8@gmail.com>
Date:   Sun Jun 27 13:08:02 2021 -0300

    mensaje del commit
```

Lo cual nos da la pauta que ese commit fue producto de un merge entre los commits 3adde80 y a544554

> Notar que no estamos viendo el hash completo y esto es así porque cuando el proyecto no es muy grande podremos utilizar solo una parte del id y seguirá siendo único.


# Alias
Es posible crear alias a nivel global como así también alias utilizando Git.

## Alias Globales
Cuando tenemos que escribir un comando muy largo, como por ejemplo `git log --oneline --graph --decorate --all` recurrimos al uso de un alias `alias arbolito="git log --oneline --graph --decorate --all"`. De esta manera bastará con ingresar `arbolito` para ejecutar el comando anterior.

> Tener en cuenta que es un recurso del mundo de Linux por lo que debemos estar con Git Bash.

## Configuración de Alias
Otra forma de hacerlo es con la configuración de Git:
```bash
git config --global alias.hist "log --oneline --graph --decorate --all"
```

Luego podremos utilizarlo con `git hist` y si queremos obtener la misma información relativa a un archivo en particular podremos ingresar `git hist -- LICENSE.md`

# Etiquetas
En los proyectos de código abierto es habitual encontrarnos con etiquetas que hacen referencia a la versión del código. Por lo general son útiles cuando estamos en GitHub, ya que así como tenemos el apartado de ramas tenemos uno de tags donde veremos las distintas etiquetas creadas y al hacer click navegaremos a esos cambios.

Es posible etiquetar puntos específicos en los cambios de código que representan ciertos releases, esta funcionalidad se usa típicamente para marcar versiones de lanzamiento (v1.0, por ejemplo).

El modo de crear una etiqueta consiste en primer lugar en copiar el hash del commit al cual queremos referenciar (podremos copiar sólo la versión reducida de dicho hash) `git tag -a v0.1 -m "Versión preliminar" 1b56158`

Con `git tag` listamos los tags disponibles
Con `git show-ref --tags` vemos el hash asociado a cada tag (no al commit).
Con `git push origin --tags` enviamos los tags al repositorio remoto (como buena práctica recordar antes hacer un pull).

Con `git tag -d dormido` podemos para eliminar el tag deseado. Luego tendremos que ejecutar también `git push origin --tags`. Si ejecutamos `git tag` veremos que ya no existe, sin embargo, en GitHub lo seguimos viendo ya que los tags suelen ser usados como *releases* (forma en la que se categoriza que algo está listo). Es por eso que para eliminarlo de GitHub debemos ejecutar `git push origin :refs/tags/dormido`

# Revertir Cambios
Existen distintas técnicas para revertir cambios que hayamos realizado en nuestro historial, estas serán más o menos seguras.

## 1 `git checkout`
Con el comando `git checkout` podremos realizar técnica menos peligrosa para revertir cambios. Nos permite ver el código en un cierto punto (*detach from master branch*) cuando hicimos el commit. Es read-only por lo que no podré arruinar el historial de cambios. Una vez que terminamos de veer y queremos volver al punto actual lo hacemos con `git checkout main` (*reatach to master branch*)

### `git checkout -- .`
El comando `git checkout -- .` nos permite revertir los cambios que tengamos en el *working directory* (es decir aquellos que no están en *staging area*).

> Si queremos chequear qué sucederá al hacer esto antes de ejecutarlo podemos ejecutar `git checkout --`
> Si queremos revertir los cambios de un archivo en particular podemos ejecutar `git checkout -- index.html`

## 2 `git revert`
Con el comando `git revert` podremos deshacer un commit en particular, es como si lo elimináramos pero no lo estamos borrando en realidad sino que añadimos un nuevo commit que revierte aquello que pasaba posteriormente. 

> Luego de ingresar este comnando tendremos que colocar un mensaje del nuevo commit.

## 3 `git reset`
Con el comando `git reset` eliminamos de manera permanente los commits que le sucedieron.

El comando `git reset` nos ayuda a volver en el tiempo. Pero no como `git checkout` que nos deja ir, mirar, pasear y volver. Con `git reset` volvemofs al pasado sin la posibilidad de volver al futuro. Borramos la historia y la debemos sobreescribir. 

Este comando es muy peligroso y debemos usarlo solo en caso de emergencia. Recuerda que debemos usar alguna de estas dos opciones:

- `git reset af6b84`: Viajamos hasta ese commit y lo que hicimos a nivel cambios sigue disponible, por lo que podríamos ponerlo en el staging area y hacer un commit. Este comando es útil para unir todo los commits y ponerlos en uno sólo. El working directory retiene los cambios.
- `git reset --soft`: Borramos todo el historial y los registros de Git pero guardamos los cambios que tengamos en Staging, así podemos aplicar las últimas actualizaciones a un nuevo commit.
- `git reset --hard`: Borra toda la información de los commits y del área de staging se borra del historial. Los commits posteriores serán eliminados de manera permanente, es por eso que debemos usarlo con cuidado.

-   `git reset` o `git reset HEAD` ambos comandos sirven para sacar archivos del área de *staging*. Tendremos los archivos en el directorio de trabajo y si queremeos volver a agregarlos a *staging* podremos hacerlo con `git add`. 
Con `git reset file.txt` o `git reset HEAD file.txt` sólo quitamos el archivo `file.txt` del staging dejando el resto en caso de tener más de uno.

- `git reset --hard` no sólo quita todo del staging sino que también elimina los archivos modificados (los trackeados).

## `git reset` vs `git rm`
`git reset` y `git rm` son comandos con utilidades muy diferentes, pero aún así se confunden muy fácilmente.

> `git rm —cached` file will **remove** the file from the stage. That is, when you commit the file will be removed. `git reset HEAD — file` will simply reset file in the staging area to the state where it was on the HEAD commit, i.e. will undo any changes you did to it since last commiting. If that change happens to be newly adding the file, then they will be equivalent.

# `git rebase`
> https://www.youtube.com/watch?v=f1wnYdLEpgI

Cuando desarrollamos una nueva característica hasta ahora hemos visto la técnica de crear un branch `feature` realizar los commits que hagan falta en esa rama (y también habrán sucedido otros commits en `main` por otros miembros del equipo) y luego realizar el merge que creará un nuevo commit con estrategia recursiva.

Sin embargo, en ocasiones puede que queramos que los commits de `feature` se integren a la historia de la rama `main` como si hubieran ocurrido en ella (y la rama nunca hubiera existido). En ese caso debemos utilizar **rebase** que es un comando destructivo que va a alterar el historial, ya que Git está eliminando los commits en el `feature` branch y duplicándolos los commits en la rama `main`. Siempre debemos hacerlo con cosas que no hayan sido pusheadas aún.

> La regla de oro respecto al uso de **rebase** es nunca hacerlo en ramas públicas.
> **rebase** no funciona muy bien en proyectos open-source y pull requests.
> El uso de **rebase** tiene la ventaja por sobre el uso de **merge**, que si hacemos `git log` o con la herramienta gráfica `gitk` veremos todos los commits en línea por lo que sera fácil de seguir el historial aún si tenemos varias personas en el equipo de trabajo.
> El uso de **merge** en cambio tiene la ventaja de que tendremos trazabilidad pues sabremos de donde provienen los commits (de un merge de tal rama con `main`)

En primer lugar en la rama `feature` ingresamos el comando `git rebase master` y lo que sucederá es que Git actualizará el contenido de mi rama auxiliar respecto de los nuevos cambios que sucedieron en `main` y me notificará en caso de conflictos. Es como si esa rama `feature` la hubiera creado cuando se realizaron los últimos cambios en `main` y no antes. Con `git log` veremos los commits del branch `feature` como si los hubiésemos hecho todos juntos a lo último. Es por eso que decimos que cambia la historia respecto de cuándo arrancó el branch.

Por último desde `main` (donde van a quedar los cambios) ingresamos `git rebase feature` y ya tendremos todos los commits en línea y podremos pushear el código.

Finalmente podremos borrar la rama feature con `git branch -D feature` (usamos `-D` suponiendo que ya lo pusheamos sino localmente bastará con `-d`)

## Aplastar Commits con rebase
rebase también nos permite aplastar commits en uno sólo de modo de limpiar un poco el historial.

# `git stash`
Con el comando `git stash` nos permite guardar los cambios que tengo en el working directory en un lugar temporal. 

Esto puede ser útil si tenemos en dos escenarios:
* Cuando tengo una serie de cambios en mi directorio de trabajo en la rama `main` y quiero moverme a una rama `footer` para revisar algo y después volver. En ese caso con `git status` vería mis cambios, luego ejecuto `git stash` y ahí con `git status` no veré ya esos cambios. A continuación con  `git checkout footer` cambio de rama sin problemas, reviso lo que tengo que revisar, vuelvo a la rama principal con `git checkout main` y luego `git stash pop` para recuperar esos cambios.

* Cuando tengo cambios en el directorio de trabajo y queremos crear una nueva rama con esos cambios. Con `git stash branch feature` creamos un branch `feature` con el contenido del stash y nos movemos a ella. Con `git status` me aparecerán los archivos que tenía modificados anteriormente.

* Cuando tengo cambios en el directorio de trabajo y quiero deshacerme de ellos ejecuto `git stash` para volver al último commit y `git stash drop` para eliminar el stash.

Con `git stash` guardo el WIP (*work in progress*) en stash.
Con `git stash list` podré ver las cosas que tengo en stash (si no devuelve nada como respuesta es que no tengo nada).
Con `git stash pop` retiro los cambios que tengo en stash.
Con `git stash branch feature` creamos un branch con los cambios del stash y nos movemos a esa rama.
Con `git stash drop` elimino el stash.

# `git clean`
El comando `git clean` nos permite eliminar los archivos no trackeados del directorio de trabajo. Esto es útil cuando tenemos archivos no deseados `.log`, archivos resultado de una compilación, etc. 
Supongamos que por error duplicamos varios archivos, en vez de eliminarlos manualmente podremos ejecutar `git clean -f` para borrar todos los *untracked files*. 
Con `git clean -- dry-run` podemos ver una previsualización de los archivos que eliminaremos si ejecutamos `git clean -f`

> Si tenemos carpetas duplicadas tendremos que borrarlas a mano ya que git se concentra en los archivos.
> `git clean`  sólo borra las cosas que puede indexar. Esto significa que si tengo `.gitignore` de `*.jpg`, lógicamente no podré borrar ese tipo de imágenes duplicadas.

# `git cherry-pick`
El comando `git cherry-pick` se utiliza cuando hemos estado trabajando en una rama y queremos en `main` (o en otra rama) sólo uno de esos avances. 

Debemos tener presente que el uso de cherry-pick es considerado una mala práctica pues estamos reconstruyendo la historia. Es aconsejable utilizar en cambio merge.

Suponemos que estuvimos trabajando en la rama `features` y en ella realizamos tres commits `c1`, `c2` y `c3` y queremos traernos a `main` sólo los cambios introducidos en `c1`. Desde la rama `main` debo ejecutar `git cherry-pick id-c1`. Si a continuación ejecutamos `git log --oneline` veremos ese commit como si hubiera sido realizado en la rama `main`. 

> Si luego decido realizar merge de `features` en `main` puede que nos aparezcan conflictos. De hecho veremos el mensaje de un commit dos veces (tendrá distinto id).

# `git reflog`
El comando `git reflog` tiene su nombre basado en *reference logs* y nos permite conocer cuando cambio el apuntador de los branches. Por ejemplo `HEAD@{2}` significa "donde solía estar `HEAD` hace dos movimientos".

Por ejemplo ante una situación en la que hemos cometido un error y no sabemos bien qué fue lo que sucedió ya que `git log` no nos ayuda, podemos ejecutar `git reflog` y buscar un commit previo a la aparición de este problema. Luego podremos poner por ejemplo `git reset --soft HEAD@{4}` (mantiene lo que sea que tengamos en staging, en staging) y `git reset --hard HEAD@{4}`.  

El `git reset --hard` debemos usarlo solo en situaciones donde algo se rompió.

Debemos tener presente que al pushear se verá como si no hubiera pasado nada.

# `git grep`
El comando `git grep` me permite buscar palabras en los archivos que tengo en el repositorio o en staging.

Con `git grep color` obtengo todas las apariciones de la palabra color indicando en qué archivo.
Con `git grep -n color` obtengo todas las apariciones de la palabra color indicando en qué archivo y en qué línea.
Con `git grep -c color` puedo contar la cantidad de veces que aparece palabra color deglosado por archivo .
Con `git grep -c "<p>"` puedo contar la cantidad de veces que usé la etiqueta `<p>` deglosado por archivo.

De manera similar si quiero filtrar de acuerdo a la información brindada por `git log` puedo usar `git log -S "cabecera"` para mostrar los commits relacionados con la palabra cabecera (ya sea que aparece esta palabra en el mensaje o que la rama tiene ese nombre).

# `git shortlog`
El comando `git shortlog` entrega un resumen de lo mostrado por `git log`. Nos permite ver cuántos commits ha realizado cada miembro del equipo y los mensajes que estos han tenido.
Con `git shortlog -sn` vemos la cantidad de commits realizados por cada usuario.
Con `git shortlog -sn --all` contabilizamos incluso los commits borrados.
Con `git shortlog -sn --all --no-merges` ídem anterior pero ocultando los merges.  

# `git blame`
El comando `git blame` me permite ver línea por línea quién fue el que la modificó, por ejemplo `git blame blogpost.html`
Con `git blame -c blogpost.html` mejoramos la tabulación de la salida.
con `git blame blogpost.html -L35,53 -c` podremos ver quién modificó de la línea 35 a la línea 53.# GIT
> Basado en Udemy - GitHub Ultimate Master Git and GitHub - Beginner to Expert
> https://www.youtube.com/watch?v=kEPF-MWGq1w&ab_channel=PeladoNerd

# Características
* Es un **sistema de control de versiones descentralizado** o distribuido (posibilitando el **manejo offline** y **escalabilidad**).
* Creado por Linus Torvalds.
* Es un sistema **open source** y **free**. 
* Cuenta con una **comunidad muy grande** y es utilizado por muchos desarrolladores.

# Sistema Distribuido:
Git se presenta como un **sistema de control de versiones distribuido**, en el que todos los nodos manejan la totalidad de la información y por lo tanto pueden actuar de cliente o servidor en cualquier momento. Esto se lo logra gracias a que cada vez que sincronizas los cambios con el repositorio remoto Git, te guarda una copia entera de los datos con toda la estructura y los archivos necesarios. Así ya **no es necesario conectarse a internet** para consultar los cambios históricos sobre un archivo o para ver quién fue la última persona que lo editó, todo se hace directamente sobre tu **copia local** y luego, cuando lo consideres oportuno, puedes enviar esos cambios hacia el **repositorio remoto**.

En los **sistemas de control de versiones centralizados** como SVN en cambio,  el código y las distintas versiones están en el servidor, por lo que cada desarrollador debía para guardar todos los cambios y actualizaciones están fuertemente atados a la conexión a Internet para funcionar, de otra manera los cambios nunca podrán reflejarse en el **servidor central**.

# CLI vs GUI
Utilizaremos línea de comandos o **CLI** (*command line interface*) en lugar de una **GUI** (*graphical user interface*) por varios motivos. 
* Debido a que es el modo en que fue diseñado git originalmente, por lo que los **new features** aparecen disponibles primero para línea de comandos y luego para los entornos gráficos. 
* La **ayuda online** que suele ser brinda usando CLI como método standard (ya que la mayoría de los clientes gráficos tienen los mismos comandos).
* La línea de comandos nos aporta **más potencia** ya que los clientes gráficos suelen cubrir sólo los comandos más comunes.   
* Nos permite tener un set de **comandos multiplataforma** (a excepción de algún que otro comando específicos).
* Una vez que hayamos aprendido a usar la línea de comandos podremos realizar un **pasaje simple** a un cliente gráfico.

# Repositorio
Se llama repositorio a la **colección de archivos y su historial** que se encuentran bajo el sistema de control de versiones. No debemos confundir con el **working directory** o **workspace** que es la carpeta que contiene los archivos (y carpetas) con los que estamos trabajando y esta puede tener tanto archivos manejados por git, como archivos que no lo son todavía o bien archivos que nunca lo serán. Dentro del *workspace* contamos con una carpeta cuasi oculta llamada `.git` la cual contiene el **repositorio**. 

Es recomendable tener **un repositorio para cada parte de la aplicación** por ejemplo uno para el *front end* y otro para el *back end*.

# Commits
Git almacena el **estado** de todos los archivos que maneja y lo hace a través de **commits**, que son *snapshots*. 

Debemos tener presente que **git versiona archivos, no carpetas**. Es por esto que si una cierta tecnología nos obliga a tener carpetas vacías, podemos colocarle dentro *dummy files* vacíos a modo de preservarla. 

# Master Branch
Los **commits** son guardados en una línea de tiempo conocida como **branch**. Un **repositorio** tendrá por lo menos una rama que por default recibe el nombre de **master branch**

# GitHub
GitHub es un servicio de hosting para repositorios que cuenta además con otros servicios como ser hosting para páginas web, *issue tracking*, mecanismos para compartir *code snippets*, etc.

# Instalación Git
Descargar el instalador en [https://git-scm.com/](https://git-scm.com/) y ejecutar dejando los valores default. 

A continuación en Git Bash o Cmder: 

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

Si queremos obtener los valores cargados `git config user.name` y `git config user.mail`

# Configuración Editor
Muchos comandos de Git lanzarán un editor de texto para solicitar más información. Si queremos procesar esa solicitud con Visual Studio Code: `git config --global core.editor "code --wait"`

Si queremos conocer el editor que tenemos configurado `git config core.editor`

Ingresando `git config --global -e` abirá el archivo de configuración `.gitconfig` donde veremos:

```bash
[core]
	editor = code --wait
[user]
	name = Juan Ocho
	email = juaneme8@gmail.com

(...)
```

# Instalación P4Merge
Es una herramienta gráfica para comparar y resolver conflictos, podemos descargarla en [https://www.perforce.com/downloads/visual-merge-tool](https://www.perforce.com/downloads/visual-merge-tool) recordando presionar **Skip Registration**

Al instalar elegir sólo Visual Merge Tool y no las restantes herramientas.

# Integración P4Merge con Git

## p4Merge as Diff Tool (Comparing)
```bash
git config --global diff.tool p4merge
git config --global difftool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"
git config --global difftool.prompt false
```
## p4Merge as Merge Tool
```bash
git config --global merge.tool p4merge 
git config --global mergetool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"
git config --global mergetool.prompt false
```

Podemos chequear con `git config --global -e` que hayamos cargado todo bien.

# Local Git States
En el **repositorio git local** existen tres estados o áreas en los que pueden estar los archivos manejados por git: *working directory*, *staging area* y *git repository*.

|                   |              |                |
| :---------------: | :----------: | :------------: |
| WORKING DIRECTORY | STAGING AREA | GIT REPOSITORY |

El **working directory** contiene todos los archivos y carpetas de la aplicación los cuales pueden estar o no manejados por git (de cualquier modo git estará al tanto de esos archivos). En el otro extremo tenemos el **repositorio** que contiene todos los cambios que han sido guardados o *committed*. En el medio tenemos el **staging area** que se utiliza para preparar el siguiente commit.

# `git init`
## Iniciar Repositorio Nuevo
En la carpeta `projects` queremos inicializar un repositorio git dentro de la carpeta o *working directory* llamada `udemyCourse`
```bash
mkdir projects
cd projects
git init udemyCourse
```

Luego si hacemos `cd udemyCourse\` veremos `(master -> origin)` lo cual nos da la pauta de que tenemos un repositorio git y que estamos en el master branch.

Con `ls -al` veremos el repositorio `.git`

## Iniciar con Proyecto Existente
Si queremos agregar el sistema de control de versiones a un proyecto existente, simplemente navegamos hasta la carpeta del proyecto:
```bash
cd projects/
cd demo/
git init .
```
Con el `.` indicamos que queremos que la carpeta actual esté bajo el sistema de control de versiones. Nuevamente veremos `(master -> origin)` y podremos comprobar la existencia del repositorio  `.git`

Para listar los archivos utilizamos  `ls -al`:
* `-a` (*all*) para ver todos los archivos y carpetas incluyendo los ocultos y los que empiezan con `.`
* `-l` (*long*) para listar los archivos en formato largo

# `git help`
Con el comando `git help` podremos obtener todas las opciones disponibles para un determinado comando por ejemplo `git help log`

# `git status`
El comando `git status` nos permite obtener el estado del repositorio.
```bash
git status
```

```bash
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

# Crear Archivo

```bash
code README.md
```
De este modo creamos el archivo en **Visual Studio Code** `README.md` y luego lo guardamos.

```bash
git status

On branch master
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md
nothing added to commit but untracked files present (use "git add" to track)
```

# `git add`
Con el comando `git add` agregamos un archivo o varios al *staging area*.
```bash
git add README.md
git status

On branch master
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
```

En este caso movimos sólo un archivo, si quisieramos mover la totalidad de los *untracked files* podríamos haber utilizado el *wildcard* `.`
```bash
git add .
```

# `git commit -m`
Con `git commit -m` ingresamos un mensaje de commit en línea.
```bash
git commit -m "Primer commit en el repo demo"
git status

On branch master
nothing to commit, working tree clean
```
Con `git commit` en cambio, nos abre el editor por default para ingresar allí el mensaje que podrá ser multilínea y una vez concluido guardamos dicho archivo `CTRL+S` y cerramos la ventana `CTRL+W`

# `rm -rf .git`
Con el comando `rm -rf .git` eliminamos el repositorio.
```bash
rm -rf .git
```
Utilizamos los flags `-rf`:
`r`: recursiva
`f`: forzada.

```bash
 git status
fatal: not a git repository (or any of the parent directories): .git
```

# `git log`
`git log` nos permite obtener una lista de los commits del repositorio
```bash
git log

commit f91d408707edb473a5337fd81b3d53800ca5be80 (HEAD -> master)
Author: Juan Ocho <juaneme8@gmail.com>
Date:   Tue Aug 18 00:46:27 2020 -0300
    Commit ingresado manualmente
```
Veremos el *commit id* es un identificador **SHA-1** que nos permite identificar univocamente los commits dentro de un repositorio.

## Opciones
Con el comando `git help log` podremos ver todas las opciones que podemos utilizar junto con este comando:
Por ejemplo `git log --oneline --graph --decorate --all`
* `--oneline`: queremos la información en una única línea
* `--graph`:  para obtener un gráfico basado en asterísticos denotando la jerarquía de las ramas.
* `--decorate`: para saber de qué rama son los *commits*
* `--all`: para obtener el historial de todos las ramas

# Git Alias
 Al ingresar ``git log --oneline --graph --decorate --all`` podemos pensar en la vista obtenida como un **historial git** si bien no existe un comando con ese nombre, vamos a crear un **alias**. 
Este alias no será otra cosa que una representación corta de un comando existente más largo.

```bash
git config --global alias.hist "log --oneline --graph --decorate --all"
```
Con `--global` ya que queremos que sea **a nivel usuario** y no a nivel repositorio.
Con `alias.hist` establecemos el nombre del alias que en este caso será `hist`
Notar que a la hora de generar el alias cuando escribimos el comando no incluimos la palabra `git`

Para verificar lo ingresado `git config --global --list`

 Luego podremos utilizarlo con `git hist` y si queremos obtener la misma información relativa a un archivo en particular podremos ingresar `git hist -- LICENSE.md`

# `git show`
Con `git show` obtenemos el último commit y un *diff* (entre el antes y el después) con los cambios.
```bash
git show
```

Si modificamos un archivo que se encuentra siendo manejado por git, veremos que aparece como **modified**.

# `git ls-files`
El comando `git ls files` nos permite obtener los archivos que están siendo trackeados por git.

> Si creamos un nuevo archivo con `touch newfile.txt` y luego ejecutamos `git ls-files` veremos que dicho archivo como es de esperarse no aparece dentro de los *tracked*. Para eliminar ese archivo de prueba `rm newfile.txt`

# `git commit -a`
El comando `git commit -a` se conoce como **Express Commit** ya que toma los *tracked files* (que obtuvimos anteriormente con `git ls-files`) agrega aquellos con cambios al *staging area* y procede a realizar el commit. 

Como queremos agregar también un mensaje en línea agregamos el parámetro `-m` por lo que nos queda `git commit -am "mensaje asociado al commit"`

# `git reset`
En ocasiones puede que hayamos agregado al *staging area* ciertos cambios que nos dimos cuenta luego que no queremos. 

Supongamos que modificamos el archivo `README.md` luego hicimos `git add .` y luego nos damos cuenta que no nos interesan esos cambios. Para quitarlo del *staging area* (*unstage changes*) para eso debemos poner `git reset HEAD README.md`

# `git checkout`
El comando `git checkout` es útil si a su vez queremos revertir esos cambios y llevar al archivo a su último estado conocido presente en el repositorio git, debemos ingresar `git checkout -- README.md`

# `git mv`
Cone el comando `git mv` podremos cambiarle el nombre a un archivo que hayamos commiteado. Suponiendo que tenemos un archivo `example.txt` y queremos que pase a llamarse `demo.txt`, debemos hacer lo siguiente: `git mv example.txt demo.txt`.
Si a continuación hacemos `git status` veremos que *the change has been staged*.
```bash
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        renamed:    example.txt -> demo.txt
```
Para completar el renombramiento debemos hacer el commit.

### Renombrar con bash Command
Si le cambiamos el nombre utilizando  el *bash command* `mv example.txt demo.txt`, nos aparecerá `example.txt` como deleted y `demo.txt` como untracked. El archivo eliminado podríamos agregarlo al staging area con `git add -u` mientras que `demo.txt` seguiría como untracked. Para incluir tanto agregados como eliminaciones podemos usar `git add -A`

# `git rm`
Con el comando `git rm` podremos eliminar un archivo con la ventaja respecto de eliminarlo usando el sistema operativo de que git trackeará dicha eliminación.
Suponiendo que queremos eliminar `demo.txt` ejecutamos `git rm demo.txt` y al hacer `git status` veremos que *the deletion has been staged*:

```bash
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        deleted:    demo.txt
```
Para completar la eliminación debemos hacer el *commit*.

### Eliminar con bash command
Si lo eliminamos usando el sistema operativo con el *bash command*`rm demo.txt`, nos aparecería como deleted. Luego con `git add -u` añadimos el archivo eliminado al staging area y luego realizamos el commit. 

# `.gitignore`
Supongamos que tenemos un archivo `app.log` que no queremos que forme parte del repositorio, esto podemos hacerlo con el archivo `.gitignore`, para crearlo `code .gitignore` y dentro podemos colocar tanto `app.log` o directamente `*.log` para omitir todos los archivos que terminen con `.log`

# `git diff`
Si ejecutamos `git log --oneline --graph --decorate --all` o el alias creado anteriormente `git hist`, veremos el id de cada commit y podremos comparar dos de ellos por ejemplo: `git diff f91d408 HEAD` Si queremos comparar lo mismo relativo a un archivo en particular podemos poner: `git diff f91d408 HEAD hello.txt`

Si ahora realizamos un cambio en el *working directory*, podremos ejecutar `git diff` y obtener la comparación entre esos cambios y la posición `HEAD` en el repositorio.

Por último a este comando también podemos pasarle el nombre de *branches* en lugar de *commit IDs* `git diff feature master`

# `git difftool`
De manera similar podemos hacer `git difftool f91d408 HEAD` y lanzar P4Merge o bien directamente ejecutar `git difftool` para comparar los cambios hechos en el *working directory* con el contenido del mismos archivo en la posición `HEAD` del repositorio.

# Branching & Merging
Podemos pensar en un **branch** como un *timeline* de commits o más específicamente es el nombres que le damos a esta línea de tiempo en git ya que podremos crear y eliminar branches sin afectar a los *timelines*.

Hasta el momento hemos trabajado en el *default master branch*, podemos crear un **branch** que realice una determinada tarea (*feature branch*) y luego reunirse al *master branch* haciendo un **merge**. Git intentará hacerlo automáticamente cuando esto sea posible, por lo que vamos a lidiar con distintos tipos de escenarios:
* **fast-forward merge**: es el caso más simple y se da cuando no se detecta trabajo adicional en el *parent branch*, git simplemente aplicará los commits del *feature branch* al *parent branch*, por lo que será como si el branch nunca hubiera existido. En caso de que no lo queramos se puede deshabilitar el fast-forward merge, pero la mayoría de las veces será el comportamiento deseado.
* **automatic merge**: cuando git detecta cambios no conflictivos en el *parent branch* se preservan los dos *timelines* y se crea un *merge commit* automático para mostrar el merge de los dos branches.
* **manual merge**: ocurre cuando git no es capaz de solucionar los conflictos automáticamente , una vez resueltos  son guardados en un *merge commit*. 

# Special Markers
Podemos pensar en los **markers** como si fueran punteros, uno especial es el llamado HEAD normalmente es el último commit del branch actual. Al cambiar de branch HEAD cambiará de ubicación de modo tal de coincidir con el último commit de dicho branch. Sin embargo es posible mover manualmente `HEAD` para que apunte a otro lado. Por ejemplo si en el master branch tenemos varios commits y nos movemos hacia uno con `git checkout` `HEAD` apuntará a dicho commit.

# `git branch`
El comando `git branch` nos permite conocer los branches con los que contamos y veremos un asterisco al lado del branch actual. Si queremos obtener información extra `git branch -vv`
Por otra parte para crear un nuevo branch podremos hacerlo con  `branch features`


# `git branch -a`
Con el comando `git branch -a` podemos listar todas las ramas (locales y remotas).

# `git branch -r`
Con el comando `git branch -r` podemos listar las ramas remotas

# `git checkout`
El comando `git checkout features` nos permite switchear al branch desado.

# `git checkout -b`
Con el comando `git checkout -b feature` podremos crear el branch `feature` y switchear a él a la vez.

**NOTA**: Es interesante tener en cuenta que si realizamos modificaciones en el *master branch* (las cuales podremos ver lógicamente con `git status`) y nos damos cuenta que esos cambios deberían estar aislados, podremos crear un nuevo branch y esas modificaciones serán llevadas a ese nuevo branch.

# `git checkout`
Usamos el comando `git checkout master` para switchear al master  branch. Luego de hacer esto podremos ver que `(HEAD, master)` apuntan al mismo commit ID.

# `git merge`
Con el comando `git merge feature` realizamos el merge del branch `feature` en el branch actual.

### Caso 1
En caso de que se realice un *fast-forward merge* se aplicarán los commits del *feature branch* en el *master branch* en ese momento veremos si usamos el alias `git hist` que `(HEAD, updates, master)` apuntan al mismo commit id.

### Caso 2  
En caso de que haya conflictos que no puedan ser solucionados por el merge automático (por ejemplo si creamos un branch modificamos una línea, realizamos el commit y luego en el master branch también modificamos y commiteamos cambios en la misma línea original. En ese momento veremos que el cursor pasó a decir `(master -> origin)` en letra roja. Con `cat README.md` lo cual entrega como output el contenido del archivo podremos visualizar el conflicto.

Si bien en los casos más simples podríamos resolverlo manualmente con `git mergetool` veremos de manera gráfica con P4Merge las soluciones posibles. Debemos elegir una opción, luego guardar y salir.

En ese momento con `git status` veremos
```bash
git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)
```
Por lo que hacemos el commit para concluir el merge.

> Luego de hacer esto P4Merge nos genera un archivo `README.md.orig` que es un backup del archivo original con las marcas de conflicto. Podemos eliminarlo (`git rm README.md.orig)` antes de hacer el commit o bien agregarlo a `.gitignore` (poniendo `*.orig`). Si queremos evitar que suceda la próxima vez podemos hacerlo con `git config --global mergetool.keepBackup false`

# `git branch -d`
Con el comando `git branch -d features` eliminamos el branch `features`

# Git Tags 
Es posible etiquetar puntos específicos del historial como importantes, esta funcionalidad se usa típicamente para marcar versiones de lanzamiento (v1.0, por ejemplo).


**lightweight tags**: `git tag --list`
**annotated tags**: `git tag `
 Iniciar Repositorio
En la carpeta `projects` queremos inicializar un repositorio git dentro de la carpeta o *working directory* llamada `udemyCourse`
```bash
mkdir projects
cd projects
git init udemyCourse
```

Luego si hacemos `cd udemyCourse\` veremos `(master -> origin)` lo cual nos da la pauta de que tenemos un repositorio git y que estamos en el master branch.

Con `ls -al` veremos el repositorio `.git`


# Obtener branches remotos
git fetch --all