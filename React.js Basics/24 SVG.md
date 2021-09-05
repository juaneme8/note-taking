# SVG
A la hora de trabajar con SVGs podemos utilizar la página [react-svgr.com](https://react-svgr.com/) que convierte los archivos `.svg` en componentes React. Para ello tendremos que arrastrar el archivo a la parte izquierda de la pantalla y a la derecha nos entrega el `jsx` del componente.

A este componente podremos pasarle como props `width`, `height`, `stroke`,`fill`. Tener presente para el caso del `fill` que si bien tenemos:

```jsx
<svg
	width={1024}
	height={1024}
	viewBox="0 0 1024 1024"
	fill="none"
	{...props}
>
	<path
		(...)
		fill="#1B1F23"
	/>
</svg>
```

Si bien en la parte de `<svg>` vemos que estamos sobrescribiendo el valor de fill con las props, debemos tener presente que en `path` está sobrescribiendo también esto con un valor hardcodeado.