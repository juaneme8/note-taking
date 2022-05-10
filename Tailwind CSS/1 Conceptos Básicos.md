# Tailwind CSS
Basado en [playlist](https://www.youtube.com/watch?v=bxmDnn7lrnk&list=PL4cUxeGkcC9gpXORlEHjc5bgnIi5HEGhw&ab_channel=TheNetNinja) de The Net Ninja y el [repositorio](https://github.com/iamshaunjp/tailwind-tutorial). 

:warning:  El repositorio que había creado al escribir estas notas ya no está disponible debido a múltiples versiones nuevas que salieron de Tailwind y vulnerabilidades detectadas por Dependabot.

## Introducción
Tailwind es un plugin de PostCSS y se trata de un framework de CSS que viene con clases de utilidades que nos permite lograr sitios visualmente atractivos. Es similar a Bootstrap y Materialize pero mucho más de bajo nivel (las clases que nos entrega sólo modifican una o dos cosas) ya que no cuenta con componentes, sino que nos da clases de utilidades.

Por ejemplo para hacer una **card** en tailwind lo haremos: `class="rounded shadow p-2 bg-white`. Usando las clases de utilidades logramos hacer nuestros propios **componentes** que podrán ser reutilizados. También cuenta con utilidades que nos permitirán implementar **diseño responsivo** sin tener que escribir CSS ni usar *media queries*. Nos aporta también mucha más **flexibilidad** logrando diseños únicos.

Implementaremos la configuración necesaria para Tailwind en un proyecto de VainillaJS pero debemos tener presente que muchos entornos como Laravel, NextJS, Vue CLI nos permiten trabajar con Post CSS de manera inmediata.

## Extensión VsCode
Se aconseja instalar `Tailwind CSS IntelliSense de Brad Cornes` de modo tal que nos sugiera los nombres de las clases de tailwind.

## Instalación Dependencias
Lo primero que hacemos es crear un `package.json`:
`npm init -y`

`npm i tailwindcss`

El proceso es el siguiente: primero creamos un archivo `src/styles.css` en el cual importamos todos los estilos y funcionalidades de tailwind, junto con nuestro propio código css. Luego utilizamos tailwind para procesarlo y entregar un archivo `public/styles.css` en CSS Vainilla que será referenciado por el html.

En `src/styles.css` utilizamos la directiva `@tailwind` para inyectar en nuestro CSS los estilos `base`, `components`, `utilities`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

```

En `package.json` borrar el script de test y dejar sólo:
```json
"scripts": {
		"build": "tailwindcss build src/styles.css -o public/styles.css"
	},
```
Luego al ejecutar `npm run build` logramos que tailwind procese el archivo que acabamos de crear, que da como resultado el archivo de salida en `public` con todas las clases de utilidades que luego podemos utilizar en el HTML.
Debemos tener presente que cada vez que editemos el archivo en `src` tendremos que volver a procesar el archivo.

# Live Server en Public
Instalamos LiveServer con el comando `npm install -g live-server` luego ejecutamos `live-server public`

# Conclusión Personal
Lo ideal es repasar cada uno de los conceptos de CSS y luego trabajar con la documación en mano ya que a excepción de algunos temas que se verán (como ser `@apply`), tailwind no aporta muchas cosas nuevas sino que nos da clases para lograr lo mismo que haríamos en Vainilla CSS
En primer lugar hacemos la estructura de la página sin aplicar clases

## Tipografía
Para trabajar con `font-size` podremos ver el listado de las clases en https://tailwindcss.com/docs/font-size. 
Por ejemplo para representar `font-size: 4rem;` utilizaremos la clase `text-6xl`. 

En cuanto a `font-weight` podremos especificar estas opciones https://tailwindcss.com/docs/font-weight por ejemplo `font-weight: 700` equivale a la clase `font-bold` 

Si queremos que el texto tenga `text-transform: uppercase` podremos poner simplemente la clase `uppercase`

## Colores
Podemos encontrar la paleta default en: https://tailwindcss.com/docs/customizing-colors#default-color-palette

Por ejemplo si queremos el color amarillo con la fuerza 400 debemos poner: `text-yellow-400`

## Padding
Podremos ver que `p-1` equivale a `padding: 0.25rem` por lo que `p-4` equivale a `padding: 1rem`.
También podremos especificar `px-`, `py-`, `pt-`, `pb-`, `pr-`, `pl-`

## Margin
Contamos con clases similares a las vistas para padding.

## Border
https://tailwindcss.com/docs/border-width
Si queremos establecer `border-width: 2px` lo logramos con `.border-2`. Si queremos `border-top-width: 2px;` debemos usar la clase `.border-t-2`. Si no especificamos el número tomará 1px, por ejemplo `border-b` equivaldrá a `border-bottom-width: 1px;`

Para determinar el color del borde lo hacemos con `border-gray-200`

## Ancho y Alto
https://tailwindcss.com/docs/width
https://tailwindcss.com/docs/height

Si queremos darle a un `div` un cierto ancho y alto podemos hacerlo con `h-4` (`height: 1rem;`) y `w-6` (`width: 1.5rem;`) respectivamente.

## `border-radius`
https://tailwindcss.com/docs/border-radius
La clase `.rounded` tiene las siguientes propiedades `border-radius: 0.25rem;`

## `box-shadow`
La clase `.shadow` tiene las siguientes propiedades `box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);`scrimba curso gratis
[https://scrimba.com/g/gtailwind](https://scrimba.com/g/gtailwind)