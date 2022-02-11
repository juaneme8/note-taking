# Emmet
Se trata de una herramienta para mejorar el trabajo con HTML y CSS. **Emmet** toma la idea de los *snippets* de código y mediante expresiones con una sintaxis similar a la usada en los selectores de CSS, produce una salida dependiente de las abreviaciones usadas.

Visual Studio Code soporta Emmet sin la necesidad de instalar ninguna extensión.

Por ejemplo si ingresamos `#page>div.logo+ul#navigation>li.item$*5>a[href="#"]{Elemento $}`

Esto se traducirá en:
```html
<div id="page">
  <div class="logo"></div>
  <ul id="navigation">
    <li class="item1"><a href="#">Elemento 1</a></li>
    <li class="item2"><a href="#">Elemento 2</a></li>
    <li class="item3"><a href="#">Elemento 3</a></li>
    <li class="item4"><a href="#">Elemento 4</a></li>
    <li class="item5"><a href="#">Elemento 5</a></li>
  </ul>
</div>
```
Podemos encontrar información completa sobre la sintaxis de las abreviaciones:
[https://docs.emmet.io/abbreviations/syntax/](https://docs.emmet.io/abbreviations/syntax/)



> Notar que el signo `$` en `{Elemento $}` luego es reempalzado por un número.