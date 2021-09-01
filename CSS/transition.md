# Transitions
Las transiciones nos permiten animar componentes cambiando propiedades de manera gradual.

# `transition`
La propiedad `transition` es un *shorthand* de cuatro propiedades:
* `transition-property`
* `transition-duration`
* `transition-timing-function`
* `transition-delay`

`transition: transition-property transition-duration transition-delay transition-timing-function`

En cuanto al orden de las propiedades en el *shorthand* el segundo número es `transition-delay`

Debemos especificar  la propiedad que queremos afectar y la duración del efecto `transition:  width 2s;` También es posible añadir una transición a más de una propiedad:  `transition:  width 2s, height 4s;`

La transición sucede cuando la propiedad especificada cambie de valor.

# `transition-delay`
La propiedad `transition-delay` hace referencia a cuánto tarda la transición en suceder. Se especifica en segundos (`s`) o milisegundos (`ms`).

# `transition-duration`
La propiedad `transition-duration`hace referencia a la duración de la transición. Se especifica en segundos (`s`) o milisegundos (`ms`).

# `transition-property`
La propiedad `transition-property` indica cuál es la propiedad que sufrirá la transición. En caso de que haya más de una debemos separarlas por comas.
Se recomienda no utilizar `all` ya que es malo en términos de performance.

# `transition-timing-function`
La propiedad `transition-timing-function` nos permite especificar la curva de velocidad de la transición. 
* **ease**: comienza lento, luego rápido y termina lento (valor por default)
* **linear**: mantiene velocidad constante de principio a fin (se recomienda para transiciones de opacidad).
* **ease-in**: comienza lento (analogía con auto que arranca y lllega a la velocidad máxima)
* **ease-out**: comienza rápido y termina lento. (analogía con golpe a pelota de tenis)
* **ease-in-out**: Comienza lento y termina lento. No es recomendable usarla en transciones de menos de un segundo.
* **cubic-bezier**: nos permite personalizar la *timing function*. Cada uno de los valores anteriores `ease`, `linear`, etc tienen una equivalencia en `cubic-bezier(n,n,n,n)`.
Con las Devtools podemos hacer click en el reloj y personalizar la curva para luego pegarla en el código.

Tanto para `transition-duration`, `transition-delay` y `transtion-timing.function` en caso de tener `transition-property` con más de una propiedad podemos especificar los valores separados con comas a menos que sean siempre el mismo en cuyo caso solo especificamos uno.