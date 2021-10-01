# SVG Icons
Utilizaremos https://heroicons.dev/. Sólo tendremos que hacer click sobre un ícono y copiará el SVG para ese ícono en particular. Por ejemplo si queremos mostrar un reloj en el badge podemos hacerlo de este modo:
```html
<div class="badge">
	<svg class="inline-block w-5" fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" stroke="currentColor" viewBox="0 0 24 24"><path d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
	<span>25 mins</span>
</div>
```
Tener en cuenta que por default los elementos `svg` son de bloque, por lo que nos colocará uno arriba del otro, es por eso que colocamos `.inline-block` Otra opción sería aplicar `flex` en el contenedor y a priori creo que sería más adecuado para poder controlar la alineación vertical del ícono y el `span`.