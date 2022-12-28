# Tailwind Portfolio

> 24/03/2022

La idea de este proyecto es tomar un video que utilice React y Vainilla CSS e implementarlo utilizando TailwindCSS a modo de práctica.

El video en cuestión es https://youtu.be/G-Cr00UYokUnpx 



El proyecto será utilizando Next.js y Tailwind CSS por lo que nos basamos en las siguientes guías:

https://nextjs.org/docs

https://tailwindcss.com/docs/guides/nextjs



* Hacemos la limpieza como siempre de las cosas que no vamos a usar.
* Creamos la carpeta `components` en la raíz.
* Creamos la carpeta `/assets/images` en `public` donde colocamos todas las imágenes y el `cv.pdf` lo dejamos suelto.

### Colores

En el proyecto utilizan variables para los colores en CSS, lo que haremos será utilizar la página:



* Para saber si hay un color en Tailwind similar para evitar tener valores arbitrarios:

  https://v3--find-nearest-tailwind-colour.netlify.app/

* Para convertir de RGBA a Hexa utilizaremos: 

  https://rgbacolorpicker.com/rgba-to-hex

Por ejemplo vemos `primary` de `#4db5ff` y buscando vemos que el más similar es `sky-400` . Recordemos que en Tailwind v3 todos los colores de la paleta extendida están disponibles por defecto por lo que podremos usarlos sin modificar modificar nada en `tailwind.config.js`

* `--color-bg: #1f1f38` :left_right_arrow: `gray-800`
* `--color-bg-variant: #2c2c6c` :left_right_arrow: `indigo-900`
* `--color-primary: #4db5ff` :left_right_arrow: `sky-400`
* `--color-primary-variant: rgba(77,181,255,0.4)` :left_right_arrow: 
* `--color-white: #fff` :left_right_arrow: `white`
* `color-light: rgba(255,255,255,0.6)` :left_right_arrow: elegimos en cambio `#fafafa` que en Tailwind es `neutral-50`



En `globals.css` configuramos lo siguiente:

```
html{
  scroll-behavior: smooth;
}

::-webkit-scrollbar {
  display: none;
}
```



## Fuente Custom

Queremos usar la fuente Poppins de Google fonts:

https://fonts.google.com/specimen/Poppins?query=poppins

* Light 300

* Regular 400

* Medium 500

* Semi-bold 600



Lo primero que debemos hacer es importar la fuente en el proyecto, para ello en `globals.css`:

```
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,300;0,400;0,500;1,600&display=swap');
```



Luego debemos configurar para usarla en Tailwind.

You can either overwrite the [default Tailwind fonts](https://tailwindcss.com/docs/font-family). Or you can extend and add your own. Using extend will add the newly specified font families without overriding Tailwind’s entire font stack.

Decidimos extender:

```
const defaultTheme = require('tailwindcss/defaultTheme');

module.exports = {
  content: ['./pages/**/*.{js,ts,jsx,tsx}', './components/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {
      fontFamily: {
        sans: ['Poppins', ...defaultTheme.fontFamily.sans],
      },
    },
  },
  plugins: [],
};

```



# Iconos

Utilizamos React Icons



## Navegación

Utilizamos links con `href="#contact"` y luego en el componente en particular utilizo `<section id="contact">` y navegará hacia esa posición.
