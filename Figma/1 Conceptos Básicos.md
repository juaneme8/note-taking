# Conceptos Básicos
> Basado en la [playlist](https://www.youtube.com/watch?v=_c26Fyi7RFA&list=PLvq-jIkSeTUbpfewvbKLhHctdkgadAy-K&ab_channel=jonmircha) de youtube de jonmircha.
> Encontramos recursos adicionales en https://jonmircha.com/figma

1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23. Completos al 21/10.

# Introducción
Figma es una herramienta que nos permite diseñar interfaces de usuario permitiéndonos trabajar con UI o UX. 

* Se trata de una **web application** (aunque es posible instalarla). Esto representa una ventaja comparado con alternativas como AdobeXD que funciona sólo en Windows o Mac o  o [Sketch](https://www.sketch.com/) que sólo funciona en Mac.
* Permite la **colaboración en tiempo real** con lo cual un equipo de trabajo puede trabajar en distintos aspectos asegurándonos siempre estar actuando en la última versión. 
* Es posible crear **componentes** reutilizables.
* Nos permite simular la **interacción** (por eso decimos que es una herramienta de UI/UX).
* Nos permite **generar código** en formato CSS.
* Maneja un **historial de versiones**.
* Contamos con **Figma community** donde encontraremos plugins y archivos compartidos por usuarios.

# Recursos
## Figma Mirror
Figma Mirror una aplicación para celular que nos permite ver los diseños. Sólo tendremos que iniciar sesión y luego seleccionar un frame.

## Google Sheets Sync
Google Sheets Sync es un plugin que nos permite sincronizar datos de una hoja de cálculos de google en nuestro archivo Figma.
https://www.figma.com/community/plugin/735770583268406934/Google-Sheets-Sync


# Duplicar Layer
Con `ctrl+d` duplicamos la selección. De la misma manera que en Illustrator si a un layer lo ararstramos con `alt` presionado creará una copia. Si en ese momento presionamos `ctrl + d` creará una nueva copia y la ubicará a la misma distancia que había entre la primera y la segunda.

# Renombrar Layers
Si en el panel lateral izquierdo estamos renombrando un layer y presionamos `tab` pasaremos a renombrar al siguiente sin la necesidad de hacer doble click sobre su nombre.

# Copiar/Pegar Estilos
Si tenemos un elemento con ciertos estilos aplicados y queremos los mismos en otro elemento, simplemente hacemos `ctrl+alt+c` para copiarlos y luego `ctrl+alt+v` para pegarlos

# Incrementar Campo
Cuando seleccionamos un campo numérico como por ejemplo X, Y, W, H etc, veremos una flecha en ambas direcciones que al arrastrarla horizontalmente variará muy rápido, si queremos que lo haga más lento debemos bajar en dirección vertical y ahí sí podremos movernos horizontalmente obteniendo un paso más chico.

# Desplazar Selección
Si hacemos una selección rectangular, luego podremos moverla presionando la tecla `space`. En ocasiones puede convenirnos hacer una selección de aproximadamente un pixel y desplazarla de modo de tener una selección precisa.

# Performance
Si queremos obtener información sobre performance presionar `ctrl+}` y  buscar `resource used`

# Agrupar
Es posible agrupar con `ctrl+g` y luego presionando la tecla `enter` desde la parte superior del grupo podré ingresar a los layers internos. Para ir subiendo niveles de anidamiento puedo presionar `shift+enter`

# Zoom
`+` Acercar
`-` Alejar
`shift + 0`: Zoom  al 100%
`shift + 1`: Zoom to Fit
`shift + 2`: Zoom  a la Selección

# Opacidad
`1`: Opacidad 10% (previamente debemos seleccionar un elemento)
`01`: Opacidad 1%
`2`: Opacidad 20%
`55`: Opacidad 55%
`9`: Opacidad 90%
`0`: Opacidad 100%
`00`: Opacidad 0%


`shift + a`: Auto Layout
`ctrl + alt + k`: Componentes
`ctrl + d`: duplicar selección
`ctrl + g`: agrupar selección
`ctrl + shift + g`: desagrupar selección

# Herramientas
`R` herramienta rectángulo
`L` herramienta linea
`O` herramienta elipse
`shift + L` herramienta flecha
`H o space` Herramienta mano
`F` Herramienta de marco  (frame)
`S` Herramienta de corte
`V` Herramienta selección
`K` Herramienta escala
`R` Herramienta rectángulo
`T` Herramienta texto

Si queremos dibujar una elipse empezando desde el centro debemos presionar la tecla `alt` y si queremos que sea un círculo presionando además `shift`
Al seleccionar **elipses** veremos un punto que dice arco, al arrastrarlo se convierte en sweep y modificándolo podremos crear figuras tipo "pacman".  Además aparece otro punto llamado  **ratio** podremos lograr además que ese pacman se convierta en una letra "C".
Cuando dibujamos un **polígono** por default será un triángulo. Al seleccionarlo veremos dos puntos uno será para redondear los vértices y el otro para aumentar la cantidad de lados
Cuando seleccionamos una **estrella** de manera similar a lo relatado anteriormente veremos tres puntos. Uno para determinar si tendrá picos finos o gruesos, uno para redondear los vértices y uno para aumentar la cantidad de picos.

> IMPORTANTE: todo lo que podemos lograr al seleccionar la figura arrastrando los puntos también podemos lograrlo en la barra de la derecha de manera mucho más controlada.

## Editar Puntos de Formas
Seleccionando la forma (por ejemplo un rectángulo o elipse) podremos editar sus puntos de ancla presionando `enter` y luego `shift+enter` para salir del modo edición.

# Mapa de Bits
`ctrl + shift + k`: Insertar imagen. 
También es posible arrastrar una imagen desde una carpeta.
También es posible colocarle a cualquier figura geométrica vectorial una imagen como relleno. Esto podemos hacerlo en **Fill** además del relleno sólido, podremos elegir degradado lineal, radial, angular, diamante y en último lugar el relleno de imagen. Por otra parte es posible tener más de un fondo de los mencionados pudiendo trabajar con la opacidad y el orden de los mismos.

## Insertar Múltiples Imagenes
Utilizando el mismo atajo de teclado posible seleccionar mas de una imagen y las iremos insertando como en Illustrator haciendo click sobre la forma contenedora.

# Frames
Con la tecla `f` accdemos a la herramienta con la cual podemos crear un nuevo frame.
Con `ctrl + alt + g` creamos un nuevo frame con el contenido que tenemos seleccionado

# Vectores
Figma es una herramienta de diseño vectorial. Podemos trabajar con la pluma (`p`) o con el lápiz (`shift+p`).
Es posible copiar en Illustrator un vector y pegarlo en Figma. Como se trata de un objeto vectorial que no ha sido construido con las herramientas de figma será recibido como un frame.

# Texto
Es posible trabajar con todas las fuentes de Google Fonts y todas sus variantes de estilo.

Basándonos en la información de https://fontawesome.com/cheatsheet podremos utilizar la fuente Font Awesome 5 Free (que nos servirá para los íconos agrupados dentro **Regular** o **Solid**) o bien Font Awesome 5 Brands.
Por ejemplo supongamos que queremos mostrar el ícono de la ambulancia que encontramos dentro del grupo Solid. Debemos copiar la palabra `ambulance` y pegarla en figma con la fuente `Font Awesome 5 Free` seleccionada y cambiar la variante Regular por Solid.

# Reglas y Guías
`shift + r`: Mostrar / Ocultar Reglas y Guías
`ctrl + shift + 4`: Mostrar / Ocultar  Grid

Cuando no tenemos nada seleccionado tenemos una coordenadas globales, mientras que si seleccionamos un frame en particular este tendrá sus propias coordenadas locales. Si seleccionamos ahora un elemento de un frame en particular podremos posicionarlo respecto de esas coordenadas locales.

Las guías las obtenemos de manera similar a Illustrator estirando desde la regla. Para eliminarlas simplemente las arrastramos hasta arriva de la regla.

# Alineación
Cuando seleccionamos un sólo elemento la alineación la hacemos respecto del frame. 
Para distribuir horizontal o verticalmente elementos debemos tener más de dos elementos y los de las puntas no se moverán y si lo hacen los del medio.


`alt + a` alineación izquierda
`alt + d` alineación derecha
`alt + s` alineación inferior
`alt + w` alineación superior
> Notar que sigue las direcciones de la "flecha" formada por ASDW en el teclado.

`alt + h` centrado horizontal
`alt + v` centrado vertical

`ctrl + alt + shift + h` distribuir en el espacio horizontal
`ctrl + alt + shift + v` distribuir en el espacio vertical

# Constraints
Las restricciones o constraints nos permiten bloquear a los elementos en una cierta posición. Inicialmente tienen la restriccion arriba y a la izquierda, esto significa que si el frame crece en diagonal hacia abajo a la derecha por ejemplo, los elementos permanecerán en su lugar. En cambio si el frame crece en diagonal hacia arriba a la izquierda los elementos se moverán de modo de acompañar dicho crecimiento.
Esto podría ser útil al trabajar con diseño responsive que tenemos cierto menú y queremos que quede centrado al aumentar el tamaño de la pantalla. En este caso podríamos ponerle el constraint Center, Top
En cambio si queremso que algo ocupe todo el ancho podríamos poner el constraint Left & Right
# Grids 
Los grids se habilitan seleccionando el frame en el panel de Layout bajo el nombre Layout Grid. Nos permiten tener una guía de las columnas como usaríamos en Bootstrap o Grid. También es posible configurar filas o una cuadrícula.
Haciendo click en cel signo `+` podemos agregar más de una grid.

# Auto Layout 
Auto layout nos permite crear **frames dinámicos** que responden al contenido que se ensancharán o achicarán conforme cambie el contenido dentro de ellos. Es posible aplicarlo para crear tarjetas fluidas (*fluid cards*). 

Supongamos sin crear ningun frame creamos un texto, lo seleccionamos y si presioamos el atajo `shift + a` vamos a crear un nuevo frame con auto layout activado. Por default tendrá un cierto padding configurado. En cambio si seleccionamos más de un layer y repetimos lo mismo el padding por default será nulo aunque siempre podremos modificarlo desde el menu de la derecha.
En cualquiera de los dos casos al modificar el texto veremos que el tamaño del frame cambia de modo tal de seguir garantizando el padding y el espaciamiento configurados.

Supongamos que queremos hacer una fila de botones. El primer pasó será crear el texto, luego con `shift + a` creamos el frame con auto layout. Luego como es **posible anidar frames con auto layout dentro de otros frames con auto layout** seleccionamos el frame y presionamos `shift + a`. Luego seleccionamos el botón y con `control + d` lo duplicamos. En ese momento podemos ponerle el texto deseado a cada botón. En caso de querer modificar su orden podemos seleccionarlos y presionar las flechas a derecha e izquierda (en caso de tener auto layout vertical presionaríamos las flechas de arriba y abajo). Otra opción es arrastrar el objeto o hacer click en la flecha que nos aparecerá en la barra lateral.

Si queremos arrastrar una capa a un frame y esa es mayor que dicho frame, a priori no nos dejará hacerlo y tendremos que presionar la tecla `ctrl` o desde la barra de layers.

El **auto layout** es sumamente útil a la hora de crear botones, que quiero garantizar un padding determinado independientemente del texto.

## Scrolling Overflow
Supongamos que queremos tener varias de estas cards, para ello al original lo convertimos en componente con `ctrl + alt + k`. A continuación creamos varias instancias y las colocamos en un layer con auto layout `shift + a`. Queremos incorporar scroll horizontal pero no es posible añadir *scrolling overflow* a un frame con autolayout. Esto es porque *scrolling overflow* requiere que el contenido de un frame se extienda fuera de sus límites. Debemos seleccionar el frame y presionar `control + alt + g` para colocarlo dentro de un nuevo frame al cual redimencionaremos (375px). Por último en la pestaña de **Prototype** elegimos Overflow Behavior como Horizontal Scrolling.

# Componentes
Luego de crear un componente con `ctrl + alt + k` podremos verlo en la barra de la izquierda bajo la pestaña **Assets** dentro de Local Components. A su vez en la pestaña Layers podremos ver con un ícono distinto al componente original y a las instancias. Los cambios realizados en el componente original se verán replicados en todas las instancias, mientras que los cambios realizados en ellas sólo afectarán a esa instancia en particular.
Si tenemos un proyecto bastante grande, y no sabemos donde está el componente original, podemos seleccionar una instancia y en la barra de la derecha hacer click en un ícono que nos llevará al **Main Component**. Una vez hechos los cambios que queramos podremos clickear **Return to instance** en la parte inferior.
También podemos elegir **Detatch Instance** de modo que no se vea afectado por los cambios que hagamos de ahora en más en el componente original.
Con **Reset all Overrides** podremos volver a los valores originales del Main Component.
Con **Push Overrides to Main Component** podremos guardar los cambios que tenemos en la instancia en el Main Component.

# Máscaras
La imagen que quiero conservar debe ir arriba y la forma con la que quiero recortarla debe ir abajo. Luego podemos presionar la tecla rápida `ctrl + alt + m`

# Operaciones Booleanas
* **Union Selection**: Conserva el color del elemento de arriba.
* **Substract Selection**: El de abajo - el de arriba
* **Intersect Selection**
* **Exclude Selection**

Cuando aplicamos una de estas operaciones se forma un nuevo grupo especial de capas. Es decir que si nos arrepentimos podemos revertir la operación tanto sacando las capas de ese grupo o bien si aplicamos por ejemplo una unión y nos damos cuenta que es una intersección lo que queremos, podemos aplicarlo simplemente seleccionando este layer que agrupa los elementos.

Cuando ya estamos seguros de la forma obtenida con **Flatten Selection** `ctrl + e` dejamos de tener un grupo y pasamos a tener un objeto vectorial.

# Efectos
Es posible aplicar:
* Drop Shadow
* Innet Shadow
* Layer Blur
* Background Blur

# Modos de Fusión
De manera similar a los programas de Adobe podremos seleccionar entre distintas opciones:
* Pass Through
* Normal

* Darken
* Multiply
* Color Burn

* Lighten
* Screen
* Color Dodge

* Overlay
* Soft Light
* Hard Light

* Difference
* Exclusion

* Hue
* Saturation
* Color
* Luminocity


# Ajustar Colores
Es posible a la hora de elegir un color partir de un color negro y luego con las flechas ir subiendo para buscar el tono deseado (si presionamos `shift+flecha` equivale a presionar la flecha 10 veces). Esto es así debido a que tenemos configurado por default **Small Nudge** 1 y **Big Nudge** 10, lo cual podemos chequear buscando **Nudge** (presionar `ctrl+}` para acceder a la barra de búsqueda).

# Exportar
La función exportar es muy parecida a la de Illustrator, pudiendo exportar en distintos formatos (PNG, JPG, SVG, PDF) y distintas escalas.

Dependerá de lo que tengamos seleccionado. De manera similar a Illustrator, podremos exportar en distintos formatos y escalado en distintos tamaños. Si seleccionamos más de una capa no nos creará una composición sino que generará un ZIP con tantos archivos individuales como capaz hubiera seleccionado.
Sin embargo si seleccionamos un frame sí exportará la composición.

## Exportar y Optimizar SVG
Desde figma podemos seleccionar un objeto y luego hacer click derecho y elegir **Copy as SVG**. Luego para optimizarlo vamos a https://jakearchibald.github.io/svgomg/ y hacemos click en **Paste Markup** y seleccionamos **Remove xmlns**. Luego pegamos el código en un archivo SVG en Visual Studio Code.

# Slice
La herramienta slice tecla rápida `s` nos permite seleccionar un área y exportarla visualizando todo el contenido que tiene dentro. 

Como dijimos anteriormente si quisiéramos obtener tres capas al intentar exportar nos armaría un zip con cada layer de manera individual. Gracias a esta herramienta podemos hacer un rectángulo que envuelva a las tres capas y exportarlas en una sóla imagen.

También podríamos querer una imagen que muestre mas de un frame, en ese caso también podríamos hacerlo con la herramienta slice. Podemos pensar que nos permite obtener algo similar a **capturas de pantalla**.

# Styles
Los estilos nos permiten guardar ciertas propiedades para utilizarlas luego en otros diseños: colores, tipografías, efectos, layout grids, etc. 

Por ejemplo para guardar el color de relleno de una figura, vamos a Fill y hacemos click en **Show Styles** representado por cuatro puntitos y luego con el signo + habilitamos la opción **Create new color style**. Lo mismo rige para Stroke es decir que estaré guardando sólo el color y no sus características de borde. 
> Es posible guardar gradientes e incuso imágenes como estilo de color.

En cambio si repito esta acción en el panel de Effects estaré guardando un nuevo efecto en los estilos.

Lo mismo podemos repetir para guardar tipografías como estilo. Simplemente escribimos un texto con el formato deseado y lo agregamos como estilo.

Si queremos ver todos los estilos podemos hacer click en el canvas y los veremos vayo el panel de propiedades de la derecha Styles. Si queremos eliminarlo podemos hacerlo desde ahí. En caso de editarlo los cambios afectarán a todos los elementos que lo tengan aplicado. De la misma manera si seleccionamos a un elemento con un estilo aplicado veremos la opción **Edit Style**  y **Detatch Style** que nos permite independizarnos de un estilo.

A la hora de nombrar estilos el [equipo de Figma](https://www.figma.com/best-practices/components-styles-and-shared-libraries/organizing-and-creating-libraries/) recomienda usar la convención `fondo/rojo/puro` es decir separado por `/` de esta manera logramos armar una jerarquía. Si tenemos `fondo/rojo/puro` y `fondo/rojo/suave` nos los pondrá juntos y al resto separados. Otro ejemplo al trabajar con tipografías podría ser `Raleway/h1`, `Raleway/h2`, `Roboto/h1`, etc.

# Prototype
> Basado en el [video](https://www.youtube.com/watch?v=ps6p9e6QmgY&list=WL&index=8&ab_channel=KevinPowell) de Kevin Powel 

A la hora de realizar prototipos interactivos debemos asegurarnos de haber alcanzado el diseño final ya que el primer paso es hacer varias copias del frame terminado que luego representarán las distintas escenas. Es decir que si tenemos dos botones que al pasar el mouse por encima cambian de color y luego un menu de tres secciones, tendremos que hacer cinco copias. 

## Efecto de Desplazamiento
La idea será que en el primer frame tenemos opacidad 0 y algunos elementos en una dirección y luego se mueven hacia otra y la opacidad queda en 100%.
Si movemos un cierto elemento hacia el borde del frame de modo tal que una parte quede fuera, debemos tener presente que el cursor al arrastrarlo quede sobre el artboard sino considerará que queremos quitarlo de dicho frame.

Luego seleccionamos la pestaña **Prototype** y luego seleccionamos el frame y veremos un círculo grande a su alrededor. Hacemos click en ese círculo y elegimos el frame que queremos mostrar a continuación. Como queremos que el segundo frame cargue ni bien carga la página en **Interaction Details** elegimos la opción **After Delay** de 1ms y en **Animation** la opción **Smart Animate**. Luego hacemos click en el botón play para presentar el prototipo. 

> Con la tecla `r` podremos volver a cargar al prototipo para así poder ver la animación otra vez.

## Efecto de Hover 
Si tenemos un botón y queremos que al pasar el mouse por encima este cambie de color debemos seleccionar el **elemento** en cuestión (y no el frame como hicimos anteriormente) y luego el **frame** que contiene el estilo para el hover y elegir la opción **While Hovering.**

> En el lugar donde coloquemos el signo de play será el lugar donde comenzará el prototipo.

## Efecto Cruz para Cerrar
En un frame tenemos el hamburguer icon y en el otro, a la línea central la movemos y le quitamos la opacidad y a las otras dos las rotamos.

Como el ícono está formado por tres líneas horizontales y no queremos que el usuario tenga que hacer click justo sobre ellas, creamos un rectángulo del tamaño de todo el ícono y a el le aplicamos el efecto.

## Overlay de Menu
Si queremos que al hacer click sobre un elemento aparezca un menú y este se encuentra fuera del frame, para lograr interactividad debemos convertirlo en componente.

# Design System
En proyectos de magnitud puede que queramos que determinados recursos como colores, tipografías, como así también componentes de UI (botón primario, botón secundario, modal, cabecera, etc) estén disponibles para todos los archivos (y para los miembros del equipo en caso de tener la versión paga de Figma). Design System nos permite confeccionar **guías de estilos** de acuerdo a la **identidad gráfica** del proyecto y reutilizarlos fácilmente en los distintos archivos.

Anteriormente trabajamos con estilos los cuales sólo están disponibles de manera local en el archivo en el cual fueron creados.

Para eso tendremos que publicarlos y esto podemos hacerlo yendo al panel de la izquierda en **Assets** y luego haciendo click en el ícono del libro que dice **Team Library** y allí presionar **Publish**. Cada vez que publicamos debemos colocar un comentario que indique los cambios que se realizaron. De esta manera los usuarios sabrán si actualizan a la última versión o no.

Además en ese mismo apartado podremos activar o desactivar los estilos existentes anteriormente. 
Una vez  activados estos estilos los veremos debajo de los estilos locales en el panel de FILL por ejemplo. Tanto haciendo click en los **Cuatro Puntitos** o eligiendo el selector de color y clickeando el desplegable donde podremos elegir entre **Document Colors** o estos estilos de equipo.

# Diseño Colaborativo
Es posible compartir los diseños realizados haciendo click en el botón **Share** ubicado en la parte superior. Luego podremos determinar si cualquiera que tenga el link puede visualizar o editar el archivo o si queremos que sólo puedan hacerlo aquellos cuyo correo electrónico fue especificado. Es posible la colaboración en tiempo real y ambos afectarán al mismo archivo.
Con la tecla `c` es posible mostrar u ocultar comentarios y respuestas generados por cada  usuario.

# Community
Nos permite explorar archivos publicados por miembros de la comunidad de Figma. La mayoría ofrece una versión gratuita y una de pago.

Al hacer click sobre el botón **Duplicate** podremos crear una copia en mis Drafts. 

# Plugins
Los plugins nos permiten incorporar a Figma cierta funcionalidad desarrolla por una persona ajena a Figma.
* Lorem Ipsum: Nos permite agregar dummy text.
* Google Sheet Sync: nos permite cargar en figma contenido que tenemos en una hoja de cálculo.
* Unsplash: nos permite agregar imágenes de esta página.
* Get Waves: para hacer las típicas ondas de las páginas
* Feather Icons: aporta íconos.
* [Export Styles to CSS Variables](https://www.figma.com/community/plugin/816737626312049592/Export-styles-to-CSS-variables)

Con `ctrl+alt+P` repetimos el último plugin ejecutado.

# Inspect
En la barra lateral derecha nos encontramos con un botón que dice Inspect al hacer click podremos ver las reglas de CSS asociadas al frame o elemento que tengamos seleccionado. Si  bien la mayoría de las veces entrega las propiedades usando posicionamiento absolutos, si al diseñar utilizamos la herramienta **auto layout** veremos que la detecta y trabaja con `display: flex`.

# Version History
Si vamos a `File` nos encontramos con `Show Version History` donde podremos ver todas las distintas versiones que Figma fue autoguardando (o cuando guardamos manualmente haciendo uso de la opción `Save to Version History...`). Es posible acceder a cómo se veía el archivo en cada uno de esos momentos haciendo click sobre la versión correspondiente. Sólo se conservan los cambios de los últimos 30 días en la versión gratuita, si queremos conservar alguna versión pasado ese tiempo podríamos hacer una copia (haciendo click derecho sobre la versión en particular) y guardarlo como un nuevo archivo

# Consideraciones de Diseño
Si estamos trabajando en diseño responsivo para dispositivos móviles, tablets y escritorio quizás nos convenga utilizar como tamaño de frame el **Iphone SE** ya que es el dispositivo de pantalla más pequeña 320x568 de los disponibles.