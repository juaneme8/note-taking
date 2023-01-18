# Prettier

> :link: Basado en el video de [Web Dev Simplified](https://youtu.be/DqfQ4DPnRqI)

```bash
npm install --save-dev --save-exact prettier

```

Queremos que en el `package.json` nos quede fija la versión de modo que no corramos el riesgo de que ante una instalación futura utilicemos una versión posterior que pudiera introducir cambios no deseados.



## Formatear

Si queremos formatear el código de acuerdo a lo establecido por Prettier:

```
npx prettier --write file.js
```

Si queremos formatear todos los archivos:

```
npx prettier --write .
```



## Chequear formato 

Si queremos chequear que los archivos cumplan con lo establecido por Prettier:

```
npx prettier --check file.js
```

> Esto es interesante en caso de que estemos trabajando con pre-commit hooks o en un paso de CI.



Si queremos chequear todos los archivos:

```
npx prettier --check .
```

Como realizar esta tarea de manera manual podemos instalar la extensión **Prettier** para automatizarlo.

> Debemos tener presente que si no tuviéramos Prettier instalado como dependencia e instalamos la extensión, utilizaría una versión built-in pero no es lo aconsejable en proyectos con múltiples colaboradores ya que lo deseable será que todos usen la misma versión.

En la configuración veremos todas las opciones relacionadas con Prettier:

* Text Editor -> Editor: Default Formatter debemos elegir Prettier - Code formatter.
* En las configuraciones de la extensión podemos dejar las default.
* Configurar Format on Save



## Ignorar archivos

Para ignorar el formateo en determinados archivos, debemos crear un archivo `.prettierignore` y colocarlos dentro de el:

```
node_modules
.next
```



## Configurar Prettier

Cuando queremos que las configuraciones que hagamos en Prettier se mantengan para todos los colaboradores no bastará con modificar las Settings de la extensión. En ese caso podemos hacerlo de distintas formas.

### En `package.json`

```json
"prettier": {

}
```

### Con archivo `.prettierrc`

```json
{
  "semi": false
}
```

> En este caso por ejemplo estaremos configurando que no queremos puntos y comas, por lo que esto sobrescribirá lo que hayamos puesto en la configuración de VSCode.



### `overrides`

Si queremos tener un comportamiento distinto para otros archivos podemos utilizar overrides como veremos a continuación:

```json
{
  "semi": true,
  "overrides": [
    {
      "files": ".ts",
      "options": {
        "semi": false
      }
    }
  ]
}
```



## Coexistencia con ESLint

En caso de que tengamos ESLint instalado, como también se encarga del formateo del código, podremos deshabilitar dicha opción para evitar conflictos y utilizar Prettier directamente.

En primer lugar debemos instalar 

```
npm i eslint-config-prettier
```

Luego en `.eslintrc` (que asumimos que ya tenemos con algunas configuraciones)

```
{
  "extends":[
    "some-other-config-you-use", 
    "prettier"
  ],
  "rules":{
    "indent": "error"
  }
}
```



En este caso la regla de ESlint llamada `indent` tiene conflicto con Prettier, por lo que podremos detectarlo de modo que podremos comentarla:

```
npx eslint-config-prettier script.js
```



Crear script:

```
"format": "npx prettier --write ."
```

Lo ejecutamos con `npm run format`
