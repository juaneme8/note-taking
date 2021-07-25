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

Muchos comandos de Git lanzarán un editor de texto para solicitar más información. Si queremos procesar esa solicitud con Visual Studio Code:

```bash
git config --global core.editor "code --wait"
```

> Con `git config` veremos las posibles opciones con las que podemos usar este comando.
> Con `git config --list` obtenemos todas las configuraciones.
> Con `git config -l` obtenemos todas las configuraciones.
> Con `git config --list --show-origin` obtenemos todas las configuraciones y además donde están guardadas.
> Con `git config user.name` obtenemos el nombre de usuario.
> Con `git config user.mail` obtenemos el correo electrónico.
> Con `git config core.editor`obtneemos el editor que tenemos configurado.


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