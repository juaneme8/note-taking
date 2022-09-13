# BEM
BEM significa *block element modifier* se trata de una *CSS naming convention*
```html
<div class="card card--light">
    <img src="" alt="" class="card__image">
    <h2 class="card__title"></h2>
    <p class="card__body"></p>
</div>
```
En este caso vemos que `card--light` es un **modificador del bloque** mientras que `card__image`, `card__title` y `card__body` son **elementos dentro del bloque**.

En proyectos significativos debemos tratar de anidar lo menos posible en vez de hacer `.card p {...}` conviene hacer `.card__body{..}`. Es decir que preferimos tener una clase para cada elemento a editar.



## Emmet

Supongamos que tenemos un código como el siguiente: `nav.nav>ul.nav__list>li*4.nav__item>a.nav__link` 