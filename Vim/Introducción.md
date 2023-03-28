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

Luego podemos verificar que tenemos Vim instalado con: `which vim` y nos aparecerá: `/usr/bin/vim`

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

* Si queremos guardar los cambios y salir `:wq`.



# Vim Modes

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



### Buffer

Cuando ingresamos texto en la ventana de Vim no estamos editando un archivo sino que estamos trabajando en lo que se conoce como buffer que no es más que un espacio de memoria. Esto será válido tanto para cuando creamos una archivo nuevo como así también en caso de estar editando un archivo existente que será cargado en memoria en ese buffer.



#### Editar un archivo

Para abrir un archivo con Vim 

```
vim file.txt
```



#### Modos de Funcionamiento

Vim cuenta con distintos modos de funcionamiento: *normal mode*, *command mode* e *insert mode*.

Cuando ingresamos estamos en el *normal mode*. 

Si presionamos `:` pasaremos al *command mode*.

Si  presionamos la letra `i` pasaremos al *insert mode*.



# Keybindings

#### Comandos básicos

* `i` para pasar a *insert mode* en la posición actual, `ESC` volveremos al *normal mode*.

* `A` para pasar a *insert mode* al final de la línea.

* `:q` para salir

* `:w` para guardar los cambios

* `:w text.txt` para guardar los cambios en `text.txt`.

* `:wq` para guardar los cambios y salir

* `:q!` para salir sin guardar los cambios

* `:set number` para mostrar los números al lado de cada línea

* `u` deshacer la última acción.

* `CONTROL+R` para rehacer la última acción (asociar con la búsqueda inversa en la terminal)

* `/keyword` para buscar la palabra "keyword" en el archivo. Con`ENTER` nos dirigimos al primer caracter del primer match. 

  Una vez ahi:

  * Con `n` buscar siguiente ocurrencia.
  * Con `SHIFT+n` buscar anterior.

* `:%s/word1/word2/g` para reemplazar la palabra "word1" por "word2" de manera global.

* `:%s/word1/word2/gc` para reemplazar la palabra "word1" por "word2" de manera global con confirmación antes de cada cambio.



#### Eliminar

* `x` para eliminar un caracter.

* `dd` para eliminar una línea.

* `3dd` para eliminar tres líneas.

* `di` delete inner para eliminar el contenido entre llaves de donde tengo el cursor.

* `d3w` eliminar las tres palabras siguientes.

  



#### Copiar

* `yy` copiar línea
* `p` pegar línea



#### Movimiento

* `CTRL+u` scroll hacia arriba
* `CTRL+d` scroll hacia abajo