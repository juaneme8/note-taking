# Modo Oscuro

A la hora de configurar el Darkmode lo primero que tenemos que hacer es modificar el archivo `tailwind.config.js`:

```js
module.exports = {
	darkMode: 'media'
}
```

> Con `media` establecemos que el modo oscuro estará activado o no dependiendo de la configuración del usuario en el sistema operativo. 



Por otra parte con el media `@media (prefers-color-scheme: dark){...}` como indica la [documentación](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme)  podremos aplicar clases sólo si la opción elegida por el usuario en el sistema operativo es dark.

```css
@media (prefers-color-scheme: dark){
	body{
		background: #282c34;
		color: #fff;
	}
}
```



Luego si queremos que un elemento tenga un color si está en modo claro y otro si está en modo oscuro lo hacemos de esta forma:

```html
<div class="text-black dark:text-white-100">
	Contenido
</div>
```



Si en cambio queremos que el usuario pueda toggelar de modo claro a modo oscuro y viceversa debemos configurar `tailwind.config.js` de esta forma:

```js
module.exports = {
	darkMode: 'class'
}
```

Solo se aplicarán las clases de modo oscuro si tenemos un elemento con  la clase `dark` anteriormente en el HTML

```html
<html class="dark">
<body>
  <!-- Will be black -->
  <div class="bg-white dark:bg-black">
    <!-- ... -->
  </div>
</body>
</html>
```

