# Hover Effects
Si queremos aplicar un determinado efecto sólo cuando pasamos el mouse por encima del elemento, podemos hacerlo colocando como prefijo `hover:` antes de la clase del estilo a asignar.
```html
<h1 class="font-bold uppercase p-4 border-b border-gray-100">
	<a href="/" class="hover:text-gray-700">Food Ninja</a>
</h1>
```

Si queremos más de un estilo que se vea modificado al realizar el  *hover*:
```html
<a  href="#" class="btn text-primary border-primary md:border-2 hover:bg-primary hover:text-white">Log in</a>
```
