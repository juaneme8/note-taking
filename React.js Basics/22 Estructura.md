# Estructura
Algunas recomendaciones para el manejo de la estructura de archivos en los proyectos con React.

## Carpeta `pages` y `components`
En `src` la carpeta `pages` es donde colocaremos aquellos componentes que serán páginas como por ejemplo si tenemos una sección /about al componente `About` lo pondremos allí.
Mientras que también en `src` pero en la carpeta `components` pondremos al resto de los componentes.

A los componentes que no se reutilizan muchos los renombran en minúsculas `app.tsx`, luego modificamos el import acordemente: `import App from './app';`

A continuación para evitar problemas debido a que renombramos archivos detenemos el server y ejecutamos en VSCode `Restart TS Server`

## Providers
En los proyectos tendremos `main.tsx` encargado de montar providers como `ChakraProvider` o React Router.

## Layout component
En `app.tsx` (lo renombramos a minúsculas por lo dicho anteriormente) llamaremos a `layout.tsx` (también en minúsculas) con todo el layout de la aplicación.