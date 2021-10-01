# Custom Fonts
Tailwind viene con una serie de fuentes que podemos ver en el archivo de configuración dentro del objeto `font-family` en las propiedades `sans`, `serif` y `mono`.
Para utilizarlas lo haremos con la clase `.font-serif`
En caso de que queramos añadir una fuente específica por ejemplo **Nunito**, lo primero que hacemos es copiar el `@import` de Google Fonts. Luego lo pegamos en el archivo `src/styles.css` y por último lo referenciamos en el archivo `tailwind.config.js`.

En `styles.css`
```css
@import url('https://fonts.googleapis.com/css2?family=Nunito:ital,wght@0,300;0,400;0,600;0,700;0,800;0,900;1,200;1,300;1,400;1,600;1,700;1,800;1,900&display=swap');
@tailwind base;
@tailwind components;
@tailwind utilities;

```
En `tailwind.config.js`
```json
module.exports = {
	future: {
		// removeDeprecatedGapUtilities: true,
		// purgeLayersByDefault: true,
	},
	purge: [],
	theme: {
		extend: {
			fontFamily: {
				body: ['Nunito'],
			},
		},
	},
	variants: {},
	plugins: [],
};
```
Finalmente la utilizamos con `.font-body`