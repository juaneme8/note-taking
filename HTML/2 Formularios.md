# `<form>`
Los formularios le permiten obtener información al servidor. 
Los campos de un formulario son elementos de línea-bloque.
## `method`
`<form method="get"`
El atributo `method` especifica cómo enviar la información del formulario a la página especificada por el atributo `action`. Esta información puede ser enviada como URL `method="get"` o mediante una transacción http post con `method="post"`.

En cuanto a `method="get"` podemos decir lo siguiente:
* Coloca la información del formulario en pares *name/value* en la url.
* El largo está limitado en aproximadamente 3000 caracteres.
* No usar para datos sensibles ya que serán visibles en la url.
* Suele ser usado en buscadores

En cuanto a `method="post"` podemos decir lo siguiente:
* Coloca la información dentro del *body* de un http request.
* No tiene limitaciones de tamaño.

## `enctype`
El atributo `enctype` especifica como deben codificarse los datos al enviarlos al servidor, sólo puede ser utilizado si `method="post"`

Si queremos tener un formulario que permita adjuntar archivos: 
`<form enctype="multipart/form-data">`

## `<input>`

### `"type="button"`
Se trata de un botón que activa JavaScript cuando es clickeado.
```html
<input type="button"  value="Click me"  onclick="msg()">
```
También es posible definir un botón clickeable con el elemento `<button type="button">Click Me</button>` pero a diferencia de los obtenidos con `<input>` con estos podremos colocar en el texto interior tags como `<i>`, `<strong>`, `<br>`, `<img>`, etc.

### `"type="checkbox"`
Los *checkboxes* o casillas de verificación nos permiten que un usuario elija una o más opciones.

```html
<input type="checkbox"  id="vehicle1"  name="vehicle1"  value="Bike">  
<label for="vehicle1"> I have a bike</label><br>  
<input type="checkbox"  id="vehicle2"  name="vehicle2"  value="Car">  
<label for="vehicle2"> I have a car</label><br>  
<input type="checkbox"  id="vehicle3"  name="vehicle3"  value="Boat">  
<label for="vehicle3"> I have a boat</label><br>
```

### `"type="color"`
### `"type="date"`
### `"type="datetime-local"`
### `"type="email"`
```html
<label for="correo">Correo Electrónico:</label>  
<input type="email" id="correo" name="correo">
```
Este input será automáticamente validado para asegurarnos que la dirección ingresada esté correctamente formateada. En caso de permitir más de un correo electrónico agregar `multiple`. Podemos colocar el atributo `placeholder`  a modo de *hint* del valor esperado que desaparecerá cuando el usuario haga click en el campo.

### `"type="file"`
```html
<label for="myfile">Elija el Archivo:</label>  
<input type="file"  id="myfile"  name="myfile">
```
Con el atributo `multiple` podremos elegir más de un archivo.

### `"type="hidden"`

### `"type="image"`
Nos permite definir una imagen como submit button
```html
<input type="image"  src="img_submit.gif"  alt="Submit"  width="48"  height="48">
```

### `"type="month"`

### `"type="number"`
```html
<label for="quantity">Cantidad (entre 1 y 5):</label>  
<input type="number"  id="quantity"  name="quantity"  min="1"  max="5">
```
Si sólo permitimos el ingreso de números pares podemos poner el atributo `step=2`

### `"type="password"`
Con el atributo `maxlength` indicamos la cantidad máxima de caracteres. 

### `"type="radio"`
```html
<input type="radio"  id="male"  name="gender"  value="male">  
<label for="male">Male</label><br>  
<input type="radio"  id="female"  name="gender"  value="female">  
<label for="female">Female</label><br>  
<input type="radio"  id="other"  name="gender"  value="other">  
<label for="other">Other</label>
```
Los *radio buttons* permiten que el usuario elija una única opción de un número de opciones posibles. Los *radio buttons* por lo general se los presentan en *radio groups* y esos se los identifica porque tienen el mismo *name*.
Si queremos tener un botón seleccionado por default debemos usar el atributo `checked`

 Cuando el atributo booleano `required` está presente en cualquiera de los botones radio de un grupo particular, el navegador no permitirá en envío del formulario hasta que uno de los botones en el grupo haya sido seleccionado.

### `"type="range"`
### `"type="reset"`
```html
<input type="reset" value="Borrar Contenido">
```
El botón de reset nos permitirá resetear los valores del formulario a los valores iniciales, es algo que no es recomendado pues pueden ocurrir opresiones accidentales.

### `"type="search"`

### `"type="submit"`
```html
<input type="submit" value="Send Request">
```
Al hacer click evaluará qué debe hacer de acuerdo al valor del atributo `action` en el `<form>`

### `"type="tel"`
En el atributo `pattern` ingresaremos una expresión con la cual validaremos el contenido del campo.
```html
<label for="phone">Enter your phone number:</label>  
<input type="tel"  id="phone"  name="phone"  pattern="[0-9]{3}-[0-9]{2}-[0-9]{3}">
```
Con este patrón estamos indicando que esperamos un número del estilo `000-00-000`

### `type="text"`
Nos permite definir un campo de texto de una sola línea:
```html
<label for="fname">First name:</label>  
<input type="text"  id="fname"  name="fname">
```

Si el usuario debe ingresar un patrón específico, podemos especificarlo con el atributo `pattern` como por ejemplo `<input type="text" name="countryCode" pattern="[A-Za-Z]{3}" title="Código de 3 letras">`

El `title` nos servirá como ayuda al usuario a modo de *hint*

### `type="time"`

### `type="url"`
```html
<label for="homepage">Página de Inicio:</label>  
<input type="url"  id="homepage"  name="homepage">
```
El valor ingresado será automáticamente validado antes de que el formulario pueda ser enviado.

### `type="week"`


#### `autofocus`
Si queremos que al abrir el formulario el cursor esté dentro de un `<input>` debemos colocar el atributo booleando `autofocus`

#### `label` y `aria-label`
El atributo `name` será usado para referenciar estos elementos en JavaScript o para referenciar los datos una vez que el formulario fue *submitted*. Solo los elementos  con el atributo `name` serán enviados.

El input siempre debe ir acompañado de un `<label>` como buena práctica de accesibilidad.

Siempre es aconsejable colocar un label junto con los `input` pero en aquellos casos que el diseño no lo permita, colocar en el input el atributo `aria-label` para garantizar la mayor accesibilidad posible. Se trata de un atributo diseñado para ayudar a la tecnología de asistencia (como ser *screen readers*).
Este atributo es recomendable para los botones (por ejemplo un botón "+" con `aria-label="Agregar Elemento Nuevo"`.

#### Inputs con emmet
Si queremos obtener un `<input type="url">` podemos poner `input:url + TAB`

## `<textarea>`
Nos permite definir un campo de texto multilínea.
```html
<label for="w3review">Review de producto</label>  

<textarea id="w3review"  name="w3review"  rows="4"  cols="50">  
El producto entregado cumple con las prestaciones esperadas.
</textarea>
```
El tamaño del `<textarea>` puede especificarse tanto con `rows` y `cols` como usando css (`width` y `height`)

## `<select>`
Es posible crear *drop-down lists* con el elemento `<select>`
```html
<label for="cars">Choose a car:</label>  
  
<select name="cars"  id="cars">  
	<option value="volvo">Volvo</option>  
	<option value="saab">Saab</option>  
	<option value="mercedes">Mercedes</option>  
	<option value="audi">Audi</option>  
</select>
```
Si quiero que un `<option>` aparezca seleccionado inicialmente como se trata de un **atributo booleano** puedo ponerle `selected`,`selected=""` o bien `selected="selected"`.

Utilizando el atributo booleano `multiple` es posible seleccionar más de una opción a la vez, pero como esto varía según el sistema operativo (por ejemplo con Windows debemos presionar control) y también teniendo en cuenta que deberíamos avisarle al usuario que la selección múltiple está disponible es que se aconseja utilizar en cambio *checkboxes*.

## `<datalist>`
Con el elemento `<datalist>` especificamos una lista de opciones pre definidas para un elemento `<input>`. De esta manera dotamos al `<input>` de un *autocomplete* a modo de *drop down*. El `id` del `<datalist>` debe coincidir con el atributo `list` del `<input>`.
Notar que son sugerencias, pero el usuario puede poner otro valor si así lo desea. 

```html
<label for="browser">Choose your browser from the list:</label>  
<input list="browsers"  name="browser"  id="browser">  
  
<datalist id="browsers">  
	<option value="Edge">  
	<option value="Firefox">  
	<option value="Chrome">  
	<option value="Opera">  
	<option value="Safari">  
</datalist>
```

## Validación de Formularios
Html5 ofrece la posibilidad de validar los datos de los campos sin la necesidad de utilizar JavaScript. La validación nos obliga a tanto respetar el tipo de dato de acuerdo al `<input type="">` (mail, url, patrón, número, etc) como así también a completarlos en caso de que sean `required`.
La validación se produce al presionar el botón de Submit mientras que en JavaScript podríamos hacerla en la medida que vamos completando el formulario.