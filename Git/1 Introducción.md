# Introducción
> Platzi - Curso Profesional de Git y GitHub (COMPLETO 43 VIDEOS)
> [Playlist](https://www.youtube.com/watch?v=3RjQznt-8kE&list=PL4cUxeGkcC9goXbgTDQ0n_4TBzOO0ocPR) the The Net Ninja.
>
> https://youtu.be/4lxvVj7wlZw

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
No todos los archivos con los que trabajamos deben ir al repositorio. Por ejemplo si estamos trabajando con archivos que contienen contraseñas o API keys no queremos que estas vayan al repositorio ya que podrían ser vistos por alguien más (sobre todo en proyectos  open-source donde el repositorio es público).

Para evitar que un archivo se agregado al repositorio debemos colocarlo en un archivo `.gitignore` que creamos en la raíz del proyecto y donde colocamos la lista de todos los archivos a ignorar.

Supongamos que tenemos un archivo `app.log` que no queremos que forme parte del repositorio, para ello dentro podemos colocar tanto `app.log` o directamente `*.log` para omitir todos los archivos que terminen con `.log`. A partir de ese momento si hacemos `git status` nos dejará de aparecer ese archivo como archivo no trackeado.

> Recordemos que no es una buena práctica incluir archivos binarios en el repositorio. Las imágenes por ejemplo no deberían ser incluidas y deberíamos buscar otra alternativa como CDN (*content delivery network*), hosting externo (como imgur), rsync, etc. Si quisiéramos evitar la presencia de imágenes `.jpg` en el repositorio podríamos poner `*.jpg`.
> Es posible poner comentarios en el `.gitignore` con `#`
> Es posible generar excepciones sobre los archivos a ignorar con `!` por ejemplo si queremos ignorar todos los archivos del directorio `content` menos uno llamado `file1.txt` podemos poner `content/*` y luego `!content/file1.txt`.

# `readme.md`
 El archivo `readme.md` tiene el propósito de describir de qué se trata el repositorio. Es un archivo `.md`, es decir del lenguaje de marcado Markdown que nos permite formatear archivos de texto plano. Este archivo está ubicado en la raíz del repositorio.

> Tambien es posible agregar código html en ese archivo.