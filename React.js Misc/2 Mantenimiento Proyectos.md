# Mantenimiento Proyectos

## Introducción

En la medida que tengamos Dependabot habilitado en GitHub recibiremos mails periódicamente indicando que tenemos dependencias con problemas de seguridad en alguno de nuestros repositorios. Muchas veces bastará con ejecutar `npm audit fix` y las mismas desparecerán. Sin embargo, en ocasiones es bastante complejo solucionarlos porque se trata de repositorios que hace mucho tiempo no actualizamos y para actualizar los paquetes con vulnerabilidades debemos actualizar muchos otros también.



## Create React App

Queremos actualizar un repositorio que utiliza CRA y como con cada versión no sólo cambia el contenido de `package.json` (dependencias, dependencias de desarrollo, scripts, etc) sino también otros aspectos que suceden en la plantilla `public/index.html`, en el punto de entrada `src/index.js` o en el componente principal `App.js` debemos partir de una instalación limpia para comparar cosa por cosa.

Podemos hacer uso de las diferencias que detecta git pegando el código viejo en el nuevo (o viceversa) y guardando los cambios de manera provisoria.

Según  la [documentación](https://create-react-app.dev/docs/getting-started) podemos crear un nuevo proyecto con:

```
npx create-react-app my-app
cd my-app
npm start
```

Luego vamos comparando archivo por archivo y luego actualizamos con `npm i` para ver si las dependencias propias del proyecto admiten la nueva versión de React.