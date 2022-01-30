# Vim

> [Vim Basics in 8 Minutes](https://youtu.be/ggSyF1SVFr4)

Tanto Vim como VI son editores de código muy famoso que aún en caso de que no lo usemos habitualmente para desarrollar es importante conocer pues puede que sea el único editor disponible cuando nos conectemos remotamente a un servidor.



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