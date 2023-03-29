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

## Normal Mode

Vim trabaja con distintos modos, al iniciar estamos en **normal mode** donde por ejemplo con `:` ingresaremos un comando.



## Insert Mode

Para editar un archivo debemos pasar al **insert mode** esto podemos hacerlo de distintas formas:

*  Con `i` insertamos en la posición donde tenemos el cursor.
* Con  `a` (de appending) insertamos en la posición siguiente a la que tenemos el cursor.
* Con  `o` creamos una nueva línea y comenzamos escribiendo en ella.
* Con  `SHIFT+i` insertamos al comienzo de la línea.
* Con `SHIFT+a` insertamos al final de la línea.
* Con `shift+o` insertamos una nueva línea antes de donde tenemos el cursor.



Si queremos volver a normal mode debemos presionar `ESC`.



## Visual mode

Este modo es utilizado para seleccionar elementos.

Podemos entrar en este modo presionando la tecla `v` y a partir de ese momento cuando nos desplacemos se seleccionará el texto.

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



## Archivo de configuración

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



## Desplazamiento:

En lugar de las flechitas podemos desplazarnos utilizando las teclas en línea`hjkl` de modo que no tengamos que abandonar la parte principal del teclado. Con las de las puntas nos movemos hacia los costados (h izquierda y l derecha) y con las internas abajo y arriba (j para abajo y k para arriba)

* Con  `h` nos  movemos hacia la izquierda.
* Con  `j` nos  movemos hacia abajo.
* Con  `k` nos  movemos hacia arriba.
* Con  `l` nos  movemos hacia la derecha.



* Con `w` nos movemos hacia la siguiente palabra (si tenemos `Hello-I-am-a-simple-sentence` considerará cada guión como separador de palabra)
* Con `W` nos movemos hacia la siguiente palabra. En este caso es necesario un espacio para que considere que se trata de una nueva palabra.



* Con `b` nos movemos hacia la anterior palabra (si tenemos `Hello-I-am-a-simple-sentence` considerará cada guión como separador de palabra)
* Con `B` nos movemos hacia la anterior palabra. En este caso es necesario un espacio para que considere que se trata de una nueva palabra.



* Con `Ctrl + u` hacemos scroll hacia arriba.

* Con `Ctrl + d` hacemos scroll hacia abajo.

  

## Repetición de acciones

Si queremos movernos 5 líneas para abajo podremos presionar: `5` y la tecla hacia abajo (o lo que es lo mismo `5j`)



### Buffer

Cuando ingresamos texto en la ventana de Vim no estamos editando un archivo sino que estamos trabajando en lo que se conoce como buffer que no es más que un espacio de memoria. Esto será válido tanto para cuando creamos una archivo nuevo como así también en caso de estar editando un archivo existente que será cargado en memoria en ese buffer.





# Keybindings

## Undo & redo

* `u` deshacer la última acción. Si presionamos `3u` deshacemos las últimas 3 acciones.

  > Debemos tener presente que CTRL+Z nos llevaría el proceso al background por lo que no es el comportamiento esperado.

* `CONTROL+R` para rehacer la última acción.



## Comandos en modo visual

Con `v` entramos en visual mode, seleccionamos el contenido deseado.

* Con `d` eliminamos el contenido seleccionado y pasamos a normal mode.

* Con `dd` eliminamos una línea completa.

* Con `3dd` eliminamos tres líneas.

* Con `D` eliminamos lo que resta de la línea.

* Con `di` (delete inner) eliminamos el contenido entre llaves de donde tengo el cursor.

* Con `dw` eliminamos la palabra siguiente.

* Con `d3w` (o `3dw`) eliminamos las tres palabras siguientes.

  

* Con `c` modificamos el contenido seleccionado (lo borramos pero la diferencia está en que con `d` pasamos a modo normal) y pasamos a insert mode.

* Con `cc` modificamos la línea (se borra el contenido pero no la línea) e ingresamos a insert mode.

* Con `C` modificamos **el resto** de la línea.



* Con  `y` (de yanking) copiamos el contenido seleccionado.

* Con `yy` o con `Y` copiamos una línea completa.

  

* Con  `p` pegamos el contenido previamente copiado  en la línea de abajo.

* Con `3p` pegamos el contenido tres veces.

* Con  `P` pegamos el contenido previamente copiado  en la línea de arriba.



* Con `r` reemplazamos un caracter. 



* Con `x` eliminamos un caracter.

  



## Búsqueda

* `/keyword` para buscar la palabra "keyword" en el archivo. Con`ENTER` nos dirigimos al primer caracter del primer match. 

  Una vez ahi:

  * Con `n` buscar siguiente ocurrencia.
  * Con `SHIFT+n` buscar anterior.

* `:%s/word1/word2/g` para reemplazar la palabra "word1" por "word2" de manera global.

* `:%s/word1/word2/gc` para reemplazar la palabra "word1" por "word2" de manera global con confirmación antes de cada cambio.


