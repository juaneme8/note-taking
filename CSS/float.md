# `float`
La propiedad `float`  nos permite remover a un elemento del flujo del documento y posicionarlo de modo tal de lograr el layout deseado.
El uso más común de float es para tener texto alrededor de una imagen.

`float: none|left|right|initial|inherit;`

# `clear`
La propiedad `clear` especifica si un elemento puede estar al lado de elementos flotantes que lo preceden o si debe ser movido (*cleared*).
`clear: none|left|right|both|initial|inherit;`

# Ejemplo Float
Si queremos tener una `<section>` ubicada a la izquierda (`float: left)` y ocupando un 60% de la pantalla,  `<aside>` a la derecha (`float: rigth)`y ocupando el 30% de la pantalla y un `<footer>` ocupando todo el ancho cuando terminan ambos elementos anteriores (esto nos da una idea de que usaremos `clear:both;`)

```html
<div class="contenedor">
  <section>
    Lorem, ipsum dolor sit amet consectetur adipisicing elit.
  </section>
  <aside>
    Lorem ipsum dolor sit, amet consectetur adipisicing elit. 
  </aside>
  <footer>
    Lorem ipsum dolor, sit amet consectetur adipisicing elit. Voluptates, doloribus.
  </footer>
</div>
```

```css
section{
  background-color: rgba(0,0,0,.1);
    float: left;
    width: 60%;
}

aside{
  background-color: rgba(0,0,0,.5);
    float: right;
  width: 30%;
}

footer{
  background-color: rgba(0,0,0,.8);
  clear:both;
}
```

## `shape-outside`
La propiedad `shape-outside` controlará cómo el contenido wrapeará alrededor de la caja contenedora del elemento.
Podrá tomar los valores:
* `circle()` para crear formas circulares
* `ellipse()`para crear formas elípticas
* `inset()` par crear formas rectangulares
* `polygon()` para crear formas con mas de tres vértices
* `url()` para indicar alrededor de qué imagen debe wrapear el texto

## `shape-margin`
La propiedad `shape-margin` define un margen para la forma creada con `shape-outside`