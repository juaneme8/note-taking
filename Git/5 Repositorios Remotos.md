# Repositorios Remotos
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



## Eliminar `node_modules` una vez pusheado

https://techstacker.com/remove-node_modules-after-adding-gitignore/

```
git rm -r --cached node_modules
```

```
git commit -m "Removed node_modules"
```

```bash
git push origin master
```