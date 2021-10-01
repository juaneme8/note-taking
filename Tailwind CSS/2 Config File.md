# Config
Cuando tailwind procesa nuestro archivo source css utiliza una configuración default para hacerlo, esta configuración determina los valores de propiedades controladas por clases como por ejemplo que `.text-sm` tenga un determinado tamaño de fuente o que`text-red-500` sea un código hexa particular, etc. Podremos crear nuevas clases o bien modificar las actuales.

Para crear el archivo de configuración: `npx tailwindcss init --full` con el flag `--full` logramos crear un *config file* con todos los valores default que usa tailwind *under the hood*

Por ejemplo veremos:
```json
    fontSize: {
      xs: '0.75rem',
      sm: '0.875rem',
      base: '1rem',
	  ...
    }
```
Si quisiéramos podríamos agregar una propiedad por ejemplo `mammoth: '8rem'` de modo tal de poder utilizar `text-mammoth`. Para ello debemos primero guardar el archivo `tailwind.config.js`, luego reprocesar el css fuente con `npm run build`. Sin embargo esto no es aconsejable ya que será difícil saber qué cambios hemos introducido y cuáles son los valores default, por lo que en esos casos se recomienda crear un archivo de configuración en blanco. 
Renombramos el archivo `tailwind.config.js` a `tailwind-default.config.js` (o cualquier otro nombre) para que no lo utilice y creamos un nuevo archivo con `npx tailwindcss init` (sin el flag `--full`).
Veremos
```json
module.exports = {
  future: {
    // removeDeprecatedGapUtilities: true,
    // purgeLayersByDefault: true,
  },
  purge: [],
  theme: {
    extend: {},
  },
  variants: {},
  plugins: [],
}
```
Aunque el archivo esté vacío seguirá tomando los valores default *under the hood*. A continuación si queremos agregar colores y tamaños de fuente personalizados:

```json
module.exports = {
	future: {
		// removeDeprecatedGapUtilities: true,
		// purgeLayersByDefault: true,
	},
	purge: [],
	theme: {
		extend: {
			colors: {
				primary: '#FF6363',
				secondary: {
					100: '#E2E2D5',
					200: '#888883',
				},
			},
			fontSize: {
				mammoth: '8rem',
			},
		},
	},
	variants: {},
	plugins: [],
};

```
Luego para utilizar estos colores debemos volver a procesar el archivo fuente y luego utilizarlo por ejemplo como `.text-primary` o `bg-secondary-200` (si quisiéramos aplicar un color de fondo)