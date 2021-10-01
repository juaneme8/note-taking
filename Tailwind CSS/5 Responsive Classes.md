# Responsive
Con tailwind no tendremos que usar media queries sino que usaremos en cambio como es de esperarse clases que sólo funcionarán a partir de un determinado `min-width` (implementación *mobile first*)
https://tailwindcss.com/docs/breakpoints
https://tailwindcss.com/docs/responsive-design

```html
<div class="bg-red-500 sm:bg-green-500 md:bg-blue-500 lg:bg-pink-500 xl:bg-teal-500 h-4 w-6"></div>
```
En este ejemplo para pantallas muy pequeñas la caja será de color rojo (como la configuración sin prefijo aplica para todos los tamaños de pantalla y los prefijos sólo a partir de un ancho en adelante hablamos de *mobile first*), luego a partir de `640px` (`sm`) pasará a ser verde, a continuación azul, luego rosa y por último verde azulado.

Por ejemplo si queremos que los links de `Log in` y `Sign up` estén en el medio  para pantallas chicas y a la derecha para pantallas *medium* en adelante, lo hacemos de este modo:
```html
<div class="flex justify-center md:justify-end">
	<a href="#">Log in</a>
	<a href="#" class="ml-2">Sign up</a>
</div>
```