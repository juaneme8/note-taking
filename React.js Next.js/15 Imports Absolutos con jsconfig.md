# Imports absolutos con Next.js y CRA 
En ocasiones puede que queramos deshacernos de las rutas relativas de nuestros imports y nos resulte más conveniente trabajar con imports absolutos. Cuando trabajamos con **Next.js** o **create-react-app** esto puede llevarse a cabo creando un archivo `jsconfig.json` en el directorio raíz sin la necesidad de instalar ningún paquete extra. 

> Cuando estemos en un proyecto desde cero y utilizamos Webpack debemos configurarlo para que funcione.
>
> En caso de que utilicemos TypeScript esto lo haremos en `tsconfig.json`.

La configuración indicada en `jsconfig.json` se integra con VScode de modo que al importar el Intellisence nos sugerirá las rutas con ese criterio.

```json
{
  "compilerOptions": {
    "baseUrl": ".",
  }
}
```
Al haber establecido como `"baseUrl": "."` podremos importar de manera directa desde el `root` del proyecto. No será necesario hacer imports del estilo `../../services` sino directamente podremos hacerlos con `services`

> Nota: Es necesario reiniciar la aplicación para que los cambios sean tomados en cuenta.

Otra característica quizás de una utilidad menor consiste en agregar `paths` que nos permitan escribir alias para referirnos a ciertos archivos o carpetas:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths":{
	    "@c/": ["components/*"]
    }
  }
}
```
Esto significa que en lugar de referirnos a `components/` podremos poner `@c/` y además el Intellisence nos ayudará.