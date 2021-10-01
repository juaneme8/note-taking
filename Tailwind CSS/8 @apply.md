# @apply
Hasta ahora cuando realizamos **cards** y **badges** tuvimos que aplicar gran cantidad de clases. Esto hace que sea difícil la lectura del código y también en caso de tener que realizar un cambio tendríamos que modificarlo en todos los lugares donde utilizamos dicho elemento.
En `/src/styles.css` crearemos un custom selector de `.card` que nos permitirá aplicar otras clases.

```css
.card {
	@apply rounded bg-white border-gray-200 shadow-md overflow-hidden relative;
}

.badge {
	@apply absolute top-0 ml-2 mt-2 p-2 bg-secondary-100 text-secondary-200 text-xs uppercase font-bold rounded-full;
}


```
Luego al `div` en lugar de darle todas esas clases simplemente le daremos la clase `.card` y `.badge`. Como modificamos el archivo fuente debemos volver a procesar el `css` con `npm run build`

Hacemos lo mismo para darle estilo a los botones mediante la clase `btn`
```css
.btn{
@apply rounded-full py-2 px-3 uppercase text-xs font-bold tracking-wider cursor-pointer;
}
```
`.tracking-wider` equivale a `letter-spacing: 0.05em;`

Notar que sólo incluimos los estilos genérico de todos los botones, mientras que los específicos de cada uno los indicaremos de manera individual, como por ejemplo `text-primary`:

```html
<a  href="#" class="btn text-primary border-primary md:border-2">Log in</a>
```

En este caso sólo queremos que tenga borde para pantallas `md` o superiores, al no tener especificado un width a pesar de tener `border-primary` en pantallas más pequeñas no lo veremos.