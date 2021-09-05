# React.js
> Basado en lista de reproducción de ReactJS y React Hooks de [Codevolution](https://www.youtube.com/watch?v=QFaFIcGhPoM&list=PLC3y8-rFHvwgg3vaYJgHGnModB54rxOk3&ab_channel=Codevolution)
> El código estará en la carpeta md-react/hello-world y ahí adentro iremos creando los componentes que vayamos necesitando.

React es una *library* open source de JavaScript para hacer interfaces de usuario.

React fue creado y es mantenido por **Facebook**. Es utilizado por **Netflix** e **Instagram** entre otros.

React tiene una arquitectura basada en **componentes** que son *building blocks* **reutilizables y encapsulados** que usaremos para lograr interfaces de usuario complejas. Por ejemplo un sitio tradicional podemos pensarlo en términos de los siguientes componentes `header`, `sidenav`, `main content`, `footer`.  Decimos que son reutilizables ya que podemos invocarlos con diferentes atributos para que muestren información distinta.

React se basa en el **paradigma declarativo**, lo cual significa que tenemos que indicarle qué es lo que queremos y React con su **React DOM library** construirá el UI. Podemos decir que el **paradigma declarativo** es abstracto si lo comparamos con el **paradigma imperativo** que implementa algoritmos en pasos explícitos. 

React se encargará de manera eficiente de **renderizar y actualizar los componente cuando los datos cambian**. React se encarga de la costosa tarea de actualizar el DOM de manera elegante.

Gracias al **virtual DOM** React logra actualizar el DOM de manera muy rápida cuando se produce un cambio en las props o el estado de un componente. Es decir que no será necesario manipular el DOM manualmente (usando DOM API). Cuando iniciamos la aplicación por primera vez React crea el virtual DOM basado en todos los componentes y luego renderiza el DOM. Cuando realizamos un cambio React crea un nuevo virtual DOM y lo compara con el viejo. De esta manera podrá actualizar sólo aquella parte que sufrió cambios.

El nombre React podemos asociarlos con **REACTS to STATE changes** ya que React se encarga de que state y view están en sincronía. 

Es posible integrar React con cualquier aplicación existente, pudiendo tener una parte o la aplicación entera en React.

# Primeros Pasos
Antes de empezar lo primero es tener instalado Node.js

## `create-react-app`
Es un paquete creado y matenido por Facebook que nos permite crear una aplicación React sin realizar ninguna configuración inicial.

CRA nos aporta una configuración robusta con webpack y babel, además nos permitirá:
* contar con un development server
* soporte para *ES6 features*
* mantener código modular y por ende de más fácil mantenimiento
* *build tools* para lograr código optimizado.

Crear una carpeta para todos los proyectos de este tema`mkdir md-react` luego `cd md-react`.
Para crear la aplicación  `npx create-react-app hello-world` luego `cd hello-world` y para ejecutarla `npm start`

Si ya tenemos la carpeta creada (en este caso `hello-world`) podemos ejecutar `npx create-react-app .` para que utilice esa misma como base sin tener que borrarla y dirigirnos al directorio padre.

> Cuando creamos un proyecto con create-react-app automáticamente nos inicializa un repositorio git por lo que veremos una carpeta `.git`. En ocasiones puede que estemos trabajando con distintas "carpetas" cada una de las cuales es un proyecto de CRA y sólo queramos tener un repositorio git con todos ellos (por ejemplo si estamos haciendo el curso fullstack de open. En ese caso lo eliminamos con `rm -rf .git`

 **npm** es un administrador de paquetes de Node.js y **npx** es un ejecutable de npm (*npm package runner*) que nos permite en lugar de instalar un paquete ejecutarlo directamente (lo descarga en una carpeta temporal y lo ejecuta). 
 Con esto evitamos tener que instalar de manera global el paquete `npm install create-react-app -g` y  luego `create-react-app <project_name>` que tiene las desventajas de que requiere una instalación global que pide permisos de administrador y también mantener actualizado al paquete.

**yarn** es la alternativa a **npm** desarrollado por Facebook, pero es también un gestor de paquetes.

## Estructura de Carpetas
`package.json` nombre del paquete, configuración del linter, scripts, paquetes de los cuales depende nuestra aplicación.
`package-lock.json` versiones de los paquetes instalados.
`.gitignore` para evitar commitear determinados archivos.
`README.md`
`node_modules`: dependencias que necesita la aplicación (3rd party dependencies)

`public` carpeta que contiene los archivos estáticos que serán servidos al navegador
`/public/index.html` veremos un `<div id="root"></div>` React en tiempo real controla este `div` de modo de modificar la UI. Al ejecutar `npm start` este archivo es servido en navegador.

`/src` es donde crearemos los componentes
`/src/index.js` es el punto de entrada de la aplicación, es donde especificamos que el *root component* será `<App>` y que el elemento del DOM que será controlado por React es el div con `id="root"`
`ReactDOM.render(<App />,document.getElementById('root'));`

`App.js` es el componente que se está renderizando. Notar que el nombre del componente es en mayúscula con lo cual evitamos que se interprete como una etiqueta html.

`reportWebVitals.js` para obtener los web vitals que son métricas relacionadas con la performance.

`setupTests.js`

 `.eslintcache` relacionado con el linter que viene instalado.

## Limpieza
1. En `src` eliminamos:
* `App.test.js`
* `logo.svg`
* `reportWebVitals.js`
* `setupTests.js`

2. A `App.js` lo dejamos de este modo:
```jsx
import './App.css';

function App() {
  return (
    <div className="App">
      
    </div>
  );
}

export default App;
```
3. En `index.js` eliminamos el `<React.StrictMode>`,  `import reportWebVitals from './reportWebVitals';` y `reportWebVitals();` de modo que nos queda>

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));

```

# Debug con React Dev Tools
A la hora de debuggear contamos con una extensión de Chrome llamada `React Developer Tools` que nos permite monitorear los datos de los distintos componentes. Veremos un ícono con el logo de React en la parte superior derecha y se pondrá en rojo cuando el sitio esté usando una versión de desarrollo o en azul cuando esté usando una versión de producción.
En las dev tools tendremos una nueva pestaña React donde veremos los componentes junto con sus props y state.

# Single Page Application (SPA)
La mayoría de las aplicaciones con React son SPA, en ellas sólo habrá un `.html` que será servido del servidor al navegador y el contenido se actualizará dinámicamente. 
En el primer request a `/index` el server nos entrega el archivo `index.html` y en un futuro request a `/contact` por ejemplo, React intercepta este request antes de que llegue al server y cargará el componente. De esta manera logramos una respuesta más rápida.