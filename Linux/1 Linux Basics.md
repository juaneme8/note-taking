# Edit in Place

En ocasiones puede que tengamos que editar un archivo de configuración (por ejemplo dentro de un contenedor) y no tengamos ningún editor disponible  (`vi`, `vim`, `nano`).

Una solución podría ser instalar uno de estos paquetes, pero puede que no tengamos privilegios o aún teniéndolos si son cambios pequeños puede que nos convenga editar el archivo al vuelo.

```
sed -i "/s/texttobechanged/textwanted/g" filename
```



Por ejemplo

```
docker exec 900b... sed -i "s/It works!/Docker1/" /usr/local/apache2/htdocs/index.html
```

> En Windows este comando utilizando comillas simples no funcionó.



## Cambiar nombre de host

The procedure to change the computer name on Ubuntu Linux:

1. Type the following command to edit /etc/hostname using nano or vi text editor:
   `sudo nano /etc/hostname`
   Delete the old name and setup new name.

   

2. Next Edit the /etc/hosts file:
   `sudo nano /etc/hosts`
   Replace any occurrence of the existing computer name with your new one.

   

3. Reboot the system to changes take effect:
   `sudo reboot`



## Aumentar Tamaño Fuente Terminal

Para aumentar el tamaño de fuente, debemos editar el archivo `/etc/default/console-setup`

```
sudo nano /etc/default/console-setup
```

Cambiando los siguientes valores:

```
FONTFACE="TER"
FONTSIZE="16x32"
```

Luego aplicamos los cambios con el siguiente comando:

```
sudo update-initramfs -u
```

Luego podemos reiniciar la máquina para ver los cambios `reboot`.