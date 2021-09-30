# Stylelint
[Stylelint](https://stylelint.io/) se trata de un **linter para archivos de estilos** con el cual al igual que con ESLint pretendemos evitar errores y cumplir con convenciones en la estructura del código.

Es posible utilizarlo con tanto para `css` como para `scss`

1. Instalar los siguientes paquetes como dependencias de desarrollo:
`npm install --save-dev stylelint stylelint-config-standard`

2. Crear un archivo de configuración  `.stylelintrc.json`  en la raíz del proyecto y colocar lo siguiente:
```json
{
  "extends": "stylelint-config-standard"
}
```

3. Para ejecutar stylelint para todos los archivos `css` del proyecto hacemos lo siguiente: `npx stylelint "**/*.css"`

> Si queremos hacer lo mismo pero para los `scss` `npx stylelint "**/*.scss"`

4. Para solucionar problemas automáticamente ejecutamos: `npx stylelint "**/*.css" --fix`

## Extensión
Para no tener que ingresar esos comandos cada vez utilizamos la extensión [stylelint](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint) que nos marcará los errores de manera automática.

# SCC
Si trabajamos con archivos `.scss` podemos instalar el siguiente [paquete](https://github.com/bjankord/stylelint-config-sass-guidelines) basado en las SCSS guidelines (es para sintaxis SCSS, no para Sass).

`npm i -D stylelint-config-sass-guidelines`

El archivo `.stylelintrc.json` lo editamo de modo de tener lo siguiente:
```json
{
  "extends": "stylelint-config-sass-guidelines"
}
```

> Si vemos que no cambia nada en cuanto a los problemas detectados probar abriendo nuevamente el archivo `scss` o el editor.

> Al parecer este paquete incluye los siguientes plugins (si trabajáramos con `css` y no con `scss` instalaríamos a parte `stylelint-order` así nos marca si las propiedades no están ordenadas alfabéticamente)

>   [`stylelint-order`](https://github.com/hudochenkov/stylelint-order): A plugin pack of order related linting rules for stylelint.
>   ->  [`stylelint-scss`](https://github.com/kristerkari/stylelint-scss): A collection of SCSS specific linting rules for stylelint


Para ejecutar un fix hacemos igual que mencionamos anteriormente: `npx stylelint "**/*.scss" --fix`

# Reglas
* Si nos aparece el error **Selector should be written in lowecase with hyphen (selector-class-pattern)** (y queremos usar BEM) debemos agregar al `.stylelintrc.json` la siguiente regla:
```json
 "rules":{
    "selector-class-pattern": null
  }
```

* Si nos aparece error del tipo **Expected selector X to come before selector Y (no-descending-specificity)"** podemos deshabilitar la siguiente regla:
`"no-descending-specificity": null,`

# stylelint-order
Las guías de estilo de sass para un proyecto que ya tenemos funcionando pueden ser muy exigentes, en ese caso lo que hice fue desinstalarlas: ` npm un stylelint stylelint-config-sass-guidelines` e instalamos el paquete `npm install stylelint-order --save-dev` de modo de poder seguir ordenando alfabéticamente las propiedades.

```json
{
  "extends": "stylelint-config-standard",
  "plugins": [
		"stylelint-order"
	],
  "rules":{
    "no-descending-specificity": null,
    "order/properties-alphabetical-order": true 
  }
}
```

> Notar que la propiedad `extends` volvió a ser `"extends": "stylelint-config-standard"` agregamos `"plugins": [ "stylelint-order"]` y en `"rules"` agregamos `"order/properties-alphabetical-order": true`