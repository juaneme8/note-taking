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



**Importante**

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



> **Hover en comandos**
>
> Pasando el mouse por encima de cada comando podremos ver en qué estado estaba la aplicación cuando ese comando se ejecuctó.



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



## `whithin`

`whithin` nos permite establecer el scope de los comandos `cy` subsiguientes.

Por ejemplo si tenemos un formulario y queremos referenciar elementos que este tenga dentro (esto sería útil si por ejemplo tenemos dos inputs con el mismo nombre), podríamos hacerlo de este modo:

```js
cy.get("#form1").whithin((form)=> {
	cy.get("[name='gift']").type("Medias")
	cy.get("[name='owner']").type("Goncy")
	cy.get("[name='image']").type("//placehold.it/64x64")
	cy.get("[name='count']").type("4")

	//Para enviar el formulario tenemos dos opciones:
    
    //1) Con submit (notar que uso el form devuelto por whitin())
	cy.get(form).submit();
    
    //2) Click botón:
    cy.get("button").click();
})
```



Como estamos trabajando con un formulario tenemos la opción de hacer click en el botón Guardar o hacer el Submit del form.



> `data-testid`
>
> Cuando queremos referenciar un elemento para propósitos de tests es posible utilizar como atributo `data-testid`
>
> `cy.get("[data-testid='gift']")`



## `should`

`should` nos permite verificar determinada condición. 

Por ejemplo si hemos agregado un elemento a una lista y queremos verificar que sólo haya uno:

```js
cy.get("[data-testid='gift']").should("have.length",1)
```

Si tenemos una imagen y queremos verificar que tenga determinado atributo `src` podemos hacerlo con:

```js
cy.get("img").should("have.attr","src","//placehold.it/48x48")
```



Si tenemos un texto y queremos verificar el contenido:

```js
cy.get("[data-testid='title']").should("have.text","Buzz");
```


De la misma manera si tenemos un mensaje de error y queremos verificar que aparezca en pantalla:

```js
cy.get("[data-testid='error']").should("be.visible")
```

> Esto mismo podríamos utilizar cuando presionamos un botón y esperamos que aparezca un modal: `cy.get("#add").should("be.visible")`

> **Funciones**
>
> Cuando tenemos funcionalidades que se repiten en nuestros tests, es posible extraerlas a funciones utilitarias. Por ejemplo como en varios tests vamos a llenar el formulario podemos hacerlo mediante una función. 
>
> ```js
> function fillForm({gift,owner,image,count}){
> 	gift && cy.get("[name='gift']").type(gift)
> 	owner && cy.get("[name='owner']").type(owner)
> 	image && cy.get("[name='image']").type(image)
> 	count && cy.get("[name='count']").type(count)
> 
>  cy.get(form).submit();
> }
> ```
> Como no es posible tipear en Cypress un string vacío utilizamos la operación booleana AND para ejecutar el comando sólo si recibimos dicho parámetro. No podríamos utilizar simplemente un valor default `""` ya que estaría ejecutando el `type("")` y arrojaría error.
> Luego llamaremos a esta función por ejemplo con 
>
>```js
> fillForm({gift:"Medias",owner:"Goncy",image:"//placehold.it/64x64",count:4})
>```



Como tenemos ciertas líneas de código que se ejecutan antes de cada test, podemos hacer uso de `beforeEach()` e indicarlas allí dentro. Por ejemplo los comandos encargados de visitar la página y hacer click en el botón para agregar un elemento a la lista.

```
beforeEach(()=>{
	cy.visit("http://localhost:3000");
	cy.get("#add").click();
})
```



## `contains`

Utilizando`contains` obtenemos el elemento del DOM que tiene el texto indicado.

Por ejemplo si queremos hacer click en un botón que cierra el modal y tiene el texto Cerrar podemos hacerlo con:

```
cv.contains("Cerrar").Click()
```



Luego podríamos verificar que el modal ha desaparecido

```
cv.get("form").should("not.exist")
```

> Notar que no utilizamos `should("not.be.visible")` dado que eso sería válido si estuviéramos ocultando el elemento pero estuviera en el DOM, en nuestro caso no estará directamente.



## Fetch de API

Supongamos por un momento que la TODO List la cargo con elementos de Punk API (https://punkapi.com/) que devuelve un listado de cervezas cuando le pegamos al endpoint https://api.punkapi.com/v2/beers. Como estamos dependiendo de un servicio externo, podemos decir que **estamos realizando un test end-to-end**.

Luego creo un test en un archivo `gift.spec.js` para verificar que el agregado de los elementos haya sido correcto:

```js
describe("gift list, () => {
	beforeEach(()=> {
		cv.visit("http://localhost:3000");
	})
    
    it("should render the gift properly", ()=> {
        cy.get("[data-testid='gift']").first().whithin( () => {
        	cy.get("img").should("have.attr","src", "https://images.punkapi.com/v2/keg.png")
        cy.get("[data-testid='title']").should("have.text", "Buzz");
        cy.get("[data-testid='owner']").should("have.text", "goncy");  
        })  
    })
});
```



Sin embargo si lo que queremos es testear nuestra aplicación mediante un **test de integración** con la confianza de que si cambia algo en la respuesta de la API el test seguirá funcionando, lo que tenemos que hacer es interceptar el request a la URL de esa API y entregar una respuesta personalizada.

```js
describe("gift list, () => {
	beforeEach(()=> {
		cv.visit("http://localhost:3000");
	})
    
    it("should render the gift properly", ()=> {
    	cy.intercept("GET", "/v2/beers", {
            statusCode: 200,
            body: [
                {
                    name: 'beer test', 
                    abv: 2,
                    beer_url: '//placehold.it/48x48'
                }
            ]
        })
        
        
        cy.get("[data-testid='gift']").first().whithin( () => {
        	cy.get("img").should("have.attr","src", "https://images.punkapi.com/v2/keg.png")
        cy.get("[data-testid='title']").should("have.text", "Buzz");
        cy.get("[data-testid='owner']").should("have.text", "goncy");  
        })  
    })
});
```

