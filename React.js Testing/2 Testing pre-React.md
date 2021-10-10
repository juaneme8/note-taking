# Unit Tests
## Automated testing
Es la práctica de escribir **código para testear nuestro código** y luego ejecutar esos tests de manera automática.
En una aplicación si quisiéramos testear manualmente todas las funciones esto nos demandaría un **tiempo** muy grande, ya que para cada una de ellas sería necesario ejecutar la aplicación, loguearnos, navegar hasta cierto menú, llenar formularios, submit y ver el resultado en pantalla repitiendo esto para las distintas ramificaciones conocidas.
Con automated testing directamente invocamos a la función con diferentes **inputs** y verificamos que entregue el **output** esperado. Podremos ejecutar estos testeos cada vez que cambiamos el código, cada vez que hacemos un commit y **antes de hacer el deploy** de la aplicación (reduciendo así la cantidad de bugs en producción).
Otro beneficio es que nos permitirá hacer **refactoring** del código sin olvidarnos de probar nada luego de hacerlo.

## Tipos de tests:
* **Unit tests:** testeos de una unidad de la aplicación (puede ser una clase o varias) **sin** sus dependencias externas. Tienen la ventaja de ser fáciles de escribir y que se ejecutan rápido permitiéndonos verificar los *building blocks* de la aplicación. Sin embargo como desventaja no aportan mucha confianza al no estar usando las dependencias externas. 

* **Integration tests:** testeos de la aplicación **con** sus dependencias externas. Tardan más en ejecutar (normalmente involucran leer y escribir db) pero aportan una mayor confianza.

* **End-to-end:** testeos utilizando la UI de la aplicación, existen herramientas como selenium que nos permite grabar la interacción de un usuario con la aplicación y luego reproducir las mismas acciones para ver si obtenemos el resultado correcto. Estos tests aportan la mayor confianza pero son muy lentos y determinados mejoras en la aplicación o cambios mínimos en la UI pueden romper estos tests.

En nuestras aplicaciones debemos escribir los 3 tipos de tests esto se conoce como **pirámide de tests** y hace referencia a que la base deben ser los unit tests con mayor cantidad (rápidos y fáciles de escribir), también varios **integration tests** y unos pocos **end-to-end tests** (E2E) para las funciones centales de la aplicación, los casos de borde los atendemos con unit tests. Sin embargo en la práctica la cantidad de tests de cada tipo dependerá de la aplicación siendo recomendado usar los unit tests para testear las funciones con lógica compleja (y varios *paths* posibles) de manera rápida, los integration tests en aquellos casos de lectura/escritura de db.
Como criterio siempre que podamos hacer unit tests por sobre e2e debemos hacerlo.