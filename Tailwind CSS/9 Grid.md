# Grid
En el contenedor colocamos la clase `grid` y luego establecemos el número de columnas con `grid-cols-3` como queremos que esto sólo se aplique para pantallas grandes ponemos `lg:grid-cols-3` de esta manera para pantallas pequeñas las cards estarán una arriba de la otra. El gap lo establecemos con `gap-10`
```html
<div class="mt-8 grid lg:grid-cols-3 gap-10">
<!-- cards go here -->
<div class="card">
	<img src="img/stew.jpg" alt="stew" class="h-32 sm:h-48 w-full object-cover" />
	<div class="m-4">
		<span class="font-bold">5 Bean Chili Stew</span>
		<span class="block text-gray-500 text-sm">Recipe by Mario</span>
	</div>
	<div class="badge">
		<span>25 mins</span>
	</div>
</div>
```

De la misma manera cuando queremos que el contenido de la derecha ocupe más de una columna lo hacemos de este modo. Notar que en pantallas pequeñas se ubicará un bloque arriba del otro.
```html
<div class="grid md:grid-cols-3">
	<div class="md:col-span-1">
	(...)
	</div>

	<main class="md:col-span-2">
	(...)
	</main>
</div>
```