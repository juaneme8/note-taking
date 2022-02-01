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