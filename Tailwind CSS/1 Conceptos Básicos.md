# Tailwind CSS
> Basado en [playlist](https://www.youtube.com/watch?v=bxmDnn7lrnk&list=PL4cUxeGkcC9gpXORlEHjc5bgnIi5HEGhw&ab_channel=TheNetNinja) de The Net Ninja y el [repositorio](https://github.com/iamshaunjp/tailwind-tutorial). 

## Introducción
Tailwind es un framework de CSS que viene con múltiples clases de utilidades que nos permite lograr sitios visualmente atractivos. Es similar a Bootstrap y Materialize pero mucho más de bajo nivel (las clases que nos entrega tienen una correspondencia con propiedades de CSS) ya que no cuenta con componentes, sino que nos da clases de utilidades.

Por ejemplo para hacer una **card** en tailwind lo haremos: `class="rounded shadow p-2 bg-white`. Usando las clases de utilidades logramos hacer nuestros propios **componentes** que podrán ser reutilizados. También cuenta con utilidades que nos permitirán implementar **diseño responsivo** sin tener que utilizar un archivo aparte con el CSS ni usar *media queries*. Nos aporta también mucha más **flexibilidad** logrando diseños únicos.



## Extensión VSCode
Se aconseja instalar `Tailwind CSS IntelliSense de Brad Cornes` de modo tal que nos sugiera los nombres de las clases de tailwind.



# VanillaJS

Implementaremos la configuración necesaria para Tailwind en un proyecto de VainillaJS.

## Instalación

En la [documentación](https://tailwindcss.com/docs/installation) vemos las distintas formas de instalación que ofrece Tailwind. Una de ellas es mediante Tailwind CLI que será ideal para casos en que trabajemos con Vanilla JS pero podremos encontrar apartados para cada Framework (Tailwind, Vite, etc)

> En la última parte veremos que con el comando `npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch` con el cual realizamos el buildeo, es aconsejable incorporar esto al`package.json` como un script llamado `build` que luego correríamos como `npm run build`.

### Live Server
Instalamos el paquete **LiveServer** con el comando:

```bash
npm install -g live-server
```

Luego lo ejecutamos con:

```bash
live-server public
```

### Conclusión Personal

Lo ideal es repasar cada uno de los conceptos de CSS y luego trabajar con la documación en mano ya que a excepción de algunos temas que se verán (como ser `@apply`), Tailwind no aporta muchas cosas nuevas sino que nos da clases para lograr lo mismo que haríamos en Vainilla CSS



# Características

### Preflight

En la [documentación](https://tailwindcss.com/docs/preflight) nos encontramos con información acerca de los estilos base que ofrece Tailwind tanto estilos que fueron quitados como otros agregados de manera opinionada.

## Tipografía

Para trabajar con `font-size` podremos ver el listado de las clases en https://tailwindcss.com/docs/font-size. 
Por ejemplo para representar `font-size: 4rem;` utilizaremos la clase `text-6xl`. 

En cuanto a `font-weight` podremos especificar estas opciones https://tailwindcss.com/docs/font-weight por ejemplo `font-weight: 700` equivale a la clase `font-bold` 

Si queremos que el texto tenga `text-transform: uppercase` podremos poner simplemente la clase `uppercase`

## Colores
Podemos encontrar la paleta default en la [documentación](https://tailwindcss.com/docs/customizing-colors#default-color-palette). Se trata de 22 colores con distintas sombras 

Por ejemplo si queremos el color amarillo con la fuerza 400 debemos poner: `text-yellow-400` (el color más suave será 50 y el más oscuro 900).

## Default spacing scale

En la [documentación](https://tailwindcss.com/docs/customizing-spacing#default-spacing-scale) podemos ver los valores que podremos utilizar para establecer width, height, padding, margin, etc.

* Si queremos trabajar con px al valor deseado debemos dividirlo por 4. Si queremos un ancho de 20px sería `w-5`.
* Si queremos trabajar con rem al valor deseado debemos multiplicarlo por 4. Si queremos un ancho de 1rem sería `w-4`.

### Padding
Podremos ver que `p-1` equivale a `padding: 0.25rem` por lo que `p-4` equivale a `padding: 1rem`.
Podremos especificar `px-`, `py-`, `pt-`, `pb-`, `pr-`, `pl-`

Si queremos agregar un padding de un valor que no está dentro de los disponibles igual podemos hacerlo.



### Margin
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