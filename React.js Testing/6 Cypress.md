# Cypress

Cypress es un testing framework con el cual podremos crear **tests de integración** y **tests end to end**.

Con ambos vamos a simular que somos un usuario y verificar que la interfaz funcione correctamente. La diferencia es que con los **tests end to end** vamos a asegurar que la aplicación (o una parte de ella) funcione como lo hará en un entorno de producción conectándose a todos los servicios que se conectaría como ser una base de datos (que podrá ser una de testing), pegándole a la API, entre otras cosas permitiéndonos ver cómo responden, en cuanto tiempo lo hacen etc. En los **tests de integración** no utilizamos estos servicios externos y tendremos librerías intermedias que se encargan de interceptar estos requests a APIs o bases de datos.



## Instalación

```
npm install cypress
```

> La instalación puede llevar varios minutos.



Luego de que tenemos Cypress instalado como dependencia tenemos dos formas de correrlo:

* de forma visual (particularmente útil para obtener un feedback visual cuando estamos creando los tests)
* de forma headless (para cuando quiero ejecutar los tests de una manera no visual por ejemplo si  tengo un CI/CD)



Para ejecutar los tests corremos

```
npx cypress open
```

También podemos crear un script `"test":"cypress open"` que luego podremos ejecutar con `npm test`



Veremos que se nos creo una carpeta `cypress` con una carpeta `integration` que en su interior contiene dos carpetas con tests de ejemplo.



## Creación de Test

Suponemos que queremos testear una TODO List. Lo primero que queremos hacer es testear la funcionalidad de agregado de elementos. Para ello en la carpeta `cypress/integration` creamos un un archivo `add.spec.js` (veremos que los tests tienen la terminación en `spec.js`).

En la parte superior agregamos:

```
/// <reference types="Cypress" />
```

De esta manera obtendremos ayuda en la medida que comenzamos a escribir los comandos.

Cypress está construido sobre Mocha por lo que utilizamos `describe` para agrupar los tests e `it` para crear los tests propiamente dichos. 

```js
describe("Add", () => {
	it("should add an element if all fields are completed", () => {
		cy.visit("http://localhost:3000");
	});
});
```



Notar que ahora en la ventana de Cypress nos aparecerá dentro de los Integration Tests uno llamado `add.spec.js` y si hacemos click en el lo ejecutará. A continuación veremos que el test pasará aunque no hemos hecho un expect o una assertion. Como la página devolvió un status code de 200 sabemos que ha cargado correctamente y Cypress interpreta que el test ha sido superado.



> **Selector Playground**
>
> En la interfaz de Cypress veremos un botón que dice **Open Selector Playground** que nos permite conocer los selectores de cada elemento haciéndole click. 



Si queremos ir mas lejos y también hacer click en un botón cuyo id es `add` podemos hacerlo con:

```js
describe("Add", () => {
	it("should add an element if all fields are completed", () => {
		cy.visit("http://localhost:3000");
		cy.get("#add").click();
	});
});
```

Veremos que en caso de poder hacerlo también superaremos el test.