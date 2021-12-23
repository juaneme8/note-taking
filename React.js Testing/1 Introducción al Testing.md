VIDEO 11 COMPLETO
# Introducción al Testing
Suponiendo que estamos desarrollando una *TODO APP* queremos que al hacer click en un campo Input nos permita ingresar el texto de una tarea, al presionar el botón **Add** nos permita agregarla a la lista y limpie automáticamente el input, luego que el contador de tareas pendientes aumente, etc. Si bien todas estas tareas podríamos testearlas manualmente gracias a los mecanismos de testing podremos realizarlas de manera automática. 

## Tipos de Tests
* **Unit Tests**: nos permiten testear un componente o porción de código de manera aislada.
> Por ejemplo podríamos testear el contador de tareas pendientes, que recibe un número por prop y muestra "n tareas pendientes".
> Podemos implementarlos con React Testing Library (si queremos testear componentes) o con Jest (si queremos testear funciones de utilidades).

* **Integration Tests**: nos permiten testear la interacción entre componentes (es decir entre dos o más unidades).
> Por ejemplo podríamos testear que cuando se ingresa un mensaje en el componente Input y se presiona el botón, que se agregue el contenido en el componente Lista.
> Podemos implementarlos con Cypress o React Testing Library

* **End to End Tests (E2E)**: nos permiten simular todo el proceso que llevará a cabo el usuario, desde el login, carga de nueva tarea, agregado a la lista, etc.

> Podemos implementarlos con Cypress.js, Selenium o Pupeteer.