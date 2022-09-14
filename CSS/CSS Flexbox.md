# CSS Flexbox
> Página con ejercicios interactivos para practicar: [flexboxfroggy](https://flexboxfroggy.com/)



Con `flexbox` podremos alinear los elementos en un eje horizontal y en un eje vertical (cosa que antes no era posible). Podremos modificar el orden y la dirección en que aparecen los elementos alterando la posición respecto del flujo.



A la hora de establecer `display: flex` o `display:inline-flex` lo que hacemos es definir al elemento como **contenedor flexible** y todos sus "hijos" como **elementos flexibles**. 

## `display: flex`
El valor `flex` hace que **el contenedor flexible sea un bloque** dentro del elemento "padre" al que pertenezca

Contamos con dos ejes *main axis* (por default es horizontal) y *cross axis*.

> Las propiedades de display no se heredan.

## `display: inline-flex`
El valor `inline-flex` hace que el contenedor flexible sea un elemento "en línea" dentro del elemento "padre" al que pertenezca.

```html
<a href="#">Link0</a>
<nav>
  <a href="#">Link1</a>
  <a href="#">Link2</a>
  <a href="#">Link3</a>
</nav>
```

Si tenemos `nav {display: flex;}` veremos:
```
Link0
Link1 Link2 Link3
```
Mientras que si tenemos `nav { display: inline-flex}` veremos:
```
Link0 Link1 Link2 Link3
```

## `flex-direction`
Especifica la dirección de los items flexibles. Se coloca en el contenedor y el valor por defecto es `row`.

`flex-direction: row|row-reverse|column|column-reverse|initial|inherit;`

## `flex-wrap`
Especifica si los *flex items* deben wrapear o no. Se coloca en el contenedor y el valor por defecto es `no-wrap`.

Cuando establecemos el `width` en flex items si tenemos `flex-wrap: nowrap` en el padre este ancho no se respetará ya que colocará todos los elementos en la misma línea, podemos pensarlo como un max-width.

`flex-wrap: nowrap|wrap|wrap-reverse|initial|inherit;`

## `justify-content`
Alinea los elementos respecto al eje principal. Se coloca en el contenedor y el valor por defecto es `flex-start`

`justify-content: flex-start|flex-end|center|space-between|space-around|space-evenly|initial|inherit;`

## `align-items`
Alinea los elementos respecto al eje secundario. Se coloca en el contenedor y el valor por defecto es `stretch`

`align-items: stretch|center|flex-start|flex-end|baseline|initial|inherit;` 

* `stretch`: rellena todo el espacio del eje secundario.
* `baseline`: alineamos los elementos respecto de una línea imaginaria que es la base de los textos.

## `align-content`
La propiedad `align-content` es similar a `align-items` pero en lugar de alinear *flex items* alinea *flex lines*
Decimos que `align-items` aplica a la alineación dentro de una línea flex mientras que `align-content` aplica dentro de la alineación de líneas flex dentro de un contenedor (si no notamos su efecto chequear que el contenedor tenga establecido un cierto tamaño para manifestar los distintos valores).

`align-content: stretch|center|flex-start|flex-end|space-between|space-around|initial|inherit;`

## `align-self`
La propiedad `align-self` se coloca sobre un hijo nos permite sobrescribir la propiedad `align-items` establecida en el contenedor.

## Alinear Items con Margin:
Si queremos alterar lo establecido con `justify-content` podemos trabajar con las propiedades individuales de margen `margin-left: auto`. Veremos que el **margen izquierdo** al estar en automático tira todo el elemento **hacia la derecha**.

## `flex-wrap`
La propiedad `flex-wrap` determina si los elementos hijos son obligados a permanecer en una misma línea (`flex-wrap: nowrap` valor inicial) o si pueden fluir en varias líneas (`flex-wrap: wrap` o `flex-wrap: wrap-reverse`)

`flex-wrap: nowrap | wrap | wrap-reverse`

## `flex-flow`
La propiedad `flex-flow` es un *shorthand* para `flex-direction` y `flex-wrap`.

```css
flex-flow: row nowrap; //es el valor predeterminado
```

## `order`
Los items flexibles se muestran en el mismo orden que aparecen en el documento, mientras que la propiedad `order` nos permite especificar el orden de un item flexible relativo al resto de los items del mismo contenedor.
El valor por default es 0. Utilizando un `order: 1`  esto hará que el elemento se ubique delante de los elementos que tienen `order:0`, mientras que si `order:-1` esto hará que aparezca detrás de esos elementos.

### Ejemplo:
Si tengo:
`1 2 3 4 5 `
y quiero tener:
`2 4 5 
       1  3 `

```css
.uno, .tres{
	order: 1;
	align-self: flex-end
}
```

### Ejemplo
Si tengo
`1 2 3 4 5 6 7`
y quiero tener:
` 	  5
	  4
7	  3
6	  2
      1
`

```css
.container{
	flex-direction: column-reverse;
	flex-wrap: wrap-reverse;
	justify-content: center;
	align-items:center; 
}
```
## `flex-basis`
La propiedad `flex-basis` nos permite establecer el tamaño inicial de un elemento flexible.

## `flex-grow`
La propiedad `flex-grow` especifica cuánto crecerá el ítem en relación al resto de los items del contenedor.

El valor por default es `flex-grow: 0`

Por ejemplo si quiero que el segundo ítem flexible crezca tres veces mas que el resto:
```css
div:nth-of-type(1) {flex-grow:  1;}  
div:nth-of-type(2) {flex-grow:  3;}  
div:nth-of-type(3) {flex-grow:  1;}
```

Notar que al primero y al tercero les asigno el valor 1 porque con 0 su ancho será el del contenido y no de `1/5` del total como es deseado.

### Ejemplo definiéndolo sólo para uno:
Suponiendo que tengo dos elementos y quiero que uno ocupe todo el espacio disponible menos el espacio del segundo.
```html
<div class="parent">
  <div class="A">A</div>
  <div class="B">B</div>
</div>
```

```css
.parent{
  display: flex;
}

.A{
  flex-grow: 1
}
```
Es decir que en este ejemplo tendremos A ----------------- B

## `flex-shrink`
La propiedad `flex-shrink` especifica cuánto se achicará el ítem en relación al resto de los items del contenedor.

El valor por default es `flex-shrink: 1`

Por ejemplo si quiero que el segundo item flexible se achique tres veces más que el resto:
```css
div:nth-of-type(2) {  
	flex-shrink:  3;  
}
```

## Ejemplo con `flex-grow`, `flex-shrink` y `flex-basis`
```html
<div class="container">
  <div class="item one">Flex Item 1</div>
  <div class="item two">Flex Item 2</div>
  <div class="item three">Flex Item 3</div>
</div>
```

```css
.container{
  display:flex;
}
.one{
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 250px;
  background-color: blue;
}

.two{
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 250px;
  background-color: red;
}

.three{
  flex-grow: 2;
  flex-shrink: 3;
  flex-basis: 250px;
  background-color: pink;
}
```
Del ejemplo anterior podremos ver lo siguiente:
* Cuando tengamos más espacio disponible que 250px para cada elemento, el elemento `.three` crecerá dos veces más rápido que el resto.
* Cuando tengamos menos espacio disponible que 250px el elemento `.three` se achicará más rápido.

## `flex`
La propiedad `flex` es el shorthand de `flex-grow` `flex-shrink` y `flex-basis`. 

Si sólo especificamos el valor de `flex-basis` por ejemplo poniendo `flex: 250px` debemos tener en cuenta lo siguiente. Si bien el valor default de `flex-grow` es 0 y el de `flex-shrink` es 1 cuando utilizamos el shorthand el `flex-grow` es 1 y el `flex-shrink` también es 1.

Si especificamos `flex: 1 250px` estaremos indicando el `flex-grow`
Si especificamos `flex: 2 2` estaremos indicando `flex-grow` y luego `flex-shrink`
Si especificamos `flex: 1` estaremos indicando `flex-grow: 1` y tomaremos el valor `flex-shrink: 1` y `flex-basis:0`