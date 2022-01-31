# Vim

> [Vim Basics in 8 Minutes](https://youtu.be/ggSyF1SVFr4)
>
> (Vim Text Editor for Beginners)[https://www.youtube.com/playlist?list=PLT98CRl2KxKHy4A5N70jMRYAROzzC2a6x] by LearnLinuxTV

Tanto Vim como VI son editores de código muy famoso que aún en caso de que no lo usemos habitualmente para desarrollar es importante conocer pues puede que sea el único editor disponible cuando nos conectemos remotamente a un servidor.



#### Verificar Instalación

Podemos verificar que tenemos Vim instalado con 

```
which vim
```

En caso de tenerlo instalado veremos en pantalla: `/usr/bin/vim`



Podremos ver la versión instalada ingresando

```
vim
```



Según LearnLinuxTV es recomendable instalar la siguiente versión:

```
sudo apt install vim-nox
```



#### Buffer

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



#### Comandos básicos

* `:q` para salir

* `:w` para guardar los cambios

* `:w text.txt` para guardar los cambios en `text.txt`.

* `:wq` para guardar los cambios y salir

* `:q!` para salir sin guardar los cambios

* `:set number` para mostrar los números al lado de cada línea

* `dd` para eliminar una línea.

* `3dd` para eliminar tres líneas.

* `u` deshacer la última acción.

* `CONTROL+R` para rehacer la última acción (asociar con la búsqueda inversa en la terminal)

* `/keyword` para buscar la palabra "keyword" en el archivo. Con`ENTER` nos dirigimos al primer caracter del primer match. 

  Una vez ahi:

  * Con `n` buscar siguiente (hasta .
  * Con `SHIFT+n` buscar anterior.

* `:%s/word1/word2/g` para reemplazar la palabra "word1" por "word2" de manera global.

* `:%s/word1/word2/gc` para reemplazar la palabra "word1" por "word2" de manera global con confirmación antes de cada cambio.