# Selectores Avanzados
> [w3.org - Selectores avanzados de CSS](https://www.w3.org/community/webed/wiki/Es/Selectores_avanzados_de_CSS)
## Selectores Universales
Los selectores universales seleccionan **todos los elementos** de una página. Por ejemplo, la siguiente regla dice que cada elemento de la página debe tener un borde negro sólido de 1 píxel:
```css
* {
  border: 1px solid #000000;
}
```

Si tengo
```html
<div class="projects">
	<a href="#">
		<h3>titulo proyecto</h3>
		<img src="assets/img.png" alt="imagen proyecto" />
	</a>
</div>
```
Si tengo `.projects a *{...}` es lo mismo que tener `.projects a h3, projects a img{...}` 

### `*,*::before,*::after`
Normalmente se suele poner `*,*::before, *::after{ ... }` hacemos esto cuando queremos seleccionar también a los pseudoelementos (que como no son elementos no forman parte del DOM).  Esto es necesario fundamentalmente si la propiedad no es heredable, ya que si es heredable ya estaré afectando esa propiedad de los pseudoelementos por ser hijos de elementos.: ==ampliar explicación==
```css
*,*::before, *::after{
	...
} 
```

## Selectores de Atributo
Los selectores de atributo le permiten seleccionar elementos en función de los atributos que contienen. Por ejemplo, usted puede seleccionar todos los elementos `<img>` con un atributo `alt` mediante el siguiente selector:
```css
img[alt] {
  border: 1px solid #000000;
}
```
**NOTA**: Usando este selector podría decidir colocar un borde negro alrededor de las imágenes que tienen un atributo `alt` y aplicar un borde rojo brillante al resto de las imágenes. Esta sería una técnica útil a emplear en pruebas de **accesibilidad**.

### Selección por el valor de un atributo
Los selectores de atributo se hacen inmediatamente más útiles si consideramos que puede seleccionar por el  _valor del atributo_, no sólo por el nombre del atributo. La regla siguiente selecciona todas las imágenes con un valor de atributo  `src`  de  `alerta.gif`:
```css
img[src="alerta.gif"] {
  border: 1px solid #000000;
}
```

### Selección basándose en una sub-cadena dentro del valor de un atributo
```css
img[src^="alerta"] {
  border: 1px solid #000000;
}
```
El carácter ^ (al igual que en RegEx) indica que este selector debe seleccionar los elementos `<img>` sólo si tienen la cadena `alerta` al comienzo del valor del atributo `src`.

```css
img[src$="gif"] {
  border: 1px solid #000000;
}
```
El carácter $ (al igual que en RegEx) indica que este selector debe seleccionar los elementos `<img>` sólo si tienen la cadena `gif` al final del valor del atributo `src`. 


```css
img[src*="ert"] {
  border: 1px solid #000000;
}
```
El carácter * indica que este selector debe seleccionar los elementos `<img>` sólo si tienen la cadena `ert` en cualquier lugar dentro del valor del atributo `src`.

#### Ejemplo de uso:
Suponiendo que tenemos varios links, podemos agregar un ícono después de los elementos `<a>` cuyo atributo `href` termine en `.pdf`
```html
<a href="file1.pdf">file1</a>
<a href="file2.png">file2</a>
```
```css
a[href$='pdf']::after {
	content: url('http://lorempixel.com/20/20');
	margin-left: 5px;
	margin-right: 25px;
}

```


### Selección basándose en elementos delimitados dentro del valor de un atributo
1) Si tuviera un elemento en una página con un número de clases aplicadas, por ejemplo:
```html
<article class="resaltado archivo ingles"></article>
```
Podría seleccionarlo usando cualquiera de los siguientes selectores:
```css
article[class~="resaltado"]
article[class~="archivo"]
article[class~="ingles"]
```
El carácter ~ indica que estos selectores deben seleccionar un elemento  `<article>`  cuyo valor de atributo  `class`  es una **lista de valores separados por espacios en blanco**, pero sólo si uno de esos valores es igual al valor indicado dentro de las comillas.

2) Ahora veamos un elemento en una página con un valor de ID en forma de una **lista separada por guiones**:
```html
<article id="ingles-diario-resaltado"></article>
```
Podría seleccionarlo usando el siguiente selector:
```css
article[id|="ingles"]
```
El carácter | indica que este selector debe seleccionar un elemento  `<article>`  cuyo valor del atributo  `id`  es una lista de valores separados por guiones, pero **sólo si el valor más a la izquierda** es  `ingles`.
**Regla Mnemotécnica**: Para buscar los guiones utilizo el guión parado `|`

## Selector de hijos (>)
La siguiente regla cambiará el texto de elementos `<strong>` que son hijos de elementos `<h3>` a color azul, pero no afectará a otros elementos `<strong>`:
```css
h3 > strong {
  color: blue;
}
```
Normalmente es aconsejable no seleccionar con elementos html sino utilizar clases.

**Regla Mnemotécnica**: Pensar en > como una D de direct children

## Selector de descendientes (`" "`)
Los selectores de descendientes son muy similares a los selectores de hijos, excepto los selectores de descendientes seleccionan elementos coincidentes en cualquier lugar en la jerarquía de elementos, no sólo descendientes directos. 
```html
<div>
  <p>Hola</p>
  <article> 
    <p>En este párrafo diré adiós</p>.
  </article>
</div>
```
Si queremos seleccionar ambos `<p>` podemos hacerlo usando el selector de descendientes
```css
div p {
 ...
}
```
Mientras que si usamos el selector de hijos o secundarios `(>)` sólo seleccionaremos al primero.

## Selector de hermano adjacente (`+`)
Conocido como **adjacent sibbling selector**  permite seleccionar un elemento específico que viene inmediatamente después de otro elemento específico, en el mismo nivel de la jerarquía de elementos. Tomemos el ejemplo siguiente
```html
<h2>Mi encabezado</h2>
<p>Mi primer párrafo</p>
<p>Mi segundo párrafo</p>
<p>Mi tercer párrafo</p>
<p>Mi cuarto párrafo</p>
<p>Mi quinto párrafo</p>
```

Notar que selecciono al que nombro en último lugar.

Si desea seleccionar sólo el elemento `<p>` que está inmediatamente después del elemento `<h2>` (y cualquier otro elemento `<p>` en la misma condición que pueda aparecer más adelante en el documento), puede utilizar la siguiente regla:

```css
h2 + p {
 ...
}
```
**Regla Mnemotécnica**: Pensar que el signo `+` forma una A de **adjacent sibbling**.

## Selectores de hermano general (`~`)
El selector de hermano general es muy similar al selector de hermano adyacente, excepto que le permite seleccionar todos los hermanos del tipo de elemento especificado, no sólo el que aparece inmediatamente después del elemento en el lado izquierdo.

```css
h2 ~ p {
 ...
}
```

## Ejemplo Selectores
Si tenemos un código como el siguiente:

```html
<div class="card">
	<h3 class="card__title">Titulo</h3>
	<p class="card__price">Lorem ipsum</p>
	<div class="card__body">
		<p>parrafo 1</p>
		<p>parrafo 2</p>
		<a href="" class="button button--card">boton</a>
	</div>
</div>
<div class="card">
	<p>Contenido</p>
</div>
<p> parrafo suelto 1</p>
<p> parrafo suelto 2</p>
```

Con `.card:first-child` seleccionamos el `div` con la clase card
Con `h3:first-child`seleccionamos el `h3`
Con `.card > *` seleccionamos sólo los descendientes directos es decir `h3`, `p` y el `div` 
Con `.card *` seleccionamos `h3`, `p`, el `div` y los elementos internos.
pero no sus elementos internos.
Con `.card + .card` selecciono el segundo div con la clase `.card`.
Con `.card + p` seleccionamos el primer párrafo suelto.


## Pseudoclases
Las pseudoclases se utilizan para proporcionar estilos, pero no a los elementos sino a los diferentes **estados de los elementos**.

Las **pseudoclases más comunes** que encontrará son las que que se utilizan para aplicarle estilos a los estados de los enlace
-   `:link`  — selecciona enlaces no visitados.
-   `:visited`  — selecciona enlaces que ya ha visitado.
-   `:focus`  — selecciona enlaces en los que se encuentra posicionado el cursor del teclado .
-   `:hover`  — selecciona enlaces sobre los cuales se encuentra posicionado el mouse.
-   `:active`  — selecciona enlaces sobre los que se está haciendo clic en el momento actual.

`:focus`, `:hover`, `:active` pueden utilizarse no sólo en enlaces, sino en los estados de prácticamente cualquier elemento en el que tenga sentido usarlas en una interfaz de usuario.

> En cuando a `:hover` si buscamos inspiración podemos visitar el [SIGUIENTE](https://tympanus.net/codrops/2014/06/19/ideas-for-subtle-hover-effects/) sitio con **Ideas for Subtle Hover Effects**

Otras demos:
> [https://tympanus.net/Tutorials/CaptionHoverEffects/](https://tympanus.net/Tutorials/CaptionHoverEffects/)
> [https://tympanus.net/Tutorials/ShapeHoverEffectSVG/](https://tympanus.net/Tutorials/ShapeHoverEffectSVG/)

### Pseudoclase `:empty`
La pseudoclase `:empty` nos permite seleccionar elementos vacíos, entendiéndose como tales a aquellos que no tienen elementos hijos (nodos) o ni contenido de texto.
En cuanto al tratamiento de los espacios en blanco en la actualidad según la [w3.org](https://www.w3.org/TR/selectors-4/#the-empty-pseudo), los elementos que contengan sólo espacios en blanco serán seleccionados cosa que antes no ocurría. 


### Pseudoclase de negación (not)
La pseudoclase de negación puede utilizarse para aplicar estilos explícitamente a elementos que  **no son**  seleccionados por un selector simple. 
Supongamos que queremos seleccionar a todos los `<section>` que no tienen `id="referencias"`
```html
<section id="resumen"> ... </section>
<section id="experimento"> ... </section>
<section id="pruebas"> ... </section>
<section id="resultados"> ... </section>
<section id="conclusiones"> ... </section>
<section id="referencias"> ... </section>
```
```css
section:not(#referencias) {
  ...
}
```

> En determinadas ocasiones tendremos por ejemplo cierta cantidad de íconos de redes sociales y queremos darle estilo utilizando `display: flex` pero que a su vez esos elementos tengan un `margin-right` determinado. Sin embargo, no quiero que el último elemento tenga ese margen pues sino alteraría el centrado. En ese caso podemos usar el selector: `.container .social-link:not(:last-of-type)`


### Pseudoclase de destino (target)
La pseudoclase `:target` representa al elemento con un id coincidente con el identificador de fragmentos (lo que viene después del #) de la URI.

Supongamos que nuestra hoja de estilo contiene lo siguiente:
```css
:target { border:  2px solid #D4D4D4; background-color:  #e5eecc;  
}
```
Si un usuario accede a la url `http://example.com/example.html#references`, el elemento que tiene ese Id será el target, por lo que los estilos definidos anteriormente se aplican a ese elemento.

### Pseudoclases de Input
Comúnmente las utilizará para aplicar estilos a elementos de formulario de HTML como por ejemplo la validación nativa.

Supongamos que tenemos un input text en un formulario cuyo valor es *required*
```html
<input type="text" required>
```
Podríamos aplicarle estilos sólo cuando la información introducida en él es válida o inválida usando `input:valid {}` o `input:invalid {}`
Otra opción sería cuando los datos son válidos/inválidos y además hago foco: `input:valid:focus {}` o `input:invalid:focus {}`

También aplicarle estilos dependiendo de si está activado (por defecto) o desactivado (mediante el atributo  `disabled`), usando `input:enabled {}` o `input:disabled {}`

Por último, podríamos aplicarle estilos a una casilla de verificación sólo cuando está seleccionada con `input[type="checkbox"]:checked {}`

### Pseudoclases estucturales
Las pseudoclases estructurales son selectores avanzados que le permiten seleccionar elementos específicos en función de su posición en la jerarquía del documento.

`:root`  selecciona el elemento raíz del documento, que suele ser el elemento  `<html>`. `:root` es utilizado para declarar variables CSS globales.

`:nth-child(n)` permite seleccionar al elemento `n` dentro de un conjunto  elementos.
Tener en cuenta que no es que apunto a un elemento y selecciono a su hijo, sino que apunto a los hijos y selecciono el `n`.

```html
<ul>
  <li>Primero</li>
  <li>Segundo</li>
  <li>Tercero</li>
  <li>Cuarto</li>  
  <li>Quinto</li>
  <li>Sexto</li>
  <li>Séptimo</li>
  <li>Octavo</li>
  <li>Noveno</li>
  <li>Décimo</li>
</ul>
```
Para seleccionar únicamente los elementos pares o impares:
```css
li:nth-child(odd)
li:nth-child(even)
```
O podríamos utilizar:
```css
li:nth-child(2n+1)
li:nth-child(2n+0)
```
Algunos ejemplos:
*   `li:nth-child(5)`: selecciona items 5 de la lista.
*   `li:nth-child(4n+1)`: selecciona los items 5 y 9 de la lista.
*   `li:nth-child(3n-2)`: selecciona los items 1, 4 y 7 de la lista.

A continuación, pasemos a  `nth-last-child(n)`. Este hace lo mismo que  `nth-child(n)`, pero cuenta desde el último elemento de la secuencia, no desde el primero.

`nth-of-type(n)`  y  `nth-last-of-type(n)` seleccionarán al elemento `n` del tipo indicado a diferencia de `nth-child(n)`  y  `nth-last-child(n)` que sólo lo seleccionarán si es el hijo `n`

Esto puede verse claramente con el siguiente ejemplo:
```html
<article>  
	<h1>Titulo</h1>
	<p>First paragraph...</p>  
	<p>Lorem ipsum...</p>  
	<p>Dolor sit amet...</p>  
	<p>Consectetur adipisicing...</p>  
</article>
```

```css
p:nth-of-type(odd) {
	color: red;
}
```
Cambiaremos el color de *"First paragraph"* y de *"Dolor sit amet"*

```css
p:nth-child(odd) {
	border: 1px solid blue;
}
```
Como el primer elemento es de tipo `<h1>` pondremos un borde sólo en *"Lorem ipsum"* y *"Consectetur adipisicing..."*

Con  `p:first-child` no seleccionaría ningún párrafo, si quisiera seleccionarlo debería usar `p:first-of-type` que es menos específico.

También contamos con las siguientes pseudoclases:
```css
:first-child()
:last-child()

:first-of-type()
:last-of-type()
```

## Pseudo Elementos
Los pseudoelementos difieren de las pseudoclases en que no seleccionan estados de elementos, sino que seleccionan partes de un elemento.

Los pseudoelementos se escriben de una nueva manera en CSS3 usando doble dos puntos, por ejemplo  `a::after { ... }`, para así diferenciarlos de las pseudoclases.

`::after` y `::before` son `inline` por default, más info en [stackoverflow](https://stackoverflow.com/questions/47755989/what-is-the-default-display-property-of-the-before-and-after-pseudo-elements)

### ::first-letter
```css
p:first-letter {
  font-weight: bold;
  font-size: 300%;
  background-color: red;
}
```
La primera letra de cada párrafo estará ahora en negritas, será 300% más grande que el resto del párrafo y tendrá un fondo rojo. Este es un buen comienzo hacia la creación de un efecto de capitular decente.

### ::first-line
Para colocar en negritas la primera línea de cada párrafo, puede utilizar la siguiente regla:
```css
p::first-line {
  font-weight: bold;
}
```
### ::before y ::after
> Basado en videos Kevin Powell - [Before and After pseudo elements explained - part one: how they work](https://www.youtube.com/watch?v=zGiirUiWslI)
>
> Es posible utilizar los pseudoelementos  `::before`  y  `::after`  para insertar contenido antes y después del elemento que está seleccionando. 

Debemos incluir la propiedad `content` de manera obligatoria y su valor podrá ser:

 1. **Texto** 
 Tener en cuenta que el texto agregado no podrá ser seleccionado
 `content:"Hola";` 

También puede utilizar la función  `attr()`  para insertar los valores de los atributos de los elementos después del elemento. Por ejemplo, puede colocar el destino de cada enlace del documento entre paréntesis después de cada uno de ellos usando la siguiente regla:
```css
a::after {
  content: "" "(" attr(href) ")";
}</code>
```
Esta es una técnica para usarla en una **hoja de estilos de impresión** en la que desea mostrar las direcciones URL en el documento en lugar de mantenerlas escondidas dentro de los enlaces (inútiles en una página impresa).

También puede insertar valores numéricos crecientes después de elementos que se repiten (como viñetas o párrafos) mediante la función  `counter()`.

 2. **Imagen** 
 `content: url('https:picsum.photos/200')`

También podríamos utilizar la siguiente regla para insertar una imagen decorativa después de cada enlace en la página:
```css
a::after {
  content: " " url(flor.gif);
}
```

 3. **Comillas**
Si queremos mostrar comillas de apertura `content:open-quote` o `content:close-quote` si queremos comillas de cierre.

4. **Tooltip**
Para agregar un tooltip debemos utilizar *data attributes* en el HTML para luego acceder a dicho contenido desde el CSS.
```html
<p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Nisi sed, ducimus veritatis numquam dolore <a href="#" data-tooltip="esto es un tooltip">dolorum</a>?
</p>
```
Queremos que el tooltip aparezca al posicionarnos sobre el enlace y agregamos una animación tanto al escalado como a la ubicación.
```css
a[data-tooltip]{
  position: relative;
}

a[data-tooltip]::after{
  content: attr(data-tooltip);
  position: absolute;
  background-color: red;
  white-space: nowrap;
  transform: scale(0);
  transition: transform ease-out 150ms, bottom ease-out 150ms;
  bottom: 0;
  left: 0;
}

a[data-tooltip]:hover::after{
  transform: scale(1);
  bottom: 100%;
}
```

6. `content:""`
 Esto será útil para cuando queremos agregar una forma.

7. **Iconos de Font-Awesome**
> Basado en video de Kevin Powell [Setting up Font Awesome icons as pseudo-elements](https://www.youtube.com/watch?v=lMBa7gLWyO4)
> Más información en la [documentación oficial](https://fontawesome.com/how-to-use/on-the-web/advanced/css-pseudo-elements)

Para utilizar [Font Awesome](https://fontawesome.com/) tenemos distintas opciones:

1. Obtener en la página oficial un kit que será similar a:
```html
<script src="https://kit.fontawesome.com/1d5fd033e0.js" crossorigin="anonymous"></script>
```
2. Ir a [https://cdnjs.com/](https://cdnjs.com/) y buscar Font Awesome y elegir el archivo `all.min.css` y presionar el botón para copiar el tag. Aunque lo ideal en términos de optimización sería elegir el archivo según los íconos que sabemos que vamos a usar.

> En cualquiera de los dos casos pegamos el `<link>` en el `<head>`
> En caso de utilizar Codepen podemos entrar a Pen Settings y pegarlo en Stuff for `<head>`.

Cuando buscamos un ícono en el buscador, la url terminará diciendo `style=solid`,  `style=brands`, esto nos dará la pauta del valor que debemos poner en `font-family` como veremos a continuación. Además el unicode con el que cargamos el `content` lo encontraremos también en esa misma página.
Si queremos cambiarle el color podremos hacerlo directamente con `color`

Por ejemplo para agregar el ícono de Twitter (`style=brands)`, utilizamos `font-family: "Font Awesome 5 Brands"` para este caso trabajamos con `font-weigth:400`.
```css
.class:before{
  font-family: "Font Awesome 5 Brands"; content: "\f099";
}
```
Mientas que para agregar un icono de usuario (`style=solid`)  `font-family: "Font Awesome 5 Free"`. Estableciendo `font-weigth:400` elegimos la versión regular y con `font-weigth:900` la versión solid.
```css
.class:before{
  font-family: "Font Awesome 5 Free"; content: "\f007";
}
```

8. **Contadores**
Suponiendo una situación en la que tengo un `.container` y dentro de el varios divs con la clase `.section` y quiero que cada uno tenga asociado un número.
```css
.container{
	counter-reset: counter-name;
}
.section::after{
	counter-increment: counter-name;
	content:counter(counter-name) ')';
}
```
Notar que incremento y luego presento el valor como el contenido del pseudo elemento.


#### Comentarios Contenido Insertado
Si tenemos `p::before{ }` o `p::after { }` lo que pongamos será insertado antes o después del contenido del párrafo pero no antes del párrafo, es decir que **se comportará como un hijo del párrafo**.

Se recomienda **no insertar información importante con CSS** ya que un usuario puede decidir no usar su hoja de estilos. La regla de oro es que las CSS son para aplicar estilos, el HTML es para el contenido importante.