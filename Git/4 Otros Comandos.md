# `git show`
El comando `git show` nos permite obtener los cambios introducidos por el último commit y quién los ha efectuado.

# `git help`
El comando `git help` nos permite obtener ayuda sobre cualquiera de los comandos, por ejemplo si ejecutamos `git help show` nos abrirá en el navegador un documento de ayuda sobre el comando `git show`, `git help commit`, etc.

> Ante la confusión acerca de si es `git help show` o `git show help` pensar que **el comando** es `git help`
>
> 

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
-   `--decorate`: muestra los ref names de todos los commits que se muestren.
-   `--all`: para obtener el historial de todos las ramas
-   `--stat`: veo la cantidad de cambios `+` y `-` que se hicieron en cada archivo con cada commit.
-   `--patch` similar al anterior pero para mostrar además qué información fue agregada o quitada en cada archivo modificado en cada commit.
-   `--author="Juan"` me aparecerán todos los commits realizados por un usuario que coincida con la cadena ingresada. No hace falta poner el nombre exacto.
-   

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
El comando `git restore` nos permite quitar del staging area un archivo dejándolo como cambios en el working directory. Por ejemplo si tenemos un archivo `bio.txt`, al cual le generamos cambios y después hicimos `git add .` podremos quitarlo del staging con `git restore --staged bio.txt`. 

Esto es equivalente a ejecutar `git reset HEAD bio.txt`

> Si en ese momento ejecutamos `git diff bio.txt` veremos las diferencias entre working directory y repositorio local.



> Similitudes entre `git restore --cached`, `git rm --cached` y `git reset` basado en [comentario StackOverflow](https://stackoverflow.com/a/65434709/11384318)
>
> - a commit holds a snapshot of all files that Git knew about, as of the form they had when you said to commit them;
> - the snapshot is made *from* the files that are in Git's index, aka staging-area, aka cache (three terms for the same thing); and
> - `git add` means *make the copy in the index/staging-area/cache match the copy in my working tree* (by copying from the working tree if the working tree copy is updated, or by removing from the index if the working tree copy is removed).
>
> So the index / staging-area contains, at all times, your *proposed next commit*, and was initially seeded from your *current commit* when you did a `git checkout` or `git switch` to obtain that commit.1 Your working tree thus contains a *third* copy2 of each file, with the first two copies being the one in the *current commit* aka `HEAD`, and the one in the index.
>
> * ``git rm --cached *file*`: removes the copy of the file from the index / staging-area, without touching the working tree copy. The proposed next commit now *lacks* the file. If the current commit *has* the file, and you do in fact make a next commit at this point, the difference between the previous commit and the new commit is that the file is gone.
>
> * `git restore --staged *file*`: Git copies the file from the `HEAD` commit into the index, without touching the working tree copy. The index copy and the `HEAD` copy now match, whether or not they matched before. A new commit made now will have the *same* copy of the file as the current commit. 
>   If the current commit *lacks* the file, this has the effect of *removing* the file from the index. So *in this case* it does the same thing as `git rm --cached`. Porque no tendré que hacer un commit más para eliminarlo.
>
> - `git reset *file*`: this copies the `HEAD` version of the file to the index, just like `git restore --staged *file*`.



Resumiendo la diferencia es que `git rm --cached` lo eliminará al archivo del repositorio en el próximo commit y `git restore --staged` solo lo quita del staging.



# `git ls-files`
El comando `git ls-files` nos permite obtener los archivos que están siendo trackeados por Git.

> Si creamos un nuevo archivo con `touch newfile.txt` y luego ejecutamos `git ls-files` veremos que dicho archivo como es de esperarse no aparece dentro de los *tracked*. Para eliminar ese archivo de prueba `rm newfile.txt`

# `git checkout`
El comando `git checkout` nos permite traer al *working directory* los cambios que tenemos en un determinado commit.
Si queremos obtener el archivo `historia.txt` que teníamos en el commit cuyo id es `6ab4581...` debemos ingresar `git checkout 6ab4581... historia.txt`. En ese momento si hacemos `git status` nos aparecerá como que lo hemos modificado y si hiciéramos commit volveríamos al valor que tenía anteriormente. Si en cambio una vez que lo vimos queremos volver a `main` hacemos `git checkout main historia.txt`

> Cuando hacemos `git checkout` veremos un mensaje *dettached HEAD* lo cual nos da la pauta que estamos viendo un commit viejo. Cuando terminemos de ver lo que queremos con `git checkout master` podríamos volver al estado inicial.

También `git checkout` es útil si a su vez queremos revertir cambios y llevar al archivo a su último estado presente en el repositorio Git, debemos ingresar `git checkout -- README.md`

# `git reset`
## Volver a una versión anterior

El comando `git reset` nos permite volver a una versión anterior, para ello debemos poner el id del commit al cual queremos volver. Existen dos tipos de reset el `--hard` (todo vuelve al estado anterior y los commits subsiguientes desaparecen) y `--soft` (el contenido del directorio de trabajo vuelve a la versión anterior pero lo que tengamos en el área de staging se conserva).
La forma de ejecutar estos comandos sería `git reset ab97... --soft` o `git reset ab97... --hard`

## Quitar de cambios staging area

En ocasiones puede que hayamos agregado al *staging area* ciertos cambios que nos dimos cuenta luego que no queremos.
Supongamos que modificamos varios archivos y entre ellos `README.md` luego hicimos `git add .` y luego nos damos cuenta que no queremos que los cambios en la documentación estén en el mismo commit que un feature. Para quitar ese archivo del *staging area* (*unstage changes*) debemos ejecutar `git reset HEAD README.md`. Siendo `HEAD` un apuntador a la versión más reciente del repositorio. Sin embargo, tener presente que los cambios realizados no se habrán ido sino que sólo lo quitamos del staging area.

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
Si a continuación hacemos `git status` veremos que estos cambios fueron agregados al staging area.

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

Con el comando `git rm` podremos eliminar un archivo (tanto del repositorio como de la copia local). Si en cambio lo borramos con el sistema operativo con `rm` no estaríamos trackeando esa eliminación, por lo que tendríamos que hacer `git add deleted.txt` y luego con `git status` ya veriamos "deleted".

Suponiendo que queremos eliminar `demo.txt` ejecutamos `git rm demo.txt` y al hacer `git status` veremos que la eliminación ya está en la zona de staging y debemos hacer el commit para completarla.

```bash
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        deleted:    demo.txt
```

-   `git rm --cached`: Elimina los archivos del área de staging pero los mantiene en nuestro disco duro. Si sólo queremos eliminar un archivo `git rm --cached index.html`.  Es decir que vamos a conservar los cambios realizados localmente.
-   `git rm --force` o `git rm -f`: Elimina los archivos de Git y del disco duro.




Suponiendo que creamos un archivo `biografia.txt` y hacemos `git add biografia.txt` y luego:

```bash
git rm --cached biografia.txt
```

Luego de esto el archivo seguirá estando disponible y si hacemos `git status` nos aparecerá como *untracked*. 



## Eliminar con comando de Bash
Si lo eliminamos usando el sistema operativo con el *bash command*`rm demo.txt`, nos aparecería como *deleted*. Luego con `git add -u` añadimos el archivo eliminado al staging area y luego realizamos el commit.

# Special Markers
Podemos pensar en los **markers** como si fueran punteros, uno especial es el llamado `HEAD` normalmente es el último commit del branch actual. Al cambiar de branch `HEAD` cambiará de ubicación de modo tal de coincidir con el último commit de dicho branch. Sin embargo es posible mover manualmente `HEAD` para que apunte a otro lado. Por ejemplo si en el `main` branch tenemos varios commits y nos movemos hacia uno con `git checkout` `HEAD` apuntará a dicho commit.



# Ramas

## `main` branch

Cuando creamos un repositorio se crea el `main` branch (antiguamente llamado `master`). Este representa la versión estable de nuestro código (normalmente nos referiremos a el como *stable*,*released*, *production* o *published*) es por eso que a la hora de implementar nuevas funcionalidades no queremos hacerlo en esta rama sino en otra.
Los branches nos permiten trabajar con múltiples funcionalidades (probablemente de la mano de distintos programadores) a la vez y luego ir mergeandolos al `main` en la medida que los terminamos.



Los **commits** son guardados en una línea de tiempo conocida como **branch**. Un **repositorio** tendrá por lo menos una rama que por default recibe el nombre de `main`.

> Desde el 1 de octubre de 2020 GitHub cambió el nombre de la rama principal: ya no es `master` sino `main`. Este derivado de una profunda reflexión ocasionada por el movimiento #BlackLivesMatter. La industria de la tecnología lleva muchos años usando términos como master, slave, blacklist o whitelist y de a poco se intenta quitarlos.

Hasta el momento hemos trabajado en el branch default `main`. En ocasiones nos convendrá trabajar con otra rama que es un entorno aislado (línea temporal alternativa que posibilita la **división lógica** del trabajo) sin comprometer la rama principal.  Por ejemplo el uso de ramas será útil si queremos solucionar un bug, desarrollar un nuevo feature, experimentar con una librería, etc y luego volverlas a unir a `main` mediante lo que se conoce como merge si así lo quisiéramos.

> Entre paréntesis podremos ver el branch dentro del cual nos encontramos. Otra forma es con `git status` ya que dirá por ejemplo "On branch main"


## `git branch`
Con `git branch features` creamos una nueva rama llamada `features` . Si lo hacemos desde `main` veremos con `git show` o con `git log` que `HEAD`, `main` y `features` están asociados al mismo commit.
Con `git branch` obtenemos los branches con los que contamos y veremos un asterisco al lado del branch actual.
Con `git branch -vv` obtenemos información extra.
Con `git branch -a` podemos listar todas las ramas (locales y remotas). 
Con `git branch -r` podemos listar las ramas remotas.

Con `git branch -d features` eliminamos localmente el branch `features` localmente si ya fue mergeado. De lo contrario nos aparecerá el error **The branch 'features' is not fully merged.**
Con `git branch -D features` eliminamos localmente el branch `features` haya sido mergeado o no.
Con `git push origin --delete features` eliminamos el branch `features` en el repositorio remoto.

Con `git fetch --all` obtenemos todas las las ramas remotas.

Con `git checkout features` cambiamos al branch `features`.
Con `git checkout -b features2` creamos el branch `features2` y saltamos a él a la vez.

Con `git show-branch` mostramos las ramas locales y sus cambios.
Con `git show-branch -all` mostramos las ramas locales y remotas y sus cambios.

Con `gitk` abrimos un entorno gráfico en el cual podemos ver la historia (debemos estar en Git Bash para poder hacerlo).

Con `git push origin features` enviamos la rama `features` al repositorio remoto. Sin embargo, puede que no queramos enviar la rama en la que hemos trabajando localmente.

> Es interesante tener en cuenta que si realizamos modificaciones en el *main branch* (las cuales podremos ver lógicamente con `git status`) y nos damos cuenta que esos cambios deberían estar aislados, podremos crear un nuevo branch y esas modificaciones serán llevadas a ese nuevo branch.

> Usando el comando `git checkout main` para switchear al main branch. Luego de hacer esto podremos ver que `(HEAD, main)` apuntan al mismo commit ID. Cuando hacemos `git checkout features` veremos que el apuntador `HEAD` se mueve al último commit de dicha rama.



## Naming

Además de `main` tendremos una rama `dev` que es código testeado candidato para le próximo **realease**, está listo para producción.

Para el desarrollo de nuevas funcionalidades o solución de bugs lo habitual es tener nombres como `feature/new-table` o `bugfix/new-table`. Es aconsejable tener features pequeños que no lleven mucho tiempo de desarrollo con lo cual será potencialmente menos conflictivo a la hora de realizar el merge.

Cuando se trabaja por sprints luego del tiempo establecido el contenido de los branchs de feature y bugfix son mergeados a `dev` y al finalizar el sprint serán mergeados a `main`.

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
Ocurre cuando Git no es capaz de solucionar los conflictos automáticamente , una vez resueltos son guardados en un *merge commit*. Esto sucede por ejemplo si creamos un branch modificamos una línea, realizamos el commit y luego en el branch `main` también modificamos y commiteamos cambios en la misma línea.
Cuando hacemos `git merge hotfix` desde `main`, nos aparecerá:

```bash
CONFLICT (content): Merge conflict in bio5.txt
Automatic merge failed; fix conflicts and then commit the result.
```



> En caso de querer abortar el merge podemos hacer `git merge --abort` y será como si no hubiéramos ejecutado `git merge`



Automáticamente nos abre el editor por defecto y podremos aceptar los cambios entrantes, los de la rama actual, ambos, etc. Una vez hecho esto debemos hacer `git add .` y `git commit` para concluir el merge. Como consecuencia de este merge nos aparecerán también los commits que tenía la rama con la cual mergeamos.

> En el editor veremos `<<<<<<<< HEAD` (**posición donde nos encontramos en la rama actual**) y `>>>>>>>> hotfix` rama con la que estamos fusionando. De hecho si abrimos el archivo con el block de notas veremos que se han agregado esas marcas en las zonas conflictivas. Visual Studio Code me permite solucionarlas de manera simple haciendo click en sus links pero también podría borrarlas manualmente y quedarme con el cambio que quiera. Cuando hablamos de **current change** nos referimos a la rama en la cual estamos. Ante la duda también podemos hacer click en **Compare Changes** y ver ambas versiones una al lado de la otra. Una vez resuelto el conflicto tendremos que hacer el `git add .` y `git commit`.

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
Cuando tenemos que escribir un comando muy largo, como por ejemplo `git log --oneline --graph --decorate --all` recurrimos a un recurso de Linux para alias y si estamos trabajando con Git bash a pesar de estar en Windows podremos usarlo:  `alias arbolito="git log --oneline --graph --decorate --all"`. De esta manera bastará con ingresar `arbolito` para ejecutar el comando anterior.



## Configuración de Alias
Otra forma de hacerlo es mediante la configuración de Git:
```bash
git config --global alias.hist "log --oneline --graph --decorate --all"
```

Luego podremos utilizarlo con `git hist` y si queremos obtener la misma información relativa a un archivo en particular podremos ingresar `git hist -- LICENSE.md`

# Etiquetas
En los proyectos de código abierto es habitual encontrarnos con etiquetas que hacen referencia a la versión del código. Por lo general son útiles cuando estamos en GitHub, ya que así como tenemos el apartado de **ramas** tenemos uno de **tags** donde veremos las distintas etiquetas creadas y al hacer click navegaremos a esos cambios.

Es posible etiquetar puntos específicos en los cambios de código que representan ciertos releases, esta funcionalidad se usa típicamente para marcar versiones de lanzamiento (v1.0, por ejemplo). 

El modo de crear una etiqueta consiste en primer lugar en copiar el hash del commit al cual queremos referenciar (podremos copiar sólo la versión reducida de dicho hash) `git tag -a v0.1 -m "Versión preliminar" 1b56158`. De este modo estaremos agregando una referencia en `refs/tags/`.

Con `git tag` listamos los tags disponibles
Con `git show-ref --tags` vemos el hash asociado a cada tag (no al commit).
Con `git push origin --tags` enviamos los tags al repositorio remoto (como buena práctica recordar antes hacer `git fetch --all --tags`).

Con `git tag -d dormido` podemos para eliminar el tag deseado. Luego tendremos que ejecutar también `git push origin --tags`. Si ejecutamos `git tag` veremos que ya no existe, sin embargo, en GitHub lo seguimos viendo ya que los tags suelen ser usados como *releases* (forma en la que se categoriza que algo está listo). Es por eso que para eliminarlo de GitHub debemos ejecutar `git push origin :refs/tags/dormido`

# Revertir Cambios
Existen distintas técnicas para revertir cambios que hayamos realizado en nuestro historial, estas serán más o menos seguras.

## 1 `git checkout`
Con el comando `git checkout` podremos realizar técnica menos peligrosa para revertir cambios. Nos permite ver el código en un cierto punto (*detach from main branch*) cuando hicimos el commit. Es read-only por lo que no podré arruinar el historial de cambios. Una vez que terminamos de ver y queremos volver al punto actual lo hacemos con `git checkout main` (*reatach to main branch*).

### `git checkout -- .`
El comando `git checkout -- .` nos permite revertir los cambios que tengamos en el *working directory* (es decir aquellos que no están en *staging area*).

> Si queremos chequear qué sucederá al hacer esto antes de ejecutarlo podemos ejecutar `git checkout --`
> Si queremos revertir los cambios de un archivo en particular podemos ejecutar `git checkout -- index.html`

## 2 `git revert`
Con el comando `git revert` podremos deshacer un commit en particular, es como si lo elimináramos pero no lo estamos borrando en realidad sino que añadimos un nuevo commit que revierte aquello que en él sucedía. 

> Luego de ingresar este comnando tendremos que colocar un mensaje del nuevo commit.

## 3 `git reset`
Con el comando `git reset` eliminamos de manera permanente los commits que le sucedieron.

El comando `git reset` nos ayuda a volver en el tiempo. Pero no como `git checkout` que nos deja ir, mirar, pasear y volver. Con `git reset` volvemos al pasado sin la posibilidad de volver al futuro. Borramos la historia y la debemos sobreescribir. 

Este comando es muy peligroso y debemos usarlo solo en caso de emergencia. Recuerda que debemos usar alguna de estas dos opciones:

- `git reset af6b84`: Viajamos hasta ese commit y lo que los cambios de los commits que estamos rebobinando siguen disponibles en el working directory, por lo que podríamos ponerlo en el staging area y hacer un commit. Este comando es útil para unir todo los commits y ponerlos en uno sólo. El working directory retiene los cambios.
- `git reset --soft`: Borramos todo el historial y los registros de Git pero guardamos los cambios que tengamos en Staging, así podemos aplicar las últimas actualizaciones a un nuevo commit.
- `git reset --hard`: Borra toda la información de los commits y del área de staging se borra del historial. Los commits posteriores serán eliminados de manera permanente, es por eso que debemos usarlo con cuidado.
-   `git reset` o `git reset HEAD` ambos comandos sirven para sacar archivos del área de *staging*. Tendremos los archivos en el directorio de trabajo y si queremeos volver a agregarlos a *staging* podremos hacerlo con `git add`. 
Con `git reset file.txt` o `git reset HEAD file.txt` sólo quitamos el archivo `file.txt` del staging dejando el resto en caso de tener más de uno.
- `git reset --hard` no sólo quita todo del staging sino que también elimina los archivos modificados (los trackeados). Si hacemos `git reset --hard origin/master` volveremos al estado que tenemos en el repositorio remoto.
- Con `git reset --soft HEAD~1` podremos deshacer el último commit preservando los cambios que en él introdujimos en el working directory.

## `git reset` vs `git rm`
`git reset` y `git rm` son comandos con utilidades muy diferentes, pero aún así se confunden muy fácilmente.

> `git rm —cached` file will **remove** the file from the stage. That is, when you commit the file will be removed. `git reset HEAD — file` will simply reset file in the staging area to the state where it was on the HEAD commit, i.e. will undo any changes you did to it since last commiting. If that change happens to be newly adding the file, then they will be equivalent.

# `git rebase`
> https://www.youtube.com/watch?v=f1wnYdLEpgI

Cuando desarrollamos una nueva característica hasta ahora hemos visto la técnica de crear un branch `feature` realizar los commits que hagan falta en esa rama (y también habrán sucedido otros commits en `main` por otros miembros del equipo) y luego realizar el `merge` que **creará un nuevo commit con estrategia recursiva**.

Sin embargo, en ocasiones puede que queramos que los commits de `feature` se integren a la historia de la rama `main` como si hubieran ocurrido en ella (y la rama nunca hubiera existido) o bien simplemente queremos mantener el historial limpio sin esos commits que ocasionaría un merge recursivo.

Un ejemplo de uso interesante es si creamos la rama `rama1` y modificamos `file1` y aunque en `main` modifiquemos `file2` ya no nos permitirá realizar un `merge` con fast foward. Entonces tendremos ese commit del merge recursivo que queremos evitar. En ese caso desde la rama hacemos `git rebase main` y los cambios en `file1` aparecerá como que se hicieron a lo último. Una vez hecho esto podremos volver a `main` y hacer `git merge rama1` con la ventaja que será resuelto con **fast forward merge**.

Debemos tener presente que **rebase** que es un comando destructivo que va a alterar el historial, ya que es como si estuviéramos alterando la historia, pero como lo hicimos en una rama que recién creamos no hay ningún problema. La regla de oro respecto al uso de **rebase** es nunca hacerlo en ramas públicas. **rebase** no funciona muy bien en proyectos open-source y pull requests.

> El uso de **rebase** tiene la ventaja por sobre el uso de **merge**, que si hacemos `git log` o con la herramienta gráfica `gitk` veremos todos los commits en línea por lo que sera fácil de seguir el historial aún si tenemos varias personas en el equipo de trabajo.
>
> El uso de **merge** en cambio tiene la ventaja de que tendremos trazabilidad pues sabremos de donde provienen los commits (de un merge de tal rama con `main`)



Es posible que al ejecutar `git rebase` nos encontremos con conflictos por ejemplo si en ambas ramas modificamos el mismo archivo. 

En caso de querer suspender el rebase podríamos ejecutar `git rebase --abort` para volver al estado anterior a ejecutar ese comando. La solución de conflictos es similar a lo explicado al realizar merges. Pero si estamos trabajando en una rama y no hemos hecho rebase en varios días, puede que tengamos múltiples conflictos y no sólo uno. Estos conflictos sucederán en el orden histórico de los commits que se realizaron en la otra rama, aunque estos ya no sean relevantes en "la actualidad". Es por esto que rebasing y hacer que los branches no vivan mucho tiempo, va a reducir significativamente la complejidad de los conflictos.



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
Con `git clean --dry-run` podemos ver una previsualización de los archivos que eliminaremos si ejecutamos `git clean -f`.

> Si tenemos carpetas duplicadas tendremos que borrarlas a mano ya que git se concentra en los archivos.
> `git clean`  **sólo borra las cosas que puede indexar**. Esto significa que si tengo `.gitignore` de `*.jpg`, lógicamente no podré borrar ese tipo de imágenes duplicadas.

# `git cherry-pick`
El comando `git cherry-pick` se utiliza cuando hemos estado trabajando en una rama y queremos en `main` (o en otra rama) sólo uno de esos avances. 

Debemos tener presente que el uso de `cherry-pick` es considerado una mala práctica pues estamos reconstruyendo la historia. Es aconsejable utilizar en cambio `merge`.

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
con `git blame blogpost.html -L35,53 -c` podremos ver quién modificó de la línea 35 a la línea 53.



# Special Markers
Podemos pensar en los **markers** como si fueran punteros, uno especial es el llamado `HEAD` normalmente es el **último commit del branch actual.** Al cambiar de branch `HEAD` cambiará de ubicación de modo tal de coincidir con el último commit de dicho branch. Sin embargo es posible mover manualmente `HEAD` para que apunte a otro lado. Por ejemplo si en el main branch tenemos varios commits y nos movemos hacia uno con `git checkout` , `HEAD` apuntará a dicho commit.





