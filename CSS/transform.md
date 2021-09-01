#  Transform

La propiedad `transform` aplica una transformación 2D o 3D con la cual nos permite trasladar, rotar, escalar o sesgar elementos. 

`transform: none|_transform-functions_|initial|inherit;`


Si la propiedad tiene un valor diferente a `none` se creará un **stacking context**.


## Transformaciones 2D

Al transformar un elemento parte de este puede quedar fuera del contenedor por lo que tendré que ajustar los márgenes.


### `tranlate()`

El método `translate()` mueve un elemento de su posición actual de acuerdo a los valores de coordenadas x e y. `transform:  translate(50px, 100px);`  También podemos escribirlo como `transform: translateX(50px) translateY(100px);`
Al utilizar porcentajes estos serán relativos al elemento en sí mismo y no al padre, es por eso que habitualmente se utiliza para centrar elementos:

```css
.parent{  
	position: relative;  
}  
.child{  
	position: absolute;  
	top: 50%;  
	left: 50%;  
	transform: translate(-50%, -50%);  
}
```


### `translateX()`

El método `translate()` mueve un elemento de su posición actual en el eje x
`transform:  translateX(50px);`


### `translateY()`

El método `translate()` mueve un elemento de su posición actual en el eje x
`transform:  translateY(100px);`



### `rotate()`

El método `rotate()` rota un elemento en sentido horario (valor positivo) o anti-horario (valor negativo) de acuerdo al valor recibido. `transform:  rotate(20deg);`


### `scale()`

El método `scale()` aumenta o disminuye el tamaño de un elemento. 
Si queremos aumentar al doble el ancho y disminuir a la mitad alto: `transform:  scale(2, .5);` Otra forma de poner lo mismo sería `transform: scaleX(2) scaleY(.5);`
Si queremos duplicar el alto y ancho podemos poner `transform: scale(2)`


### `scaleX()`

El método `scaleX()`aumenta o disminuye el ancho de un elemento


### `scaleY()`

El método `scaleY()` aumenta o disminuye el alto de un elemento


### `skew()`

El método `skew()` sesga un elemento en el eje x y el eje y de acuerdo a los ángulos recibidos.
Si el segundo parámetro no se especifica se comporta igual que `skewX()`. 


### `skewX()`

El método `skewX()` sesga un elemento en el eje x  de acuerdo al ángulo recibido.


### `skewY()`

El método `skewY()` sesga un elemento en el eje y  de acuerdo al ángulo recibido.


### `matrix()`

El método `matrix()` combina todos los métodos de transformaciones 2D en uno. Recibe 6 parámetros: `matrix(scaleX(),skewY(),skewX(),scaleY(),translateX(),translateY())`



Vamos a poder tener expresiones como `transform: scale(2, 0.5) translate(100px, -100px);`


## Transformaciones 3D


### `rotateX()`

El método `rotateX()` produce una rotación en el eje X según los grados especificados.


### `rotateY()`

El método `rotateY()` produce una rotación en el eje Y según los grados especificados.


### `rotateZ()`

El método `rotateZ()` produce una rotación en el eje Z según los grados especificados.


Si queremos rotar en los tres ejes `transform: rotateX(45deg) rotateY(10deg) rotateZ(50deg);`


# `transform-origin`

La propiedad `transform-origin` nos permite cambiar el origen de la transformación. El valor predeterminado es `transform-origin: center center;` donde el primer valor corresponde al eje x y el segundo al eje y.


`transform-origin: [x-axis y-axis z-axis]|initial|inherit;`


# `transform-style`


# `perspective`

La propiedad `perspective` determina qué tan alejado se encuentra el objeto del usuario. Un valor bajo de *length* resulta en un efecto 3D más intenso que un valor alto. Se aplica sobre el **padre** del elemento que queremos que tenga la vista en perspectiva.
`perspective: [length]|none;` 


# `perspective-origin`

La propiedad `perspective-origin` determina desde qué posición está mirando al objeto 3D el usuario. Debemos definir esta propiedad sobre el padre y se verán afectados los hijos.
`perspective-origin: 19% 50%;`


# `backface-visibility`
````





Markdown 3869 bytes 551 words 95 lines Ln 1, Col 0

HTML 3072 characters 527 words 63 paragraphs