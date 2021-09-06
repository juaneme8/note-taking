# React Hooks
> Basado en lista de reproducción de [Codevolution](https://www.youtube.com/playlist?list=PLC3y8-rFHvwisvxhZ135pogtX7_Oe3Q3A)
> El código está en el repositorio `md-hooks`

Los Hooks son una característica **incorporada a React versión 16.8** que nos permite contar con determinadas características sin tener que crear una clase (una de ella es la posibilidad de tener estado en un componente).

## Motivación
Existen distintas razones que motivaron la creación de Hooks:

* **Dificultad para reutilizar la lógica en distintos componentes**. Si bien el uso de **HOC** y **render props** son patrones que intentan resolver esto, nos obligan a restructurar los componentes (tenemos que wrapear los componentes con otros componentes) y el código termina siendo más difícil de seguir. Lo vamos a solucionar utilizando *custom hooks*. 

* Cada **método del ciclo de vida a menudo contiene una mezcla de lógica no relacionada entre sí**. Por ejemplo, los componentes pueden realizar alguna consulta de datos en el `componentDidMount` y `componentDidUpdate`. Sin embargo, el mismo método `componentDidMount` también puede contener lógica no relacionada que cree escuchadores de eventos, y los limpie en el `componentWillUnmount`. El código relacionado entre sí y que cambia a la vez es separado, pero el código que no tiene nada que ver termina combinado en un solo método. Esto hace que sea demasiado fácil introducir errores e inconsistencias. Lo vamos a solucionar con los distintos llamados a `useEffect`. 

* Dificultad en el **aprendizaje** de cómo funciona en JavaScript el `this` keyword y recordar agregar `bind` a tus manejadores de eventos. Por otra parte las clases no minifican muy bien, y hacen que la recarga en caliente sea confusa y poco fiable.

## Características
El uso de Hooks es opcional y las aplicaciones pueden contar con un mix de componentes de clase y componentes funcionales con Hooks. El equipo de React afirma no tener intenciones de eliminar las clases.