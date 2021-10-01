# Card Component
Inicialmente las cards tienen esta estructura:
```html
<div>
	<img src="img/stew.jpg" alt="stew" />
	<div> (*)
		<span>5 Bean Chili Stew</span>
		<span>Recipe by Mario</span>
	</div>
</div>
```
(*) ese div es porque quiero aplicarle `margin` al texto, si lo hiciera en el `div` externo la imagen dejaría de estar tocando la parte superior.

Como color d fondo de pantalla usamos`bg-gray-100` y para el color de fondo de la card utilizamos `bg-white`

En el `div` exterior el propósito de `overflow-hidden` es evitar que la imagen fuerce vértices rectos en la parte superior en lugar de los redondeados que indicamos con `rounded`.
En cuanto a la imagen queremos que ocupe todo el ancho lo cual logramos con `w-full`, como queremos que no sea muy alta limitamos su altura con `h-32` y para pantallas más grandes `sm:h-48`. Para evitar que la imagen se distorsione `object-cover` (que equivale a `object-fit: cover;`). El efecto será cómo si efectuáramos un zoom sobre la imagen.
```html
<div class="rounded bg-white border-gray-200 shadow-md overflow-hidden"> 
  <img src="img/stew.jpg" alt="stew" class="h-32 sm:h-48 w-full object-cover">
  <div class="m-4">
    <span class="font-bold">5 Bean Chili Stew</span>
    <span class="block text-gray-500 text-sm">Recipe by Mario</span>
  </div>
</div>
```