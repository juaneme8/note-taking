# ESLint con Prettier
Basado en varios artículos y ideos se armó un compilado de lo aprendido.
> Código en `md-eslint-prettier` que es un proyecto utilizando Next.js
> [Video](https://www.youtube.com/watch?v=EEDRcolSHms) de midudev
> [Artículo](https://blog.theodo.com/2019/08/empower-your-dev-environment-with-eslint-prettier-and-editorconfig-with-no-conflicts/)
> [Video](https://www.twitch.tv/videos/977960819) Gonzy Pozzo
> [Video](https://youtu.be/O4ZIJgOWj_A)

## ¿Qué es ESLint?
ESLint es un linter, es decir una herramienta encargada de analizar el código en **búsqueda de problemas**. Tanto en cuestiones de **formateo** (longitud de línea máxima, uso de comillas simples, tipo de indentación, puntos y comas, etc) como en términos de **inconsistencia del código** (detectar si hay variables sin usar o variables globales).
Es una **herramienta basada en reglas** y en función de ellas serán los problemas que encontrará.
Además de encontrar problemas tiene la capacidad de solucionarlos por ejemplo si tenemos una variable implementada con `let` y no la hemos modificado en ninguna parte es posible configurar una regla para que sea cambiada a `const`. Otras posibles soluciones serían en cuanto a espaciados, cambiar comillas dobles por comillas simples, agregar puntos y coma, etc.

## ¿Qué es Prettier?
Así como dijimos que ESLint se preocupa tanto del formateo como de la búsqueda de posibles errores del código, Prettier es sólo un formateador de código y como tal estará preocupado por temas como espaciados, comillas simples o dobles, uso de puntos y comas, etc. La idea detrás de Prettier es evitar debates acerca de cómo debe ser el estilo del código y si bien son posibles las personalizaciones la idea es no utilizarlas y que sea automático el formateo al guardar el código.

# Instalación ESLint y Prettier
`npm install eslint prettier --save-dev`

## Inicialización ESLint
Para inicializar la configuración ejecutamos `npx eslint --init` e ingresamos a un wizard donde nos hará una serie de preguntas. 

> Al utilizar `npx` irá a buscar el ejecutable a `/node_modules/bin/` si lo que le pasamos no está instalado lo que hará será bajarla de npm y ejecutarla pero sin agregarlo a las dependencias del proyecto. Como nosotros ya lo hemos isntalado podríamos hacer `./node_modules/bin/eslint --init`

1. Elegimos la opción **To check syntax, find problems, and enforce code style**
2. Elegimos **JavaScript modules** (import/export)
3. Elegimos **React** como framework con el que vamos a trabajar.
4. Indicamos que **no utilizamos TypeScript**
5. Especificamos que el código corre en Browser (podríamos poner node también presionando la letra `a` para seleccionar ambas opciones)
6. Luego elegimos una **guía de estilos populares** y luego `standard` (es un preset que trabaja sin puntos y comas por ejemplo).
7. Luego indicamos que queremos la **configuración en JSON**.
8. Nos dirá las dependencias que debe utilizar y nos preguntará si puede hacerlo con `npm`.
Las dependencias que instalará son las siguientes: 
`
eslint-plugin-react@latest
eslint-config-standard@latest
eslint@^7.12.1
eslint-plugin-import@^2.22.1
eslint-plugin-node@^11.1.0
eslint-plugin-promise@^4.2.1
`

> Si le indicáramos que vamos a trabajar con TypeScript nos instalaría esas mismas y además: `@typescript-eslint/parser@latest` y `@typescript-eslint/eslintplugin@latest`



## Configuración

Veremos que nos ha creado el archivo `.eslintrc.json` con el siguiente contenido:
```json
{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "plugin:react/recommended",
        "standard"
    ],
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": 12,
        "sourceType": "module"
    },
    "plugins": [
        "react"
    ],
    "rules": {
    }
}
```

## Extensión ESLint
Para tener la integración con el linter y ver los subrayados indicando los errores debemos instalar la extensión `ESLint`.

> Si además queremos tener una indicación del error en la línea en que sucede debemos instalar la extensión `Error Lens`./

Luego de instalarla nos aparecerá en el panel de Problems: *ESLint is disabled since its execution has not been approved or denied yet. Use the light bulb menu to open the approval dialog*. Hacemos click en la lámpara y luego en **Manage Extension Execution** y ponemos **Allow Everywhere**.

En este momento ya nos aparecerán marcados los errores de acuerdo a la guía de **standard**

## ESLint desde CLI
Con `npx eslint .` podemos ver un listado de los problemas que tenemos. Nuevamente podemos hacerlo también con `./node_modules/bin/eslint .`

Con `npx eslint . --fix` los podemos solucionar los problemas que sean solucionables.

## ESLint con Script
> Si quisiéramos evitar tener que hacer esto cada vez que queremos arreglar los problemas, en `package.json` agregamos un script `"lint": "eslint ."` y otro `"lint:fix": "eslint . --fix"`. Debiendo ahora ejecutar `npm run lint` o `npm run lint:fix` según queramos.
> Notar que como los scripts del `package.json` tienen acceso al PATH no es necesario poner `/node_modules/bin/`

## ESLint Fix al guardar
Las reglas de standardjs establecen que debemos usar *single quotes* y *2 spaces indentation*, si queremos que en caso de haber violado esta regla al guardar se corrija automáticamente, en  `settings.json` debemos poner:

```json
"editor.codeActionsOnSave": {
	"source.fixAll.eslint": true
 },
```

Esto podemos ponerlo en una carpeta `.vscode` y dentro en un archivo `settings.json` de modo tal que lo subamos al repositorio remoto y sea utilizado por todos los usuarios. Además podemos agregarle `"editor.formatOnSave": false,` para evitar conflictos con formateadores (nosotros vamos a querer que Prettier sea una regla de ESLint). Nos quedará de esta forma:

```json
{
    "editor.formatOnSave": false,
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
     },
}
```
## ESLint Rules
[Basado en Repositorio Goncy](https://github.com/goncy/generator) en [stream](https://www.twitch.tv/videos/977960819)

```json
"env": {
	"browser": false,
	"es2021": true,
	"node": true
},
```
Con `node: true` podemos agregar variables de entorno (con `process.env`) .

* Si nos aparece el mensaje **'React' must be in scope when using JSX  react/react-in-jsx-scope** debido a que actualmente no es necesario importar React podemos deshabilitar esta regla del siguiente modo:
``rules: { 'react/react-in-jsx-scope' : 'off'}``

* Si queremos desactivar la regla que nos marca error **'...' is missing in props validation** por no tener definidos los tipos de las props con PropTypes:  [Artículo de Typechecking with PropTypes](https://reactjs.org/docs/typechecking-with-proptypes.html)
`"react/prop-types": "off"`

* Si queremos obtener un warning cuando tengamos un `console.log()` podemos agregar esta regla:
`"no-console": "warn"`

* Si queremos que no nos muestre errores ante variables no usadas y sí un warning:
`"no-unused-vars": "warn",`

* En cuanto al ordenamiento de los imports, podemos encontrar más información en este [este artículo](https://dev.to/otamnitram/sorting-your-imports-correctly-in-react-213m):
Si bien ESLint viene con [built-in import order rules](https://eslint.org/docs/rules/sort-imports) utilizaremos las reglas de `eslint-plugin-import` (que se nos instaló automáticamente luego del `npx eslint --init`)
```json
"import/order": ["error", 
      { "groups": ["builtin", "external", "internal"],
        "pathGroups": [
          {
            "pattern": "react",
            "group": "external",
            "position": "before"
          }
        ],
        "pathGroupsExcludedImportTypes": ["react"],
        "newlines-between": "always", 
        "alphabetize": {
      "order": "asc",
      "caseInsensitive":true
    }}],
```

Por un lado queremos que el import de React se encuentre en la parte superior , luego tener primero los imports externos y luego los internos y también tener ordenamiento alfabético. También queremos tener un grupo entre los imports de `../` y los de `./`

* Si queremos obtener tener extra tags para componentes sin `children` (es decir `<Hello name="John"></Hello>;`)
`"react/self-closing-comp": "warn",` 

* Si queremos ordenar las props alfabéticamente poniendo los callbacks a lo ultimo, los parámetros booleanos al principio:
```json
"react/jsx-sort-props": [
	"warn",
	{
		"callbacksLast": true,
		"shorthandFirst": true,
		"noSortAlphabetically": false,
		"reservedFirst": true
	}
],
```

* Si queremos un espacio previo al `return`, otro después de `const`, `let`, `var`, y  a su vez quiero que si tengo más de un `const` no me oblique a tener un espacio entre ellos.
```json
"padding-line-between-statements": [
"error",
{"blankLine": "always", "prev": "*", "next": "return"},
{"blankLine": "always", "prev": ["const", "let", "var"], "next": "*"},
{"blankLine": "any", "prev": ["const", "let", "var"], "next": ["const", "let", "var"]}
]
```

## Prettier desde CLI
Con `npx prettier . --check` nos dirá los errores que encuentra
Con `npx prettier . --write` los solucionamos.

## `.prettierignore`
Creamos también el archivo `.prettierignore` (`node_modules` la ignora por defecto), podremos agregar dentro `.next` para evitar que revise esa carpeta.

# Prettier como ESLint Rule
Para lograr ejecutar Prettier como una regla de ESLint debemos instalar el siguiente paquete: [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier) 
`npm install --save-dev eslint-plugin-prettier`

Este plugin funciona mejor si usamos la configuración recomendada para deshabilitar las reglas de ESLint relacionadas con formateo de código. Para ello es necesario también instalar `eslint-config-prettier`
`npm install --save-dev eslint-config-prettier`

Luego en el `.eslintrc.json` sólo será necesario colocar a lo último lo siguiente:
```json
{
  "extends": ["plugin:prettier/recommended"]
}
```

# Prettier Configuración
Si queremos cambiar alguna regla de las utilizadas por Prettier para el formateo, en lugar de usar un archivo de configuración de prettier lo que hacemos es agregar en `.eslintrc.json` en `rules` lo siguiente:
```json
"prettier/prettier": [
      "error",
      {
        "singleQuote":true,
        "trailingComma": "es5",
        "printWidth": 120,
        "endOfLine": "auto" 
      }
    ],
```

> El `"endOfLine": "auto" ` lo agregué para evitar errores que decían "Remove CR".

> El `"bracketSpacing": false` hace que pasemos de tener `import { makeStyles } from '@material-ui/core';` a tener `import {makeStyles} from '@material-ui/core';`

# Notas Finales

## React-Hooks
En `"extends"` agregamos `"plugin:react-hooks/recommended"` para asegurar el cumplimiento de las [reglas de los hooks](https://reactjs.org/docs/hooks-rules.html) Para ello `npm install eslint-plugin-react-hooks --save-dev`

Agregar ademas estas reglas:
```json
"react-hooks/rules-of-hooks": "warn",
"react-hooks/exhaustive-deps": "warn",
```
==duda de que haga falta==
En `"plugins"` agregamos `"import"` (para dps poder poner una regla de import order)


# Mi archivo `.eslintrc.json`

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": [
    "plugin:react/recommended",
    "standard",
    "plugin:prettier/recommended",
    "plugin:react-hooks/recommended"
  ],
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": [
    "react"
  ],
  "rules": {
    "prettier/prettier": [
      "error",
      {
        "singleQuote": true,
        "trailingComma": "es5",
        "printWidth": 120,
        "endOfLine": "auto"
      }
    ],
    "react/react-in-jsx-scope": "off",
    "react/prop-types": "off",
    "no-console": "warn",
    "no-unused-vars": "warn",
    "import/order": [
      "error",
      {
        "groups": [
          "builtin",
          "external",
          "internal",
          [
            "sibling",
            "parent"
          ]
        ],
        "pathGroups": [
          {
            "pattern": "react",
            "group": "external",
            "position": "before"
          }
        ],
        "pathGroupsExcludedImportTypes": [
          "react"
        ],
        "newlines-between": "always",
        "alphabetize": {
          "order": "asc",
          "caseInsensitive": true
        }
      }
    ],
    "react/self-closing-comp": "warn",
    "react/jsx-sort-props": [
      "warn",
      {
        "callbacksLast": true,
        "shorthandFirst": true,
        "noSortAlphabetically": false,
        "reservedFirst": true
      }
    ],
    "padding-line-between-statements": [
      "error",
      {
        "blankLine": "always",
        "prev": "*",
        "next": "return"
      },
      {
        "blankLine": "always",
        "prev": [
          "const",
          "let",
          "var"
        ],
        "next": "*"
      },
      {
        "blankLine": "any",
        "prev": [
          "const",
          "let",
          "var"
        ],
        "next": [
          "const",
          "let",
          "var"
        ]
      }
    ]
  }
}
```

# Trabajo en equipo
Commitear `eslintrc.json` y asegurarnos que todos tengan instalada la extensión de ESLint.

Además en las Workspace Settings (`settings.json` en una carpeta `.vscode` en la raíz) podemos poner lo siguiente:

```json
{
    "editor.formatOnSave": false,
      "editor.codeActionsOnSave": {
          "source.fixAll.eslint": true
      }
}
```

De esta manera nos aseguramos que si uno de los colaboradores tiene instalado Prettier y configurado como formateador no entre en conflicto con nuestra configuración del proyecto.

# ESLint con Node Express

## Implementación I

> Implementación basada en midudev pero [video del Bootcamp](https://www.youtube.com/watch?v=ep_plUeKV1Y)

Instalar el paquete `standard` como dependencia de desarrollo:

```
npm install standard -D
```



## Implementación II

> La verdadera guía es la que se hizo para React aca sólo se fueron pegando los comandos basándonos en ella y agregando alguna particularidad.

1) Instalar ESLint y Prettier
``npm install eslint prettier --save-dev``

2) Ejecutamos `npx eslint --init`
```bash
1. **To check syntax, find problems, and enforce code style**
2. **CommonJS** (require/exports)
3. Framework **None of these**
4. **No TypeScript**
5. Code runs **Browser & Node**
6. Use a popular style guide
7. Standard
8. Config file in JavaScript
9. Instalar con npm
```



Nos dirá que va a tener que instalar:
`
eslint-config-standard@latest
eslint@^7.12.1
eslint-plugin-import@^2.22.1
eslint-plugin-node@^11.1.0
eslint-plugin-promise@^4.2.1
`


3) Crear carpeta `.vscode` y en ella un archivo `settings.json` en el cual colocamos:
```json
{
	"editor.formatOnSave": false,
	"editor.codeActionsOnSave": {
		"source.fixAll.eslint": true
	 },

	 "liveSassCompile.settings.formats": [
		 {
			 "format": "expanded",
			 "extensionName": ".css",
			 "savePath": "~/../css/"
		 }
	 ],
}
```
La idea con `editor.formatOnSave` en `false` es que no entre en conflicto nuestra config con ESLint y Prettier como una regla de ESLint con un formateador que pueda tener el usuario. Por otra parte con `editor.codeActionsOnSave` le indicamos que queremos que al guardar haga el fix con ESLint. Por último con `liveSassCompile.settings.formats` indicamos que queremos que nos genere un `.css` a partir del `.scss` y donde lo queremos.

4) Instalamos paquete para correr Prettier como una regla de ESLint:
`npm install --save-dev eslint-plugin-prettier`

5) Instalamos también:
`npm install --save-dev eslint-config-prettier`

5) En `.eslintrc.js` agregamos a `extends` a lo último lo siguiente
```json
{
  "extends": ["plugin:prettier/recommended"]
}
```

6) En `.eslintrc.json` en `rules` agregar lo siguiente:
```json
"prettier/prettier": [
      "error",
      {
        "singleQuote":true,
        "trailingComma": "es5",
        "printWidth": 120,
        "endOfLine": "auto" 
      }
    ],
```

7) Para evitar el mensaje **'$' is not defined** en `"env"` agregar:
```json
"jquery": true
```

8) Para evitar el mensaje **'moment' is not defined**, **Chart is not defined** y **QRCode is not defined** agregar en `.eslintrc.js`

```json
 "globals": {
    moment: true,
    Chart: true,
    QRCode: true,
  }
```

9) Para evitar mensaje **Identifier is not in camel case** (esto me vi obligado a hacerlo porque ya tenía muchas variables con guion bajo) en las rules agregamos:
`camelcase: 'off',`

10) Si queremos evitar que nos muestre errores o warnings en librerías de terceros podemos agregar `ignorePatterns: ['moment.min.js', 'qr-generator.js'],`

# ESLint con Next.js y TypeScript

1) Instalar ESLint y Prettier
`npm install eslint prettier --save-dev`

2) Ejecutamos `npx eslint --init`
1. **To check syntax, find problems, and enforce code style**
2. **JavaScript modules (import/export)**
3. **React**
4. **TypeScript**
5. **Browser & Node**
6. Use a popular style guide
7. Standard
8. Config file in JavaScript
9. Instalar con npm

10. Nos dirá que va a tener que instalar las siguiente dependencias con npm:
`
eslint-plugin-react@latest
@typescript-eslint/eslint-plugin@latest
eslint-config-standard@latest
eslint@^7.12.1
eslint-plugin-import@^2.22.1
eslint-plugin-node@^11.1.0
eslint-plugin-promise@^4.2.1
@typescript-eslint/parser@latest 
`

Nos creará un archivo `.eslintrc.js` con la siguiente información:
```json
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true
  },
  extends: [
    'plugin:react/recommended',
    'standard'
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true
    },
    ecmaVersion: 12,
    sourceType: 'module'
  },
  plugins: [
    'react',
    '@typescript-eslint'
  ],
  rules: {
  }
}
```

11. Agregamos en `extends` a lo último: `'plugin:@typescript-eslint/recommended'`

> En el archivo `.eslintrc.js` podemos notar comparando con la versión sin TypeScript el agregado de `"parser": "@typescript-eslint/parser"`


12. Crear carpeta `.vscode` y dentro en un archivo `settings.json` :

```json
{
    "editor.formatOnSave": false,
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
     },
}
```

13. Si ejecutamos `npx eslint .` y obtenemos **React version not specified in eslint-plugin-react settings.**, agregamos en `eslintrc.js`

```json
 settings: {
    react: {
      version: 'detect',
    },
  },
```

14. Instalamos paquete para correr Prettier como una regla de ESLint:
`npm install --save-dev eslint-plugin-prettier`

15. Instalamos también:
`npm install --save-dev eslint-config-prettier`

16. En `.eslintrc.js` agregamos a `extends` a lo último lo siguiente
`'plugin:prettier/recommended'`

Por lo que nos queda:
 extends: [
    "plugin:react/recommended",
    "standard",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended",
  ],

17. En `.eslintrc.json` pegar el objeto `rules` que generamos en la configuración sin TypeScript.
```json
"rules": {
    "prettier/prettier": [
      "error",
      {
        "singleQuote": true,
        "trailingComma": "es5",
        "printWidth": 120,
        "endOfLine": "auto"
      }
    ],
    "react/react-in-jsx-scope": "off",
    "react/prop-types": "off",
    "no-console": "warn",
    "no-unused-vars": "warn",
    "import/order": [
      "error",
      {
        "groups": [
          "builtin",
          "external",
          "internal",
          [
            "sibling",
            "parent"
          ]
        ],
        "pathGroups": [
          {
            "pattern": "react",
            "group": "external",
            "position": "before"
          }
        ],
        "pathGroupsExcludedImportTypes": [
          "react"
        ],
        "newlines-between": "always",
        "alphabetize": {
          "order": "asc",
          "caseInsensitive": true
        }
      }
    ],
    "react/self-closing-comp": "warn",
    "react/jsx-sort-props": [
      "warn",
      {
        "callbacksLast": true,
        "shorthandFirst": true,
        "noSortAlphabetically": false,
        "reservedFirst": true
      }
    ],
    "padding-line-between-statements": [
      "error",
      {
        "blankLine": "always",
        "prev": "*",
        "next": "return"
      },
      {
        "blankLine": "always",
        "prev": [
          "const",
          "let",
          "var"
        ],
        "next": "*"
      },
      {
        "blankLine": "any",
        "prev": [
          "const",
          "let",
          "var"
        ],
        "next": [
          "const",
          "let",
          "var"
        ]
      }
    ]
  }
```

18. En `"extends"` agregamos `'plugin:react-hooks/recommended'` para asegurar el cumplimiento de las [reglas de los hooks](https://reactjs.org/docs/hooks-rules.html)
 `npm install eslint-plugin-react-hooks --save-dev`

Agregamos estas reglas relacionadas con hooks
```json
'react-hooks/rules-of-hooks': 'warn',
'react-hooks/exhaustive-deps': 'warn',
```

19. Para evitar el error **'React' was used before it was defined.** agregamos la siguiente regla:

```json
"no-use-before-define": "off",
"@typescript-eslint/no-use-before-define": ["error"]
```

