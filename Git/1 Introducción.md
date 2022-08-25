# Introducción
> :link: Platzi - Curso Profesional de Git y GitHub (COMPLETO)
> :link: [Playlist](https://www.youtube.com/watch?v=3RjQznt-8kE&list=PL4cUxeGkcC9goXbgTDQ0n_4TBzOO0ocPR) the The Net Ninja.
>
> https://youtu.be/4lxvVj7wlZw
>
> :link: Aprendiendo Git - midudev (LIBRO) 



Git es el sistema de control de versiones más usado en el mundo al punto tal que ha llegado a convertirse en el estándard de facto.

* Sistema distribuido
* Gratuito
* Código Abierto
* Diseñado por Linus Torvalds (creador de Linux)
* Concebido con la idea de ofrecer un gran rendimiento, ocupar menos espacio en disco y evitar la necesidad de un servidor central.
* En 2005 se empezó a desarrollar para ser utilizado con el Kernel de Linux.



## Motivación

Cuando trabajamos un archivo de texto plano (por ejemplo `archivo.txt`) y decidimos realizar cambios, en lugar de guardar el archivo con nuevo nombre `archivo_v2.txt`, `archivo_final.txt`. etc, utilizando el **sistema de control de versiones Git** podremos guardar sólo los cambios que realizamos en dichos archivos. Gracias a Git también podremos manejar los cambios que realicen otros miembros del equipo sobre esos mismos archivos posibilitando así el trabajo colaborativo sin superponerse.   
En caso de **errores** es posible saber quién generó el cambio que los ocasionó y también es posible **recuperar versiones** de manera precisa.
De manera local trabajaremos con Git en la terminal o línea de comandos y si queremos trabajar con otras personas o subir el código al a web podemos utilizar GitHub que es un sistema que guarda el código del proyecto con todas las versiones de cada archivo. 

> Git se trata de una *source control management tool*
> En todas las máquinas que clonen el repositorio hay una copia de toda la historia del proyecto.
> Git es ideal para el manejo de archivos de texto plano (código fuente) mientras que si bien puede utilizarse también con archivos binarios no será tan eficiente a la hora de versionar los cambios. Las buenas prácticas indican que los archivos binarios no deben ser agregados al repositorio y debemos ignorarlos. Por ejemplo una imagen es un archivo binario. Cuando trabajamos con imágenes, ante un cambio por cuestiones de caché puede que GitHub continúe mostrándonos una versión vieja (esto se soluciona actualizando con `CTRL+F5`).
> Hablamos de **cambios atómicos** ya que cada uno de los cambios se guarda de manera independiente.



## Características

* Es un **sistema de control de versiones distribuido** o descentralizado (posibilitando el **manejo offline** y **escalabilidad**).
* Creado por la Fundación Linux en particular por Linus Torvalds, de hecho es el sistema que maneja el kernel de Linux.
* Es un sistema **open-source** y **free**.
* Cuenta con una **comunidad muy grande** y es utilizado por muchos desarrolladores.
* Utiliza **menos espacio de disco** que si copiáramos la totalidad de los archivos cada vez que creamos una nueva versión.
* Favorece la **programación colaborativa**. No sólo en un equipo de trabajo sino de cara al código open-source (sumando además la posibilidad de hacer fork y pull requests).
* Es posible **trabajar en nuevas funcionalidades sin alterar el código principal** (concepto de ramas)
* **Posibilidad de sincronizar los cambios en un repositorio remoto online** (GitHub, GitLab, etc).

  

## Terminología
### Sistema Distribuido:
Git se presenta como un **sistema de control de versiones distribuido**, en el que todos los desarrolladores manejan la totalidad de la información y pueden actuar como cliente o servidor en cualquier momento. Esto se lo logra gracias a que cada vez que sincronizas los cambios con el repositorio remoto, se guarda una copia entera de los datos con toda la estructura y los archivos necesarios. Así ya **no es necesario conectarse a internet** para consultar los cambios históricos sobre un archivo o para ver quién fue la última persona que lo editó, todo se hace directamente sobre tu **copia local** y luego, cuando lo consideres oportuno, puedes enviar esos cambios hacia el **repositorio remoto**.

En los **sistemas de control de versiones centralizados** como SVN en cambio, el código y las distintas versiones de están en el servidor, por lo que cada desarrollador para guardar todos los cambios y actualizaciones que realizaba a los archivos, debía estar fuertemente atado a la conexión a Internet, de otra manera los cambios nunca podrán reflejarse en el **servidor central**.

### CLI vs GUI
Utilizaremos línea de comandos o **CLI** (*command line interface*) en lugar de una **GUI** (*graphical user interface*) por varios motivos.
* Debido a que es el modo en que fue diseñado git originalmente, por lo que los **new features** aparecen disponibles primero para línea de comandos y luego para los entornos gráficos.
* La **ayuda online** que suele ser brindada usando CLI como método standard (ya que la mayoría de los clientes gráficos tienen los mismos comandos).
* La línea de comandos nos aporta **más potencia** ya que los clientes gráficos suelen cubrir sólo los comandos más comunes.   
* Nos permite tener un set de **comandos multiplataforma** (a excepción de algún que otro comando específico).
* Una vez que hayamos aprendido a usar la línea de comandos podremos realizar un **pasaje simple** a un cliente gráfico.

### Repositorio
Se llama repositorio a la **colección de archivos** que se encuentran bajo el sistema de control de versiones.

> Los repositorios pueden ser locales o remotos.
> Podemos tener múltiples repositorios por cada proyecto y estos serán trackeados independientemente.

El **working directory** o **workspace** por otra parte, es la carpeta que contiene los archivos con los que estamos trabajando. Pudiendo esta tener archivos manejados por git o no. Dentro del **workspace** contamos con una carpeta oculta llamada `.git` la cual contiene el **repositorio**. 

> Es recomendable tener **un repositorio para cada parte de la aplicación** por ejemplo uno para el _*front end_* y otro para el _*back end_*.

### Estados Repositorio Local
En el **repositorio git local** existen tres estados o áreas en los que pueden estar los archivos manejados por Git: *working directory*, *staging area* y *git repository*.

|                   |              |                |
| :---------------: | :----------: | :------------: |
| WORKING DIRECTORY | STAGING AREA | GIT REPOSITORY |

El **working directory** contiene todos los archivos y carpetas de la aplicación los cuales pueden estar o no manejados por Git (de cualquier modo Git estará al tanto de esos archivos). En el otro extremo tenemos el **repositorio** que contiene todos los cambios que han sido guardados o *committed*. En el medio tenemos el **staging area** (memoria RAM o estado temporal de preparación) que se utiliza para preparar el siguiente commit.

> La existencia del *staging area* proporciona una medida extra de seguridad de cara a poder revisar los cambios antes de hacerles commit y además es posible agregar uno a uno los archivos modificados al staging y luego hacer commit de todos ellos. Otra razón es que nos permite modificar varios archivos a la vez y luego hacer commits individuales con estos cambios.



### Commits
Git almacena el **estado** de todos los archivos que maneja y lo hace a través de **commits**, que son *snapshots* o *safe points* cuando alcanzamos un determinado objetivo (*logical point*) y sobre los cuales podremos rebobinar luego de ser necesario.

> Si ejecutamos `mkdir temp` y luego ejecutamos `git status` veremos que nos dirá *nothing to commit*, esto se debe a que **git versiona archivos, no carpetas**. Es por esto que si una cierta tecnología nos obliga a tener carpetas vacías, la convención es crear un archivo vacío adentro llamado `.gitkeep` cosa que hacemos con `touch .gitkeep`. Por tratarse de un archivo cuando lo agreguemos al sistema de control de versiones, tendremos la carpeta también.

## `.gitignore`
No todos los archivos con los que trabajamos deben ir al repositorio. 

* Archivos que contienen contraseñas o API keys no queremos que estas vayan al repositorio ya que podrían ser vistos por alguien más (sobre todo en proyectos  open-source donde el repositorio es público). 
* Build folders
* Carpetas de configuración del editor de código
* Carpeta `node_modules` ya que queremos que todos los usuarios instalen la aplicación localmente basado en el `package.json`.



Para evitar que un archivo se agregado al repositorio debemos colocarlo en un archivo `.gitignore` que creamos en la raíz del proyecto y donde colocamos la lista de todos los archivos a ignorar.

Supongamos que tenemos un archivo `app.log` que no queremos que forme parte del repositorio, para ello dentro podemos colocar tanto `app.log` o directamente `*.log` para omitir todos los archivos que terminen con `.log`. A partir de ese momento si hacemos `git status` nos dejará de aparecer ese archivo como archivo no trackeado.

> Recordemos que no es una buena práctica incluir archivos binarios en el repositorio. Las imágenes por ejemplo no deberían ser incluidas y deberíamos buscar otra alternativa como CDN (*content delivery network*), hosting externo (como imgur), rsync, etc. Si quisiéramos evitar la presencia de imágenes `.jpg` en el repositorio podríamos poner `*.jpg`.
> Es posible poner comentarios en el `.gitignore` con `#`
> Es posible generar excepciones sobre los archivos a ignorar con `!` por ejemplo si queremos ignorar todos los archivos del directorio `content` menos uno llamado `file1.txt` podemos poner `content/*` y luego `!content/file1.txt`.



## Ignorar siempre los mismos archivos en todos los repositorios

Una forma sería copiar una y otra vez el mismo archivo `.gitignore` en todos los repositorios, pero una solución mas eficiente es crear un archivo `.gitignore` global. Creamos un archivo `~/.gitignore_global` 

> Notar que hablamos de GLOBAL y estamos trabajando en ~ que referencia al USUARIO, lo cual tiene una correspondencia con los niveles de configuraciones que maneja Git donde sistema es todos los usuarios y todos los repos, global es todos los repositorios del usuario logueado y local es a nivel repositorio.

Luego vamos a actualizar la configuración para que lea este archivo de manera global en todos los repositorios locales:

```
git config --global core.excludesfile ~/.gitignore_global
```



### Eliminar `node_modules` una vez pusheado

En ocasiones agregamos al repositorio carpetas o archivos accidentalmente que no queremos que estén allí. Podría ser el caso de `node_modules` o un archivo `.env` subidos por error y no queremos que git siga trackeando sus cambios sino que queremos eliminarlo del repositorio conservándolo en el working directory.

https://techstacker.com/remove-node_modules-after-adding-gitignore/

```
git rm -r --cached node_modules
```
Con `-r` indicamos que queremos que sea recursivo por tratarse de un directorio.



```
git commit -m "removed node_modules from repo"
```

```bash
git push origin master
```



# `readme.md`
 El archivo `readme.md` tiene el propósito de describir de qué se trata el repositorio. Es un archivo `.md`, es decir del lenguaje de marcado Markdown que nos permite formatear archivos de texto plano. Este archivo está ubicado en la raíz del repositorio.

> Tambien es posible agregar código html en ese archivo.



# Instalación

Descargar el instalador en [https://git-scm.com/](https://git-scm.com/) y ejecutar dejando los valores default. 

Algunos comentarios sobre las opciones elegidas:

> En caso de que estemos trabajando con Windows, Git instalará Git Bash que será una Terminal de Linux emulada. Esto no será necesario en caso de trabajar con Linux o sistemas basados en Unix como Mac que cuentan con la Terminal.

 > **Use Git from the Windows Command Prompt** (además de instalar Git Bash podremos utilizar Git con la línea de comandos de Windows).

> En cuanto a la librería de seguridad utilizaremos **OpenSSL** (conceptos como SSH, SSL, llaves públicas y privadas están relacionadas con esta tecnología) que no está instalada por defecto en Windows (que en cambio cuenta con otras alternativas). En cambio en el mundo Unix (Linux y Mac) sí está instalada por defecto.

> Windows y Linux graban los saltos de línea (ENTER) de manera distinta. Al dejar la opción **Checkout Windows-style, commit Unix-style endings**. Esto significa que se reciben los saltos de línea de la manera de Windows y luego cuando se envían al repositorio se convierten a la manera de Unix. Esta suele ser la manera utilizada en un entorno de desarrollo profesional ya que logramos la mayor compatibilidad.

> Con Git Bash en lugar de cmd utilizamos **MinTTY** (que es la terminal por defecto del sistema que emula Linux dentro de Windows).

> Si ya tenemos instalado Git y queremos saber qué versión tenemos podemos ejecutar el comando `git --version` o bien `git version`

> Mas adelante veremos que con `git config --list` o `git config -l` vemos todas las configuraciones asociadas a Git dentro de las cuales veremos las que acabamos de elegir en el proceso de instalación.

# Configuración

## `git config`

El comando `git config` nos permite establecer la configuración de Git. 

Es importante establecer el nombre y correo electrónico que estarán asociados a los cambios que realicemos, en este caso queremos establecerlo de manera global:

```bash
git config --global user.name "Juan Ocho"
git config --global user.email "juaneme8@gmail.com"
```

> Si ya establecimos un nombre de usuario, con `git config --global user.name "Juan Nueve"` podremos sobrescribirlo.
>
> Además de los argumentos `--global` (para todos los repositorios del usuario logueado) es posible utilizar `--local` (**es el valor utilizado por defecto** a nivel repositorio) o `--system` (todos los usuarios y todos los repositorios) según sea la configuración que nos interese realizar o listar. Por ejemplo `git config --system --list`

Muchos comandos de Git lanzarán un editor de texto para solicitar más información. Si queremos procesar esa solicitud con Visual Studio Code:

```bash
git config --global core.editor "code --wait"
```

> Con `git config` veremos las posibles opciones con las que podemos usar este comando.
> Con `git config --list` obtenemos todas las configuraciones. Es posible que **una misma configuración aparezca repetida** esto se debe a que es posible tener distintos archivos `gitconfig` y prevalece el último de la lista (apareceran de arriba a abajo system, global, local).
> Con `git config -l` obtenemos todas las configuraciones.
> Con `git config --list --show-origin` obtenemos todas las configuraciones y además donde están guardadas.
> Con `git config user.name` obtenemos el nombre de usuario.
> Con `git config user.mail` obtenemos el correo electrónico.
> Con `git config core.editor`obtenemos el editor que tenemos configurado.
>
> Con `git config --list --show-scope` podremos ver cada configuración y además de donde proviene.


> Notar que si el comando entrega mas de una página podemos ir avanzando con `ENTER` y si nos aparece `(END)` presionamos la `TECLA Q` para salir.
> Notar que a diferencia de cuando usamos como argumento letras sueltas como con `ls -alh` ahora ponemos doble guion con `git config --list` ya que queremos utilizar una palabra.


## Archivo de Configuración

Ingresando `git config --global -e` abrirá el archivo de configuración `.gitconfig`. 

```bash
[core]
	editor = code --wait
[user]
	name = Juan Ocho
	email = juaneme8@gmail.com

(...)
```

> En caso de no haber configurado un editor lo abrirá con el Vim.



# `git init`

## Iniciar Repositorio y crear directorio

El comando `git init` nos permite inicializar un nuevo repositorio. Supongamos queremos inicializar un repositorio Git dentro de la carpeta o _working directory_ `Proyecto1`.

```bash
git init Proyecto1
cd Proyecto1
```

Luego veremos `(main)` lo cual nos da la pauta de que estamos en el main branch.

>  Si queremos que al crear el repositorio la rama princiapl tenga otro nombre podemos hacerlo con:
>
>  ```
>  git init Proyecto1 --initial-branch=main
>  ```
>
>  Otra opción es cambiar `init.defaultBranch` con cuyo valor podemos obtener con `git config --list | grep branch`

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

* Si queremos agregar todos los archivos con cambios

```
git add -all`
```

> Esto es equivalente a ``git add -A` y si estamos en la raíz del directorio de trabajo será lo mismo hacer `git add .`



* Si queremos agregar todos los archivos con una determinada extensión:

```
git add *.js
```

> Notar que a diferencia de cuando

Cuando modificamos un archivo trackeado o creamos uno nuevo, lo veremos en rojo y luego de haberlo añadido al *staging area* lo veremos en verde.



# `git commit`

El comando `git commit` nos permite agregar el contenido del staging area al repositorio local, consiste en agregar un punto en la línea de tiempo al cual podremos volver luego. Este comando agrega todo el contenido del staging area al repositorio.

> En este caso estamos poniendo `commit -m "Mensaje asociado"` pero podríamos poner directamente `git commit` y nos abrirá el editor de código por defecto donde podremos ponerlo, luego guardar y cerrar el archivo.
> Si no establecimos ningún editor con `git config --global core.editor "code --wait" ` abriremos el archivo en Vim. En ese caso presionando `i` pasaremos al modo insertar y luego de ingresar el mensaje deseado presionamos `ESC` `SHIFT+Z` `SHIFT+Z` para guardar y cerrar Vim.
> Si tenemos establecido el Visual Studio Code como editor nos abrirá ese editor para ingresar allí el mensaje que podrá ser multilínea y una vez concluido guardamos dicho archivo `CTRL+S` y cerramos la ventana `CTRL+W`
> Podemos pensar al commit más reciente como la cabecera y lo veremos como `HEAD`, es un apuntador al último commit a la rama en la cual nos encontramos trabajando.

> Si queremos condensar los comandos de add y commit en uno sólo podemos poner `git add . && git commit -m "Mensaje"`

## `git commit -a`

El comando `git commit -a` se conoce como **Express Commit** ya que toma los *tracked files* agrega aquellos con cambios al *staging area* y procede a realizar el commit.

Como queremos agregar también un mensaje en línea agregamos el parámetro `-m` por lo que nos queda `git commit -am "mensaje asociado al commit"`

## `git commit --amend`

Cuando realizo un commit y luego me doy cuenta que me hubiera gustado agregarle más cambios a ese commit, puedo realizar dichos cambios localmente y luego agregar ese archivo al staging area `git add file.txt` y por último ejecutar `git commit --amend`, luego me preguntará si quiero cambiar el mensaje del commit. De esta manera los cambios realizados los pegará en el commit anterior sin realizar uno nuevo.

Si no quiero cambiarle el mensaje puedo poner directamente `git commit --amend --no-edit`

Esto también es útil cuando me equivoqué al escribir el mensaje del commit y quiero cambiarlo. En ese caso también bastará con ingresar `git commit --amend -m "nuevo mensaje"`



# `git restore`

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
git restore *.js
```

Si bien la forma recomendada actualmente es con `git restore` pero este es un comando incorporado a partir de Git 2.23, históricamente esto mismo era realizado utilizando `git checkout` pero como es un comando que sirve también para otras cosas por lo que puede resultar confuso.



### Esto mismo con `git checkout`

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
git checkout -- *.js
```

> Contrariamente a lo indicado en el libro de Midudev (en Windows al menos, al utilizar Cmder) las comillas deben ser dobles por eso ponemos `"*.js"`



## Quitar de Staging Area

El comando `git restore` también nos permite quitar del staging area un archivo dejándolo como cambios en el working directory. Por ejemplo si tenemos un archivo `bio.txt`, al cual le generamos cambios y después hicimos `git add .` podremos quitarlo del staging con `git restore --staged bio.txt`. 



### Con `git reset`

Esto es equivalente a ejecutar `git reset HEAD bio.txt`

> Si en ese momento ejecutamos `git diff bio.txt` veremos las diferencias entre working directory y repositorio local.



# `git clean`

Si hemos realizado una serie de cambios en un archivo que todavía no fue commiteado y queremos deshacer, podemos eliminarlo manualmente o bien usar el comando 

* Si queremos ejecutar un *dry-run* indicándonos qué archivos borraría pero sin hacerlo

```bash
git clean --dry-run
```

> O lo que es lo mismo `git clean -n`

* Si queremos efectuar el borrado de esos archivos

```bash
git clean -f
```

* Si queremos borrar también los directorios que no existían antes

```bash
git clean -d
```

> En ese caso el dry run cuando queremos borrar directorios sería: `git clean -d -n`

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



> Es posible reducir la información mostrada por `git status` con `--short` o `-s`.



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



# Dejar de hacer seguimiento a un archivo

En ocasiones nos damos cuenta que no queremos que un archivo sea parte del repositorio,  deberíamos haberlo agregado al `.gitignore` pero nos damos cuenta tarde.

Supongamos que se trata de un archivo llamado `.env`.

Lo primero que hacemos es agregarlo al `.gitignore` y luego ejecutamos `git rm --cached .env`, por último agregamos los cambios y creamos el commit.



> Tenemos otras dos formas de hacerlo si estamos dispuestos a crear nuevamente ese archivo.
>
> * Agregar el archivo al `.gitignore`, eliminarlo manualmente y luego agregarlo al staging (nos aparecerá como **Changes not staged for commit**) y realizar el commit.
>
> * Agregar el archivo al `.gitignore` y ejecutar `git rm .env` y luego realizar el commit (no hará falta agregarlo al staging ya que nos aparecerá como **Changes to be committed**).
>
> En cualquiera de los casos cuando creemos nuevamente el archivo no podrá incluirse al repositorio ya que lo tendremos ignorado.



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

# `git rm`

Con el comando `git rm` podremos eliminar un archivo (tanto del repositorio como de la copia local). Si en cambio lo borramos con el sistema operativo con `rm` no estaríamos trackeando esa eliminación, por lo que tendríamos que hacer `git add deleted.txt` y luego con `git status` ya veriamos "deleted".

Suponiendo que queremos eliminar `demo.txt` ejecutamos `git rm demo.txt` y al hacer `git status` veremos que la eliminación ya está en la zona de staging y debemos hacer el commit para completarla.

```bash
On branch main
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        deleted:    demo.txt
```

-   `git rm --cached`: Elimina los archivos del área de staging pero los mantiene en nuestro disco duro. Si sólo queremos eliminar un archivo (y conservar los cambios localmente) `git rm --cached index.html` y luego tendremos que realizar ese commit.
-   `git rm --force` o `git rm -f`: Elimina los archivos de Git y del disco duro.




Suponiendo que creamos un archivo `biografia.txt` y hacemos `git add biografia.txt` y luego:

```bash
git rm --cached biografia.txt
```

Luego de esto el archivo seguirá estando disponible y si hacemos `git status` nos aparecerá como **deleted** y también como  *untracked* a menos que lo pongamos en `.gitignore`. Tendremos que añadir los cambios al staging y generar un commit.

> Es posible utilizar el comando `git rm --dry-run` para ver qué archivos se borrarán sin realizar la operación.





## Eliminar con comando de Bash

Si lo eliminamos usando el sistema operativo con el *bash command*`rm demo.txt`, nos aparecería como *deleted*. Luego con `git add -u` añadimos el archivo eliminado al staging area y luego realizamos el commit.

# Special Markers

Podemos pensar en los **markers** como si fueran punteros, uno especial es el llamado `HEAD` normalmente es el último commit del branch actual. Al cambiar de branch `HEAD` cambiará de ubicación de modo tal de coincidir con el último commit de dicho branch. Sin embargo es posible mover manualmente `HEAD` para que apunte a otro lado. Por ejemplo si en el `main` branch tenemos varios commits y nos movemos hacia uno con `git checkout` `HEAD` apuntará a dicho commit.



## HEAD

HEAD es el puntero que referencia **al punto actual** del historial de cambios del repositorio. Normalmente será el último commit de la rama en la cual nos encontremos pero como también podemos movernos entre commits es posible que HEAD no sea el último commit.



## Obtener referencia de `HEAD`

Es posible saber el valor de `HEAD` utilizando el siguiente comando:

```
git symbolic-ref HEAD
```

El resultado será algo como `refs/heads/main`



## Obtener Valor de `HEAD`

```
git rev-parse HEAD
```

* Mirando el contenido del archivo `.git/HEAD`





# Ramas

## `main` branch

Cuando creamos un repositorio se crea el `main` branch (antiguamente llamado `master` dejado de lado para evitar connotaciones racistas). Siempre debemos tener una rama principal que representa la versión estable de nuestro código (normalmente nos referiremos a el como *stable*,*released*, *production* o *published*) es por eso que a la hora de implementar nuevas funcionalidades no queremos hacerlo en esta rama sino en otra.

Existen equipos de trabajo que a la rama principal la llaman `trunk`.

Los branches nos permiten trabajar con múltiples funcionalidades (probablemente de la mano de distintos programadores) a la vez y luego ir mergeandolos al `main` en la medida que los terminamos.



Los **commits** son guardados en una línea de tiempo conocida como **branch**. Un **repositorio** tendrá por lo menos una rama que por default recibe el nombre de `main`.

> Desde el 1 de octubre de 2020 GitHub cambió el nombre de la rama principal: ya no es `master` sino `main`. Este derivado de una profunda reflexión ocasionada por el movimiento #BlackLivesMatter. La industria de la tecnología lleva muchos años usando términos como master, slave, blacklist o whitelist y de a poco se intenta quitarlos.

Hasta el momento hemos trabajado en el branch default `main`. En ocasiones nos convendrá trabajar con otra rama que es un entorno aislado (línea temporal alternativa que posibilita la **división lógica** del trabajo) sin comprometer la rama principal.  Por ejemplo el uso de ramas será útil si queremos solucionar un bug, desarrollar un nuevo feature, experimentar con una librería, etc y luego volverlas a unir a `main` mediante lo que se conoce como merge si así lo quisiéramos.

> Entre paréntesis podremos ver el branch dentro del cual nos encontramos. Otra forma es con `git status` ya que dirá por ejemplo "On branch main"


## `git branch`

Con `git branch features` creamos una nueva rama llamada `features` . Si lo hacemos desde `main` veremos con `git show` o con `git log` que `HEAD`, `main` y `features` están asociados al mismo commit.



> Para poder crear una rama debemos tener al menos un commit en el repositorio.



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
Con `git show-branch --all` mostramos las ramas locales y remotas y sus cambios.

Con `gitk` abrimos un entorno gráfico en el cual podemos ver la historia (debemos estar en Git Bash para poder hacerlo).

Con `git push origin features` enviamos la rama `features` al repositorio remoto. Sin embargo, puede que no queramos enviar la rama en la que hemos trabajando localmente.

Con `git branch --show-current` podremos saber en qué rama nos encontramos.

Con `git branch --sort=-committerdate` (doble m y **doble t**) mostramos las ramas colocando primera a aquella que tiene el commit más actual.

> Es interesante tener en cuenta que si realizamos modificaciones en el *main branch* (las cuales podremos ver lógicamente con `git status`) y nos damos cuenta que esos cambios deberían estar aislados, podremos crear un nuevo branch y esas modificaciones serán llevadas a ese nuevo branch.

> Usando el comando `git checkout main` para switchear al main branch. Luego de hacer esto podremos ver que `(HEAD, main)` apuntan al mismo commit ID. Cuando hacemos `git checkout features` veremos que el apuntador `HEAD` se mueve al último commit de dicha rama.





# `git switch`

El comando `git switch` nos permite movernos a una rama.



```
git branch feature/responsive-menu
git switch feature/responsive-menu
```



Además es posible crear y movernos a una rama de manera simultánea con:

```
git switch -c bugfix/double-request
```



## El comando que hacía demasiadas cosas: `git checkout`

También es posible utilizar `git checkout` para realizar esto mismo:

```
git branch feature/responsive-menu
git checkout feature/responsive-menu
```

```
git checkout -b 
```



pero este comando realizaba demasiadas cosas entonces en agosto de 2019 se optó por incluir dos subcomandos nuevos `git switch` y `git restore` de manera tal de dividir responsabilidades de `git checkout` en varios comandos.



## Eliminar Ramas

Cuando mergeamos a la rama principal, desde la interfaz de GitLab tendremos la opción de eliminar la rama o conservarla. Hay equipos que deciden conservarla en caso de que surja algún inconveniente que exija algún cambio pero lo más adecuado sería borrarla y si hay algun problema crean una nueva rama porque sino acabaríamos con muchas ramas sin saber cuál está activa.

Si queremos eliminar una rama remota podemos hacerlo desde la interfaz gráfica y luego localmente con `git branch -d feature/database-connection`



## Naming Branches

Además de `main` tendremos una rama `dev` que es código testeado candidato para le próximo **realease**, está listo para producción.

Para el desarrollo de nuevas funcionalidades o solución de bugs lo habitual es tener nombres como `feature/new-table` o `bugfix/new-table`. Es aconsejable tener features pequeños que no lleven mucho tiempo de desarrollo con lo cual será potencialmente menos conflictivo a la hora de realizar el merge.

* Cuando se trabaja por **sprints** el contenido de los branchs de features y bugfixs son mergeados a `dev` y luego del tiempo establecido del sprint serán probados y mergeados a `main`. Esto ocasiona a veces que en la rama `dev` tengamos cosas a medias lo que demanda una mayor atención a la hora de mergear a `main`.

* Cuando desarrollamos con **CI/CD** siguiendo las buenas prácticas de DevOps debemos usar sólo una rama `main` .  En la medida que terminamos un feature o bugfix realizamos un pull request o merge request, luego la persona de realizar el code review podrá **aprobarlo** y luego **mergear** a `main` y esto dispara el pipeline que ejecuta las etapas de test, build y deploy al servidor de staging lo que se conoce como CD.

# Merge

Se conoce como merge al proceso de unir o fusionar los cambios de una rama con otra rama. Por ejemplo si tenemos la `main` y detectamos un bug, creamos un branch `hotfix` luego efectuamos en esa rama los commits que sean necesarios y finalmente cuando estamos satisfechos con el resultado mergeamos esos cambios en `main` donde tenemos el código de producción.

# `git merge`

Con el comando `git merge hotfix` realizamos el merge del branch `hotfix` en el branch en el cual nos encontremos actualmente. Esto podemos hacerlo también desde la UI de GitLab o GitHub.

> Antes debemos hacer `git checkout main` para switchear al branch en el cual queremos unir con `hotfix`. Pero tener presente que el merge puede realizarse en ambos sentidos, no necesariamente siempre hacia el `main`.

Git intentará hacer este merge de manera automática cuando esto sea posible, por lo que vamos a lidiar con distintos tipos de escenarios:

## Merge con Fast-Forward

Es el caso más simple y se da cuando no se detecta trabajo adicional en el *parent branch*, git simplemente aplicará los commits por ejemplo de `hotfix` al `main` branch y será como si el branch secundario nunca hubiera existido. En ese momento con `git log` que `(HEAD-> main, hotfix)` apuntan al mismo commit id. En este caso veremos que no estamos efectuando un nuevo commit producto del merge.

## Merge con Estrategia Recursiva

Cuando Git detecta cambios no conflictivos en el *parent branch* se preservan los dos *timelines* y se crea un *merge commit* automático para mostrar el merge de los dos branches. Esto sucederá por ejemplo cuando en el nuevo branch modificamos un archivo y en el `main` modificamos otro (o líneas distintas) y luego hacemos el merge. 

**Por defecto nos creará un nuevo commit**.



(:warning: no probado) Si queremos evitar que nos haga un commit automáticamente:

```
git merge --edit
git merge --no-commit
```



## Merge Manual

Ocurre cuando Git no es capaz de solucionar los conflictos automáticamente , una vez resueltos son guardados en un *merge commit*. Esto sucede por ejemplo si creamos un branch modificamos una línea, realizamos el commit y luego en el branch `main` también modificamos y commiteamos cambios en la misma línea.
Cuando hacemos `git merge hotfix` desde `main`, nos aparecerá:

```bash
CONFLICT (content): Merge conflict in bio5.txt
Automatic merge failed; fix conflicts and then commit the result.
```



Si bien no es malo que existan conflictos, si esto se convierte en algo demasiado frecuente es posible que debamos intentar evitarlo.



> Para visualizar los conflictos sin abrir el editor debemos ejecutar
>
> ```
> git diff
> ```
>
> Para **abortar el merge** debemos ejecutar:
>
> ```
> git merge --abort
> ```



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



## Evitar Merge Branch Commit al Pullear

Supongamos que estamos trabajando con otras personas de un equipo en unamisma rama. Realizamos cambios, creamos el commit y en el momento de intentar pushear, si otras personas ya pushearon cambios a la rama remota obtendremos un error.  Veremos **failed to push some refs to git@gitlab.com....**. Normalmente nosotros haríamos `git pull` para traernos esos cambios pero esto ocasionaría un "merge commit". Como esto sucederá frecuentemente y estos commits lo único que indican es que hicimos un pull de los cambios para poder hacer un push, se considera una buena práctica evitarlos.

```
git pull --rebase
```

O lo que es lo mismo `git pull -r`



En caso de que hubiera conflictos luego de solucionarnos si ingresamos `git status` veremos que podemos continuar con `git rebase --continue`



## Merge para actualizar rama cuando cambia master

En ocasiones estamos trabajando en una rama solucionando un bug o desarrollando una nueva funcionalidad y mientras tanto la rama `master` evoluciona con nuevos commits. Queremos traernos esos commits a la rama en la cual estamos trabajando.

Por ejemplo desde la rama `bugfix/uuser-auth-error` ejecutamos:

```
git merge master
```



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

Con el comando `git checkout` podremos realizar técnica menos peligrosa para revertir cambios. Nos permite ver el código en un cierto punto (*detach from main branch*) cuando hicimos el commit. Es read-only por lo que no podré arruinar el historial de cambios. Una vez que terminamos de ver y queremos volver al punto actual lo hacemos con `git checkout main` (*reatach to main branch*) o el nombre del branch en el cual estemos.



Es posible crear un branch a partir de este contenido.

> Notar que si tuviéramos cambios realizados cuando queremos ejecutar `git checkout` no nos dejaría por lo que podríamos guardarlos en el stash con `git stash`



### `git checkout .`

El comando `git checkout .` o lo que es lo mismo `git checkout -- .` nos permite revertir los cambios que tengamos en el *working directory* (es decir aquellos que no están en *staging area*).

> Si queremos chequear qué sucederá al hacer esto antes de ejecutarlo podemos ejecutar `git checkout --`
> Si queremos revertir los cambios de un archivo en particular podemos ejecutar `git checkout -- index.html`

## 2 `git revert`

Con el comando `git revert <commit hash>` (**el hash será del commit que queremos revertir**) podremos deshacer un commit en particular, es como si lo elimináramos pero no lo estamos borrando en realidad sino que añadimos un nuevo commit que revierte aquello que en él sucedía. 

> Luego de ingresar este comnando tendremos que colocar un mensaje del nuevo commit.

Esto es utilizado habitualmente cuando se trata de cambios que ya están en el repositorio remoto (habitualmente en `master` o `dev`) donde no es conveniente utilizar `git reset` ya que esto perjudicaría a los otros desarrolladores que ya hayan hecho un pull del repo con esos commits.

## 3 `git reset`

Cuando tenemos commits que queremos eliminar, podemos utilizar el comando `git reset` .

El comando `git reset` nos ayuda a volver en el tiempo. Pero no como `git checkout` que nos deja ir, mirar, pasear y volver. Con `git reset` volvemos al pasado sin la posibilidad de volver al futuro. Borramos la historia y la debemos sobreescribir. 

Este comando es muy peligroso y debemos usarlo solo en caso de emergencia. Recuerda que debemos usar alguna de estas dos opciones:

- `git reset --soft`: Borramos los commits pero guardamos los cambios que estos producían en el directorio de trabajo (y también guardamos los que estaba en staging en el directorio de trabajo), así podemos aplicar las últimas actualizaciones a un nuevo commit.

- `git reset --hard`: Borra toda la información de los commits y lo que tengamos en staging se borra. Los commits posteriores serán eliminados de manera permanente, es por eso que debemos usarlo con cuidado. No sólo quita todo del staging sino que también elimina los cambios de los archivos trackeados que tengamos en el working directory.

  

- Con `git reset HEAD~1` o lo que es lo mismo `git reset --soft HEAD~1` podremos deshacer el último commit preservando los cambios que en él introdujimos en el working directory. Es equivalente a hacer `git reset HEAD^` pero en algunos sistemas el uso del caracter ^ puede ser problemático.

- `git reset af6b84`: Viajamos hasta ese commit y los cambios de los commits que estamos rebobinando siguen disponibles en el working directory, por lo que podríamos ponerlo en el staging area y hacer un commit. Este comando es útil para unir todo los commits y ponerlos en uno sólo. El working directory retiene los cambios.

- Con `git reset --hard HEAD~1` podremos deshacer el último commit y descartar los cambios que introducía. 

- Con `git reset --hard origin/master` volveremos al estado que tenemos en el repositorio remoto. 

- `git reset` o `git reset HEAD` ambos comandos sirven para sacar archivos del área de *staging*. Tendremos los archivos en el directorio de trabajo y si queremeos volver a agregarlos a *staging* podremos hacerlo con `git add`. 
  Con `git reset file.txt` o `git reset HEAD file.txt` sólo quitamos el archivo `file.txt` del staging dejando el resto en caso de tener más de uno.

  



**Importante**: Si los commits los tenemos localmente y realizamos el reset no habrá problemas al pushear **mientras que si ya los teníamos esos cambios en el servidor** tendremos que ejecutar `git push --force`. Sin embargo nunca debemos hacer esto en `master` o `dev` o ramas donde trabajemos con otras personas sino que debemos hacerlo en aquellas donde trabajamos nosotros sólos. Si eliminamos un commit y esos colaboradores habian hecho un pull anteriormente con ese commit, al intentar pushear obtendrán error de las referencias. Si bien puede solucionarse demandará un esfuerzo de parte de ellos.En estos casos en cambio tendremos que utilizar `git revert`



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

Con el comando `git stash` nos permite guardar el *work in progress* es decir los cambios que tengo en el working directory en un lugar temporal.

Esto puede ser útil si tenemos en dos escenarios:

* Cuando tengo una serie de cambios en mi directorio de trabajo en la rama `main` y quiero moverme a una rama `footer` para revisar algo y después volver. Si intentamos hacerlo no nos lo permitirá diciendo **"Please commit your changes or stash them before you switch branches"**.  En ese caso con `git status` vería mis cambios, luego ejecuto `git stash` y ahí con `git status` no veré ya esos cambios. A continuación con  `git checkout footer` me permitirá cambiar de rama sin problemas, reviso lo que tengo que revisar, vuelvo a la rama principal con `git checkout main` y luego `git stash pop` para recuperar esos cambios.
* Cuando tengo cambios en el directorio de trabajo y queremos crear una nueva rama con esos cambios. Con `git stash branch feature` creamos un branch `feature` con el contenido del stash y nos movemos a ella. Con `git status` me aparecerán los archivos que tenía modificados anteriormente.
* Cuando tengo cambios en el directorio de trabajo y quizás algo no está saliendo como espero por lo que quiero deshacerme de ellos, para probar con el estado anterior. En ese caso primero ejecuto `git stash` para volver al último commit y si decido luego eliminar el stash puedo hacerlo con  `git stash drop` .

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



# Repositorios Remotos

Git es el sistema de control de versiones el cual nos permite trabajar de manera **local**, además podríamos crear un servidor de forma tal que cualquier persona pudiera desarrollar de manera **remota**. Sin embargo el mantenimiento que un servidor de este tipo requiere puede ser muy costoso y en raras ocasiones vale la pena. Es por eso que suele utilizarse algún servicio de alojamiento de código en la nube como ser GitHub, GitLab o BitBucket.

 

Cuando trabajamos de manera colaborativa con otras personas, debemos contar un repositorio remoto donde esté almacenada la versión unificada del trabajo de todos ellos. Los ejemplos mas comunes de servidores de repositorios en la nube son GitHub, GitLab pero también es posible montar un servidor propio. 

> GitLab tiene la ventaja de contar con una herramienta llamada GitLab CI que combinada con el uso de ramas nos permite automatizar muchas cosas

Los desarrolladores obtienen los datos de este servidor, luego trabajan localmente y por último mandan datos para que sean visibles por todos.

# GitHub

GitHub es un servicio de **hosting para almacenar repositorios remotos**. Además cuenta con otros servicios como ser hosting para páginas web estáticas, manejo de issues del código, mecanismos para compartir snippets de código, etc. 
Es una de las herramientas colaborativas mas importantes en el mundo del desarrollo.

Como veremos más adelante es una herramienta clave en el **trabajo colaborativo** ya que todos los usuarios podrán clonar el repositorio remoto y trabajar localmente, para luego hacer `push` de los cambios al `main` branch. Complementariamente el resto de los usuarios harán `pull` de manera de mantener todo sincronizado.

## Características

Además de la creación de repositorios podremos efectuar otras tareas con GitHub, por ejemplo si hacemos click en el botón `+` nos aparecen las siguiente opciones:

* New repository
* Import repository (por ejemplo de SVN)
* New gist (snippet de código)
* New organization (empresa)
* New project (conjunto de repositorios)

## Interfaz Gráfica Web

Utilizando GitHub también podremos introducir cambios manuales en el código y luego generar un commit en la rama principal o bien crear una nueva rama e iniciar un pull request.

# `git clone`

El comando `git clone url` nos permite traernos datos de un repositorio remoto, a diferencia del `git init` que usaríamos si quisiéramos inicializar un repositorio local nuevo. Con este comando hacemos una copia del `main` branch al **directorio de trabajo** y también de la base de datos todos los cambios históricos en el **repositorio** local.
Luego efectúo los cambios que considero con el flujo habitual `git add .`, `git commit -m "mensaje"`, etc y cuando termino hago `git push`para que el `HEAD` del `main` branch se envíe al repositorio remoto.

> Lógicamente en caso de que se trate de un repositorio público podremos clonarlo sin la solicitud de credenciales. Sin embargo necesitaremos que el dueño del repositorio nos agregue como **colaboradores** para poder hacer un push a dicho repositorio remoto.
> Al clonar un repositorio los alias con las direcciones del repositorio remoto se crean automáticamente, lo cual podremos chequear con  `git remote -v` donde veremos que `origin` tiene definida la misma dirección para **fetch** y para **push**. 
>
> > `origin` es un alias que hace referencia a la dirección del repositorio remoto.
>
> A la hora de clonar un repositorio si ejecutamos `git clone url` creará una carpeta con el mismo nombre del repositorio remoto mientras que si queremos personalizarlo podremos ponerle `git clone url nombreCarpeta`

# `git fetch`

El comando `git fetch` me permite traerme las actualizaciones del código que hubieran sido realizadas por otros desarrolladores luego de que cloné el repositorio. Esto me lo traerá al repositorio local pero no veré los cambios en el directorio de trabajo, para ellos debemos realizar un `git merge`.Los comandos `git fetch` y `git merge` pueden ser reemplazados por `git pull` que se encarga de ambas tareas.

Recordemos que con `git fetch --all` obtenemos todas las las ramas remotas.



# `git push`

El comando `git push` nos permite enviar el código a un repositorio remoto. Esto nos permitiría que sea accesible por otras personas si así lo deseamos.

Cuando creamos un repositorio en GitHub obtenemos una URL que si estamos trabajando con **HTTPS** será del tipo `https://github.com/juaneme8/test.git` y con **SSH** será `git@github.com:juaneme8/test.git` (suponiendo que `test` es el nombre del repositorio).
Luego suponiendo que queremos subir el branch `main` debemos ejecutar:
`git push https://github.com/juaneme8/test.git main` 

Para evitar poner la dirección cada vez podemos crear un alias `git remote add origin https://github.com/juaneme8/test.git` de modo tal que podremos hacer directamente `git push origin master`

> Como buena práctica antes de hacer un push, debemos hacer un pull para traernos la última versión del código.
> Cuando sólo tengo un lugar no haría falta colocar `git push origin master` (aunque es una buena práctica hacerlo) sino que puedo poner directamente `git push`. Esto será útil por ejemplo si acabo de clonar un repositorio y estoy trabajando en `master`

 





# `git pull`

El comando `git pull` nos permite traer código de un repositorio remoto.

> Cuando creamos un repositorio en GitHub y seleccionamos la opción de crear un archivo `README.md`, luego de establecer el remoto, antes de hacer `git push origin main` (supongamos que tenemos varios commits locales) debemos hacer un pull pues de lo contrario nos saldrá un mensaje rechazando dicho pull dado que el remoto tiene trabajo que no tenemos en la copia local. Sin embargo, si hacemos el pull con `git pull origin master` nos aparecerá el error *fatal: refusing to merge unrelated histories* por lo que ponemos `git pull origin master --allow-unrelated-histories` y luego ingresamos el mensaje de commit deseado.

>Cuando realizamos cambios localmente y hacemos `git pull origin main` (antes de pasarlos a staging y hacer el commit), si ese pull traería datos nuevos puede que nos aparezca un mensaje indicándonos: **Your local changes to the following files would be overwritten... Please commit your changes or stash them before you merge** e indicando los archivos en los cuales sucedería esta sobrescritura. Si vemos que no va a generar errores, hacemos el commit, luego el pull y por último el push.
>Si bien hasta ahora hemos visto como traernos datos del mismo repositorio al cual pusheamos, cuando trabajemos con proyectos forkeados obtendremos datos de una fuente y los enviaremos a otra dirección.

Es posible que al realizar `git push` nos encontremos con que tenemos cambios en el repositorio remoto que no tenemos localmente. Al hacer `git pull` puede que nos diga que existe un conflicto, el cual podremos solucionar de manera similar a lo visto al hacer merges. Otra opción es hacer `git pull origin master --rebase` de manera tal que los cambios que tengamos localmente sean colocados a lo último en la línea temporal.



# `git remote`

El comando `git remote` nos permite establecer el origen remoto de nuestros archivos. Esto lo hacemos con la dirección que obtenemos por ejemplo en GitHub en *Clone or download* ya eligiendo HTTPS o por SSH: `git remote add origin https://github.com/juaneme8/hyperblog`

Otras formas de usar este comando:
Con `git remote` obtenemos los repositorios remotos
Con `git remote -v` obtenemos los repositorios remotos y sus direcciones.
Con `git remote get-url origin` obtenemos la dirección del repositorio remoto.
Con `git remote set-url git@github.com:user/repo.git` cambiamos la dirección de un repositorio remoto (por ejemplo si pasamos de HTTPS a SSH).

> Es posible tener múltiples remotes. 

# Trabajo Colaborativo

Para poder trabajar en un repositorio de manera colaborativa, asumimos que el dueño del repositorio nos añadió como **Collatorators** del proyecto. También veremos que es posible aportar en un proyecto sin ser un **Collaborator** mediante el **Fork**.

En el mundo profesional la rama `main` es la que se va a subir al **servidor de producción**, es por esto que no suele mergearse la rama en la que estemos trabajando a `main` y luego pushearlo, ya que puede que otros desarrolladores no estén de acuerdo con los cambios introducidos. Debemos en cambio, trabajar con otras ramas, solicitar un pull request (o merge request en GitLab) y luego será sometido a un *code review* donde el líder del equipo o del administrador del entorno de desarrollo (DevOps) decide si realiza el merge. 

> En algunos grupos de trabajo se envía un tag a producción en lugar de la rama `main`

Lo normal es contar con un entorno muy similar al de producción pero para realizar pruebas. Así como tenemos el branch `main` tendremos uno de `staging` que es subido a un **servidor de pruebas**. 

En primer lugar realizamos un branch de `main`, trabajamos en la feature o solución de bug que estamos desarrollando, comiteamos los cambios y pusheamos a esa misma rama en el remoto.

```bash
git pull origin master
git checkbout -b feature
(cambios en el código)
git add .
git commit -m "Desarrollo de feature"
git push origin feature
```

Un vez en GitHub veremos un botón **Compare & Pull Request**, con el cual podremos comparar los cambios ingresados en el branch respecto del branch `main`, asignarle la revisión a alguna persona y luego hacer hacer click en **Create Pull-Request** para solicitar mergearlo al branch `main`.

Luego el manager o la persona a la que se lo hayamos asignado, recibirá esta solicitud y podrá realizar un comentario, aprobar los cambios realizados o solicitar nuevos cambios. 

En caso de que el reviewer me indique que debo corregir algo, lo agrego al mismo branch en el que venía trabajando y así hasta que me lo acepte.

Finalmente, el hecho de que los cambios hayan sido aprobados no significa que el merge esté listo, sino que al lado del nombre de aquella persona que fue asignado como *reviewer* aparecerá un tilde verde. Luego podrá además hacer click en **Merge pull request **. Por último podré borrar el branch en un botón que nos ofrece **Delete branch**. 

En este flujo de trabajo profesional, en caso de que los cambios sean aprobados se logra un merge a `staging`. Luego también se realiza un **pull-request** a `main`.

> Los **pull-request** son una característica de GitHub, GitLab, BitBucket y no de Git. 
> Los **pull request** le permite también a personas que no son colaboradores trabajar en una rama. 

Cada vez que haya un mensaje o un cambio de estado en el pull-request recibiremos una notificación.



Una vez que efectuamos un merge request (como se lo llama en GitLab) los commits que realicemos quedarán asociados a el. 

Cuando hacemos un merge request si tenemos un CI/CD sólo nos permitirá realizar el merge si el pipeline no falla.

En GitLab momento de aceptar los cambios de la rama que solicita el merge, podremos también hacer *squash commits* (para unir todos los commits en uno sólo) y *delete source branch* (para eliminar el branch). También podemos configurar cómo queremos que se comporten los **Merge Requests** pudiendo establecer si queremos generar un merge commit o hacer un merge fast forward (que nos aporta una historia mas limpia).



# Otras funcionalidades

GitHub cuenta además con otras funcionalidades muy útiles como una UI amigable, GitHub Actions, Webhooks, Dependabot y otras características que mencionaremos a continuación.



## Insights

En el panel Insights podremos ver mucha información sobre el repositorio. La mayor parte de estos datos están disponibles para repositorios públicos o cuentas GitHub Pro.
Por ejemplo en **Pulse** tenemos info por período acerca de los pull requests, issues, releases, contribuidores, etc. 
En **Contributors** veremos el aporte de cada contribuidor.
En **Community** veremos un checklist de las cosas que debe tener un repositorio idealmente como descripción, README, Code of conduct, License, Issue templates, Pull request template, etc.
En **Traffic** veremos el tráfico del proyecto (no lo veremos en proyecto privados).
En **Commits** veremos la cantidad de commits por día.
En **Code frequency** veremos la relación entre código añadido y código eliminado por semana.
En **Dependency Graph** veremos las dependencias y un enlace al repositorio de cada una de ellas.
En **Network** vemos un timeline de los commits.
En **Forks** veremos qué personas le han hecho fork a este proyecto.



# Fork

Cuando queremos aportar cambios en un proyecto  open-source pero no somos Collaborators del repositorio, no podremos realizar push para subir nuestro trabajo, tampoco podemos crear ramas, ni tags. Es por eso que debemos realizar un **Fork** del repositorio, con lo cual tomamos una copia del estado actual. 
Luego podré acceder a todos los commits, branches, releases, tags, etc. Para trabajar con él localmente tendremos que clonarlo.
Como GitHub sabe que ese proyecto es una bifurcación de un proyecto original, una vez introducidos los cambios puedo solicitar un pull-request haciendo click en **New pull request**. A continuación tenemos que elegir dos ramas para saber qué fue lo que cambió y nos dirá si puede hacerse un merge automático o no. Luego presionamos **Create pull request**.
El desarrollador  dueño del proyecto recibirá una notificación y revisará ese pull request y luego hará click en **Review changes** donde nuevamente podrá elegir entre Comment, Approve o Request changes. En caso de que lo acepte luego hará click en **Merge pull request**

Luego si el dueño del proyecto realiza cambios en un futuro, la persona que quiere aportar al proyecto deberá traérselos a su propio Fork. Esto lo podrá notar desde GitHub ya que verá un cartel **This branch is X commits behind repoOwner:main**. Si queremos resolverlo desde la página vamos a Compare, luego invertimos la dirección de comparación y creamos un pull request del original al nuestro. Luego bastará con aceptarlo y realizar el merge. Luego localmente deberemos hacer un pull de esos cambios.

Esto mismo desde la consola  se puede realizar creando otra fuente para hacer pull. Con `git remote -v` veremos que sólo tenemos `origin` que hace referencia a nuestro repositorio (el que fue creado cuando forkeamos). Para agregar como fuente el repositorio original, primero debemos copiar el link del proyecto original y crear una nueva fuente `git remote add upstream https://github.com/.../blabla.git`. Luego con `git remote -v` veremos que hay una nueva fuente de datos además de `origin`.

> El nombre `upstream` es el que suele usarse pero de más está decir que podríamos haberle puesto cualquier otro.
> `upstream` es un apuntador al repositorio maestro. 

Después ejecutamos `git pull upstream main` (o `git fetch upstream`) para traernos todos los cambios del repositorio original.

A continuación si queremos mergear los cambios efectuados en la rama dev en nuestra rama dev debemos ejecutar `git merge upstream/dev dev`.

Por último `git push origin main` para empujar los cambios a nuestro repositorio remoto. En ese momento nos aparecerá un mensaje **This branch is even with repoOwner:main**

Si queremos cambiar a una rama presente en ambos remotes o fuentes de datos como por ejemplo `git checkout master` nos preguntará si queremos la de origin o la de upstream. En ese caso podremos indicar `git checkout origin/master` (o `git checkout upstream/master`) o bien si queremos que siempre tome por default a origin cuando cambiamos a master o dev `git checkout --track origin/master`. Cuando sea una rama distinta de master nos lo volverá a preguntar.
Si queremos evitar esta pregunta `git config --edit` y debajo de `[core] ....` agregar:

```
[checkout]
  defaultRemote = origin
```

> Tener presente que el repositorio deberá ser público para poder hacer un Fork.
> Fork al igual que Pull-request es una característica de GitHub

# Deployment

Dijimos anteriormente que la rama `main` se suele enviar al servidor de producción. La forma de enviar a producción es similar al flujo de trabajo que venimos utilizando. Suponiendo que tenemos un servidor montado en Linux debemos clonar en él el repositorio. Luego en caso de que se hayan desarrollado nuevas características, para acceder a ellas tendremos que hacer `git pull origin main` para poder acceder a esos cambios. Sin embargo, esto que hemos hecho no es una buena práctica pues debemos proteger el `.git`, ya que alguien podría tener acceso a todos nuestros cambios. Existen formas de protegerlo con Apache y otras con NGINX. Otra forma es realizar esto utilizando **Travis CI** que conecta automáticamente las ramas de GitHub con el servidor. Suponiendo que lo configuramos con una rama `deploy` cuando le hacemos push a esa rama, Travis CI detectará esto y lo enviará al servidor. De la misma manera funcionará si el código debe ser construído. Esta es una herramienta de pago excepto que se trate de un proyecto open-source.
Otra alternativa instalable es **Jenkis** cuyas funcionalidades son más  avanzadas.

# GitHub Pages

GitHub tiene un servicio de hosting llamado GitHub Pages. Gracias a esto el contenido del repositorio podrá ser visto online.

Si queremos crear una página personal debemos crear un repositorio llamado `username.github.io` (cuando esté listo accederemos a la página con esa url). Luego debemos clonar el repositorio vacío, crear un archivo `index.html` y pushear. Como se trata de la página personal no será necesario realizar ninguna configuración adicional en las Settings del proyecto (a diferencia de si se tratara de una página de un proyecto que tendremos que ingresar a la configuración de GitHub Pages).

> En https://pages.github.com/ podremos encontrar la documentación al respecto.



## Renombrar rama de `master` a `main`

Cuando trabajemos con repositorios antiguos y queramos cambiar el nombre de la rama principal de `master` a `main` podemos seguir el siguiente tutorial.

https://dev.to/rhymu8354/git-renaming-the-master-branch-137b







