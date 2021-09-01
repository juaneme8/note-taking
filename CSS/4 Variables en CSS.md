# Variables en CSS
> [Kevin Powell - CSS Variables Playlist](https://www.youtube.com/watch?v=PHO6TBq_auI&list=PL4-IK0AVhVjOT2KBB5TSbD77OmfHvtqUi)

Las variables en CSS nos permitirán almacenar valores de colores, padding, margin, etc y utilizarlos en todos los lugares que queramos, luego en caso de querer realizar cambios podemos modificar en un único lugar y se reflejarán los cambios en todos los lugares donde las hayamos usado.

En realidad las "variables" son *custom properties* que actúan como variables, es por eso que deben estar adentro de un selector.

Es aconsejable tener todas las definiciones de variables en la parte superior, así están todas **en el mismo lugar**. 

Por convención las definiremos en `:root`  por lo que serán variables globales.
Si defino la *custom property* dentro de una clase por ejemplo, sólo será válida dentro de ella. Si quisiera usarla por fuera no funcionaría pero tampoco obtendría un error como ocucrre en SASS (donde se interrumpe la compilación).

`:root` es una pseudo-clase que matchea el *root element* del árbol del documento. En HTML `:root` representa al `<html>` y es idéntico al selector `html` solo que con **mayor especificidad**.

```css
:root{
	--name: value;
}

Luego para utilizarlas
```css
content{
	property: var(--name);
}
```

## Fallback Variables
Cuando estamos debuggeando y tenemos dudas de si está funcionando o no una variable establecemos un fallback es decir un valor que utilizará para la propieidad si no funciona la variable.
```css
color: var(--secondary,yellow);
```

## Reasignar Variables
Es posible reasignar el valor de una variable en una clase específica y para dicho elemento y sus hijos cambiará el valor visto:
```html
<p>Párrafo 1</p>
<div class="distinto">
	<p>Párrafo 2</>
</div>
```
```css
:root{
	--clr-primary: blue;
}
p{
	color: var(--clr-primary);
}
.distinto{
	--clr-primary: red;
}
```
Veremos que como consecuencia de la reasignación el segundo párrafo aparece en rojo.
Esto en cierta medida va en contra de la idea de tener todas las variables definidas en un único lugar para simplificar las modificaciones a futuro y puede ocasionar bugs difíciles de trackear.
Una aplicación concreata de esta reasignación es cuando tenemos una clase `.card` con muchos elementos y también una clase `.card--important` con unos pocos elementos y queremos cambiar en ella un color para destacarla. 

## Variables en Proyecto Real
```css
:root {
	--ff: 'Fira Sans', sans-serif; /* font-family */
	--ff-prm: 'Fira Sans', sans-serif; /* font-family primary */
	--ff-sec: 'Fira Sans', sans-serif; /* font-family secondary */
	--ff-ss: 'Fira Sans', sans-serif; /* font-family sans-serif */

	--fw-n: 300; /* font-weight normal */
	--fw-m: 600; /* font-weight medium */
	--fw-b: 800; /* font-weight bold */

	--fs-2: 5rem; /* font-size para h2 */
	--fs-3: 2rem; /* font-size para h3 */
	--fs-p: 1rem; /* font-size para p */

	--primary-clr: #225560;
	--secondary-clr: #225560;
	--secondary-clr-light: #225560;
	--accent-clr: #225560;

	--shadow: 0 0 1em rgba(0, 0, 0, 0.25);
	/* horizontal offset, vertical offset, blur radius, color */
	/* tengo 3 parámetros antes del color y no 4 porque el spread es opcional */

	--br: 5px; /* border-radius */

	--spacer: 1rem; /* espaciador que podremos usar para el padding */
	--spacer-md: calc(var(--spacer) * 2);
	--spacer-lg: calc(var(--spacer) * 3);
}
```

## CSS Variables vs SASS Variables en Media Queries

Sass es un pre procesador al igual que Less, en ellos le decimos el valor de la variable y cuando conforma el css remplaza en todos los lugares donde usamos las variables por el valor deseado.
Al usar *custom properties* en cambio, estas no son remplazadas. En las dev tools podremos ver por ejemplo `font-family: var(--ff-ss)`
Con Sass no es posible redefinir variables en distitintos lugares sino que tendremos que usar variables distintas.
Otra diferencia es que en Sass las variables no van dentro de un selector como ocurre en css.

Cuando trabajamos con media queries en caso de usar variables de Sass lo que hacemos es asignar una nueva variable a las propiedades que queremos cambiar (en caso de tener un color asignado a múltiples selectores tendremos que reasignarle la nueva variable a cada uno de ellos.

```sass
$h1-fs-small: 45px; 
$h1-fs-big: 100px; 
h1{
	font-size: $h1-fs-small;
}
@media (min-width:800px){
	h1{
		font-size: $h1-fs-big;
	}
}
```
Con las variables de css lo que hacemos es redefinir la misma variable y los cambios se producirán automáticamente en todas las propiedades. 
```css
:root{
	--h1-fs:45px;
}
@media (min-width:800px){
	:root{
		--h1-fs:100px;
	}
}

h1{
	font-size:var(--h1-fs);
}
```
Notar que todas las definiciones de variables están en la parte superior. Resumiendo en css en los media queries sólo redefino las variables y me olvido del resto.

## CSS Variables + JavaScript
El uso de JavaScript junto con CSS variables nos aportará interactividad.

```css
:root{
	--color:blue;
}
h1{
	color:var(--color);
}
```
Luego en el html tenemos un título `h1` y un `input` `type="color"`.

En JavaScript queremos modificar la variable `--color` de acuerdo al color elegido y eso cambiará el color del título.

```js
const colorPicker=document.querySelector('[type="color"]');
colorPicker.addEventListener('change',()=>{
	document.documentElement.style.setProperty('--color',e.target.value);
});
```

Cuando ponemos `document.documentElement` hacemos referencia al elemento raíz del documento (`<html>` o sea root).