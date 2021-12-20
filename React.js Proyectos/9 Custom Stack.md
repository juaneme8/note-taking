# Custom Stack

La idea de este documento es dejar notas sueltas acerca de un custom stack que utiliza las siguientes tecnologías:

* Next.js
* TypeScript
* Tailwind

Lo primero que hacemos es crear un proyecto de Next.js que utilice TypeScript.

```
npx create-next-app@latest --ts
```

Aparecerá un menú interactivo en el cual elegimos el nombre del proyecto y una vez terminado navegamos a esa carpeta e iniciamos el proyecto `npm run dev`

## Instalación Tailwind

Basado en [artículo de Tailwind con Next.js](https://tailwindcss.com/docs/guides/nextjs).



1. Instalamos `tailwindcss` y sus dependencias:

```
npm install -D tailwindcss postcss autoprefixer
```



2. Creamos los archivos `tailwind.config.js` y `postcss.config.js`

```
npx tailwindcss init -p
```



3. Modificamos `tailwind.config.js`

```js
module.exports = {
  content: [
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

4. Añadir las directivas de Tailwind al archivo `styles/globals.css`

```css
@tailwind base;  
@tailwind components;  
@tailwind utilities;
```



## Limpieza del Proyecto

Eliminar el archivo `Home.module.css`



## Estructura Proyecto

Crear en la raíz las siguientes carpetas:

* `components`

  *  `UI`

    dentro de esta carpeta tendremos los componentes de UI reutilizables por ejemplo un botón. Para ello tendremos una carpeta `Button` y dentro dos archivos `Button.tsx` y `index.tsx` con el propósito de simplificar la ruta de importación.

    * `Avatar`

      

* `context`

* `hooks`