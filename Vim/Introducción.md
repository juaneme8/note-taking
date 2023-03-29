# Introducción

> :link: [Vim Tutorial for Beginners](https://youtu.be/RZ4p-saaQkc)
>
> :link: [Vim Basics in 8 Minutes](https://youtu.be/ggSyF1SVFr4)
>
> :link: [Vim Text Editor for Beginners](https://www.youtube.com/playlist?list=PLT98CRl2KxKHy4A5N70jMRYAROzzC2a6x) by LearnLinuxTV

Tanto Vim como Vi son editores de código muy famosos que aún en caso de que no lo usemos habitualmente para desarrollar es importante conocer pues puede que sea el único editor disponible cuando nos conectemos remotamente a un **servidor**.

Existe una alternativa llamada Neovim que es utilizado por muchos desarrolladores en el día a día ya que junto con plugins nos permite tener un editor atractivo y sumado a sus **keybindings** nos permite lograr una gran velocidad a la hora de desarrollar. 

Es importante destacar que los Vim bindings pueden ser utilizados en otras herramientas como por ejemplo en VSCode.

 

# Instalación

En la mayoría de los sistemas Linux o Mac nos encontramos con VIM instalado o al menos con Vi. Debemos tener en cuenta que VIM significa  **Vi iMproved** referencia a una versión mas reciente y mejorada de vi.

En Windows podemos utilizar WSL (Windows Subsystem for Linux) y en ese caso abrimos Ubuntu:

```
sudo apt update
sudo apt install vim
```

Luego podemos verificar que tenemos Vim instalado con: `which vim` y nos aparecerá: `/usr/bin/vim` en caso de que no nos aparezca nada (por ejemplo en algun contenedor) podremos probar con `which vi` y veremos `/usr/bin/vi`.

Luego podemos ver la versión instalada ingresando: `vim` o `vi`



> Según [LearnLinuxTV](https://www.learnlinux.tv/) es recomendable instalar siguiente versión:
>
> ```
> sudo apt install vim-nox
> ```



## Conceptos Básicos

### Buffer

Cuando ingresamos texto en la ventana de Vim no estamos editando un archivo sino que estamos trabajando en lo que se conoce como buffer que no es más que un espacio de memoria. Esto será válido tanto para cuando creamos una archivo nuevo como así también en caso de estar editando un archivo existente que será cargado en memoria en ese buffer.

### Apertura de un archivo

```
vi existing_file.txt
```

### Creación de archivo

Si ingresamos un nombre de archivo que no existe, lo crearemos:

```
vi new_file.txt
```

### Salir de vim

La pregunta mas frecuente de la gente que comienza con Vim suele ser cómo salir. 

* Si queremos salir debemos presionar: `:q` y luego `ENTER`. 

* Si queremos salir sin guardar: `:q!`. No confundir el orden ya que cuando empezamos con signo de exclamación `!` es un comando de terminal (ej `:!ls`).
* Si queremos guardar los cambios `:w`.
* Si queremos guardar los cambios en un archivo con un nombre determinado: `:w text.txt`.

* Si queremos guardar los cambios y salir `:wq`.



# Vim Modes

En vim contamos con tres modos: normal mode, insert mode y visual mode.

## :cactus: Normal Mode

Vim trabaja con distintos modos, al iniciar estamos en **normal mode** donde por ejemplo con `:` ingresaremos un comando.



## :cactus: Insert Mode

Para editar un archivo debemos pasar al **insert mode** esto podemos hacerlo de distintas formas:

*  Con `i` insertamos en la posición donde tenemos el cursor.
* Con  `a` (de appending) insertamos en la posición siguiente a la que tenemos el cursor.
* Con  `o` creamos una nueva línea y comenzamos escribiendo en ella.
* Con  `I` insertamos al comienzo de la línea.
* Con `A` insertamos al final de la línea.
* Con `O` insertamos una nueva línea antes de donde tenemos el cursor.

Si queremos volver a normal mode debemos presionar `ESC`.



## :cactus: Visual mode

Con `v` ingresamos al visual mode que nos permite seleccionar porciones de líneas.



## :cactus:Visual line mode

Con `V` ingresamos al modo visual para la selección de líneas completas. Una vez con la selección realizada podremos presionar `y` para copiar, `p` para pegar, etc como veremos más adelante.



## :cactus:Visual block  mode

Con `Ctrl+V` ingresamos al visual block mode para seleccionar **columnas** de líneas. 



# Comandos

## Mostrar número de líneas

```
:set number
```



## Numeración relativa

En lugar de tener una numeración de líneas absolutas es posible establecerla de manera relativa a la línea en la que nos encontramos.

```
:set relativenumber
```

Esto nos permitirá de un vistazo fácil saber cuanto queremos saltar y ejecutarlo rápidamente.

Otra forma de hacerlo es con `:set nu`

Esto podrá ser combinado con eliminación, copiado, etc

> Si queremos deshabilitarlo podremos hacerlo con `:set norelativenumber` `:set nonu`

## Activar mouse

```
:set mouse=a
```



## Esquema de colores

```
:colorscheme slate
```

Notar que si dejamos un espacio y presionamos `TAB` podremos ver las distintas opciones de colores.



# Archivo de configuración

Para conservar los cambios que realizamos en la configuración cuando cerramos el editor debemos crear un archivo de configuración como veremos a continuación:

```
vim ~/.vimrc
```

```
set number
set relativenumber
set tabstop=4
set shiftwidth=4
set autoindent
set mouse=4
colorscheme slate
```



# :facepunch: Keybindings

# Desplazamiento

En lugar de las flechitas podemos desplazarnos utilizando las teclas en línea`hjkl` de modo que no tengamos que abandonar la parte principal del teclado. Con las de las puntas nos movemos hacia los costados (h izquierda y l derecha) y con las internas abajo y arriba (j para abajo y k para arriba)

* Con  `h` nos  movemos hacia la izquierda.
* Con  `j` nos  movemos hacia abajo.
* Con  `k` nos  movemos hacia arriba.
* Con  `l` nos  movemos hacia la derecha.



* Con `0` nos movemos al comienzo de una línea.
* Con `$` nos movemos al final de una línea.
* Con `gg` nos movemos al comienzo del archivo.
* Con `G` nos moves al final del archivo.
* Con `123G` nos movemos a la línea 123. También podemos hacerlo con el comando `:123`.

* Con `%` estando posicionados sobre una llave de apertura `{` o cierre `}`, nos desplazamos entre una y la otra.



* Con `w` nos movemos hacia la siguiente palabra (si tenemos `Hello-I-am-a-simple-sentence` considerará cada guión como separador de palabra)
* Con `W` nos movemos hacia la siguiente palabra. En este caso es necesario un espacio para que considere que se trata de una nueva palabra.



* Con `b` nos movemos hacia la anterior palabra (si tenemos `Hello-I-am-a-simple-sentence` considerará cada guión como separador de palabra)

* Con `B` nos movemos hacia la anterior palabra. En este caso es necesario un espacio para que considere que se trata de una nueva palabra.

  

* Con `e` nos movemos al final de una palabra (con `w` nos movemos al comienzo de la siguiente.) En este caso también si tenemos `Hello-I-am-a-simple-sentence` considerará cada guión como separador de palabra)

* Con `E` nos movemos al final de una palabra (solo aceptando espacios como separadores).

  

* Con `Ctrl + u` hacemos scroll hacia arriba.
* Con `Ctrl + d` hacemos scroll hacia abajo.



* Con `t*` saltamos hacia adelante a la posición previa al signo que especifiquemos (en este caso al caracter previo al asterisco).
* Con `T*` saltamos hacia atrás a la posición siguiente al signo que especifiquemos (en este caso al caracter siguiente al asterisco).
* Con `f*` saltamos a la posición del signo que especifiquemos (en este caso el asterisco).



# Acciones

* Con `d` eliminamos el contenido seleccionado y pasamos a normal mode.

* Con `dd` eliminamos una línea completa. 

  > :red_circle: Tener presente que en Vim eliminar implica copiar también o cut.

* Con `3dd` eliminamos tres líneas.

* Con `D` eliminamos lo que resta de la línea.

* Con `di` (delete inner) eliminamos el contenido entre llaves de donde tengo el cursor.

* Con `dw` eliminamos la palabra siguiente. Si estamos en el medio de una palabra no la eliminará toda sino la parte restante. Si queremos eliminarla por completo debemos utilizar `diw` (delete inner word).

* Con `d3w` (o `3dw`) eliminamos las tres palabras siguientes.

* Con `db` eliminamos la palabra anterior. Si estamos en el medio de una palabra no la eliminará toda sino la parte restante.

* Con `d0` eliminamos todo el contenido hasta el comienzo de la línea.

* Con `d$` eliminamos todo el contenido hasta el final de la línea.

* Con `di"` (delete inner quotation) podremos eliminar el contenido entre comillas.

* Con `d%` eliminamos todo el contenido entre llaves incluídas las llaves mismas.

* Con `dt*` eliminamos todo el contenido hasta el próximo asterisco (sin incluirlo).

* Con `df*` eliminamos todo el contenido hasta el próximo asterisco incluído.



* Con `c` modificamos el contenido seleccionado (lo borramos pero la diferencia está en que con `d` pasamos a modo normal) y pasamos a insert mode.
* Con `cc` modificamos la línea (se borra el contenido pero no la línea) e ingresamos a insert mode.
* Con `C` modificamos **el resto** de la línea.
* Con `cw` cambiamos el resto de la palabra. Si estamos en el medio de una palabra y queremos modificarla toda podremos utilizar `ciw`.
* Con `ciw` modificamos la palabra en el medio de la cual nos encontramos posicionados.
* Con `ci"` (change inner quotation) podremos cambiar el mensaje entre comillas, esto será útil si tenemos por ejemplo `console.log("Mensaje que queremos modificar")`.
* Con `ci(` cambiamos el contenido entre paréntesis.



* Con  `y` (de yanking) copiamos el contenido seleccionado.

* Con `yy` o con `Y` copiamos una línea completa.

* Con `yiw` copiamos la palabra en el medio de la cual nos encontramos.

* Con `yt{` copiamos todo el contenido hasta la próxima `{` sin incluirla.

* Con `yf{` copiamos todo el contenido hasta la próxima `{` incluyéndola.

  

* Con  `p` pegamos el contenido previamente copiado  en la línea de abajo.

* Con `3p` pegamos el contenido tres veces.

* Con  `P` pegamos el contenido previamente copiado  en la línea de arriba.



* Con `r` reemplazamos un caracter. 



* Con `x` eliminamos un caracter.





# Repetición de acciones

Si queremos movernos 5 líneas para abajo podremos presionar: `5` y la tecla hacia abajo (o lo que es lo mismo `5j`)



# Undo & redo

* `u` deshacer la última acción. Si presionamos `3u` deshacemos las últimas 3 acciones.

  > Debemos tener presente que CTRL+Z nos llevaría el proceso al background por lo que no es el comportamiento esperado.

* `CONTROL+R` para rehacer la última acción.



# Indentación

## Indentación manual

* Con `>>` tabulamos una línea hacia la derecha. Si estamos en visual mode con la línea seleccionada sólo hará falta presionar `>`.
* Con `>>` tabulamos una línea hacia la izquierda. Si estamos en visual mode con la línea seleccionada sólo hará falta presionar `<`.



## Indentación automática

* Con `==` generamos una indentación automática

* Con `=` y estando en visual line mode con texto seleccionado lo indentaremos.

* Con `gg` para desplazarnos al comienzo del archivo y luego `=G` indentamos todo un archivo.

>  La indentación automática será especialmente útil en lenguajes como JavaScript donde las llaves alrededor del código nos ayudan a detectar dónde debemos tabular  `{}`.



# Búsqueda



* Con `/keyword` hacemos un forward search de la "keyword".

  Con`ENTER` nos dirigimos al primer carácter del primer match. 

  Una vez ahí:


* Con `?keyword` hacemos un backward search de la palabra "keyword" hacia atrás. 

Con `n` y `N` navegamos hacia la siguiente y la anterior ocurrencia respectivamente en el mismo sentido de la búsqueda inicial.



* Con `#` y `*` podremos buscar rápidamente la anterior y siguiente ocurrencia respectivamente de la palabra sobre la cual tenemos el cursor. 

* >  Regla mnemotécnica: Asociar con el orden de las teclas en el teclado donde vemos `#*`.



* `:%s/word1/word2/g` para reemplazar la palabra "word1" por "word2" de manera global.

  Con `%` indicamos que queremos hacerlo en todo el archivo y no en una selección y con `s` indicamos substitute.

  

  Con `:s/word1/word2/g`  logramos hacer  lo mismo pero sólamente en una porción de texto que tenemos seleccionada: 

  

* `:%s/word1/word2/gc` para reemplazar la palabra "word1" por "word2" de manera global con confirmación antes de cada cambio.



## Waypoints

Con la tecla `m` seguida de un caracter por ejemplo `a` establecemos un waypoint y de esta manera marcamos una línea de código que queremos visitar a menudo. Luego para visitar esa línea debemos presionar `'a` (single quotation mark)



## Centrar cursor

Con `zz` centramos en la pantalla la línea en la cual tenemos el cursor. Esto es útil especialmente para hacerlo estando ubicados en la última línea si queremos que se mueva a la mitad cosa que no podríamos hacer moviendo la flecha hacia abajo.



## Repetir último comando

Con `.` repetimos el último comando ejecutado. Si anteriormente presionamos `dd` para eliminar una línea con `.` podremos continuar eliminando líneas. Lo mismo si ejecutamos `dt*` eliminaremos hasta el carácter previo a un `*`.



## Registers

Cada vez que copiamos o eliminamos es almacenado en un registro y podemos ver la lista tipeando `:reg` pudiendo acceder de esta manera al nombre del registro y el contenido.

Luego podremos elegir de uno de estos registros para pegar lo que nos dará múltiples clipboards con `"7p` para pegar el registro "7.

Es posible reemplazar el contenido de ese mismo registro con `"7yy`

El registro `"+` hace referencia al clipboard del sistema por lo que podremos usarlo para pasar de vim al clipboard o viceversa.

Con `"0` accedemos a la última cosa que copiamos no que fue copiada por eliminar una línea.



## Macros

Las macros también se almacenan en registros y son secuencias de bindings.

* Con `qa` grabamos una macro e indicamos que queremos que se llame `a`. 

> Luego por ejemplo ingresamos `d3wA 100 Esc`  lo que representa delete three words and jump at the end of the line and add number 100. 

* Con `q` detenemos la grabación de la macro.

* Con `:reg` podremos visualizar las teclas que representan cada macro.

* Con `@a` podremos aplicar la macro grabada anteriormente.

Es posible crear luego una macro a y b y a continuación una macro c que ejecute las macros a y b.



# Plugins

Si bien al comenzar a utilizar Vim es aconsejable utilizar la versión Vainilla, es posible mediante el agregado de plugins lograr una experiencia similar a la que tendríamos con un IDE.

Se aconseja para esto primero instalar **neovim** que es mejor, más rápido y acepta más plugins que Vim no soporta.

```
sudo apt install neovim
```

Luego podremos iniciarlo con `nvim`

Luego podremos instalar los plugins de acuerdo a las instrucciones que tenemos en [este repositorio](https://github.com/junegunn/vim-plug)

El archivo de configuración de neovim debemos crearlo en `/.config/nvim` y llamarlo `init.vim`
