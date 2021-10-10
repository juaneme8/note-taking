# Enlaces
Establecemos los enlaces utilizando el tag `<a>` que es un elemento de línea
## Enlaces Relativos:
Nos permite navegar a distintas secciones dentro de nuestro servidor
```html
<a href="section.html" title="vinculo a seccion">link text</a>
```

## Enlaces Absolutos:
Por default la apertura será dentro de la misma pestaña (`target:_self`) si queremos que se abra en una nueva pestaña podemos hacerlo con el atributo `target:_blank`.

```html
<a href="http://www.pagina.com/section" title="vinculo a seccion en pagina">link text</a>
```

Es importante especificar el atributo `title` por motivos de accesibilidad. También al pasar el mouse por arriba del enlace veremos este texto (tooltip).

* vínculo (relativo o absoluto)
* ancla: dentro de una misma página o también externos nos permiten acceder a una parte del sitio.
## Enlaces de Correo Electrónico
Estos tipos de enlaces nos permiten al hacer click abrir con el cliente de e-mail instalado en la comutadora como ser Outlook.
En la actualidad es muy cuestionado dado que facilita la dirección de correo para que sea encontrada por robotos exponiéndonos a ser agregados en listas de spam.
Lo aconsejable es crear formularios de contacto para obtener los datos del usuario y luego contactarlos. Sin embargo explicamos el funcionamiento a continuación:
```html
<a href = "mailto: abc@example.com">Send Email</a>
```
Es posible especificar el asunto y el cuerpo del correo:
```html
<a href = "mailto:abc@example.com?subject=Feedback&body=Message">
Send Feedback
</a>
```
## Enlaces de Ancla
Se trata de vínculos que hacen referencia a puntos específicos del propio documento o de una página externa.

Si quiero un enlace para ir a la parte superior de la página:
```html
<a href="#">Volver</a>
```
Si quiero tener un anclaje al punto `una-id` dentro del propio documento:
```html
<a href="#una-id">Ir al elemento con id="una-id" del propio documento</a>
```
Si quiero tener un anclaje a una página web externa debo especificar la URL y añadir al final una almohadilla (símbolo #) seguida del valor del atributo `name` o del atributo `id` del anclaje.
```html
<a href="http://sitio.com/pagina.html#una-id">Ir al elemento con id="una-id"</a>
```
```