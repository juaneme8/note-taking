# Reset CSS
En la página [meyerweb.com](https://meyerweb.com/eric/tools/css/reset/) nos encontramos con una serie de resets para los estilos del navegador (`margin`, `padding`, etc). Debemos pegar el css al comienzo de nuestra hoja de estilos o importarla también al comienzo.   

## Importancia para Navegadores Viejos
Si utilizamos una etiqueta HTML5 y esta no es reconocida por el navegador, lo tomará como un elemento de línea es por eso que en el reseteo lo que hago es tomar estos elementos semánticos y le asigno `display: block` asegurandonos que se vean bien en todos los navegadores.

## Reseteo en Codepen
Cuando utilizamos Codepen podemos agregar un reseteo con lo cual por ejemplo h1 pasaría a ser un texto pequeño y sin márgenes.  Para eso debemos ir a **Pen Settings** haciendo click en el engranaje y luego a **CSS Base: Reset**.

## Reseteo Framework
En caso de utilizar un Framework como Bootstrap, Materialize, etc no será necesario hacer un reset ya que ellos ya se encargan de colocar los estilos deseados como default.

# Normalize
`Normalize.css` nos ayuda a que todos los navegadores rendericen los elementos de manera más consistente ya que por default tienen algunas diferencias. De esta manera logramos que los sitios se vean igual independientemente del navegador utilizado.# Reseteo Manual
A continuación algunos de las propiedades que suelo resetear:

csscssmarginpaddingpar elmin e uraa de los iaratin oe

 e uraa de los lin

tedeotion noera elnar l ulet de las listaslslist-styl-tp non