# Flexbox
https://tailwindcss.com/docs/flex

### Alineación en Main Axis
Supongamos que queremos utilizar flexbox de modo tal que botones que digan `Log in` y `Sign up` estén ubicados a la derecha de la página uno al lado del otro  lo hacemos con `flex` y `justify-end`:
```html
<div>
	<a href="#">Log in</a>
	<a href="#">Sign up</a>
</div>
```
Podemos hacerlo de este modo:
```html
<div class="flex justify-end">
	<a href="#">Log in</a>
	<a href="#" class="ml-2">Sign up</a>
</div>
```

### Alineación en Cross Axis
Supongamos ahora que tenemos tres cajas flexibles y queremos que se aliñen en el centro respecto del eje y, lo hacemos con `flex` y `items-center`
```html
<div class="flex items-center">
	<div class="bg-red-500 h-4 w-6"></div>
	<div class="bg-blue-500 h-8 w-6"></div>
	<div class="bg-green-500 h-12 w-6"></div>
</div>
```