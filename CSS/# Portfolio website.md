# Porfolio Website
> Basado en el video de Kevin Powell para `freeCodeCamp.org` [Build and deploy a portfolio website](https://www.youtube.com/watch?v=_xkSvufmjEs) que forma parte de un curso de  [Scrimba](https://scrimba.com/g/gportfolio)
> Los archivos de AdobeXD podemos encontrarlos en [este enlace](http://adobe.ly/2GAgF93)
> Los archivos de este proyecto podemos encontrarlos en la carpeta `md-portfolio`

## Fuentes Utilizadas
Utilizaremos las fuente de Google
Source Code Pro 400,900 (regular y black)
Sourse Sans Pro 300,900 (ligth y black)

Lo que se traduce en:
```html
<link href="https://fonts.googleapis.com/css2?family=Source+Code+Pro:wght@400;900&family=Source+Sans+Pro:wght@300;900&display=swap" rel="stylesheet">
```
Luego para usarlas
```css
font-family: 'Source Code Pro', monospace;
font-family: 'Source Sans Pro', sans-serif;
```

## Reseteos Iniciales
```css
*,
*::before,
*::after {
    box-sizing: border-box;
}

img {
    display: block;
    max-width: 100%;
}

```
## Custom Properties
A la hora de definir las variables relacionadas con las fuentes las llamamos `-ff-primary` y `--ff-secondary` para hacerlo lo más genérico posible, quizás podríamos haberlo llamado `--ff-sans` y `--ff-mono`
Los tamaños de las fuentes los pensamos en función del diseño en AdobeXD.
Utilizamos un *media querie* para que cuando la pantalla es superior a cierto tamaño, las fuentes sean más grandes por lo que redefinimos el valor de las variables.
Tomando como ejemplo `--fs-h1` veremos que va de `3rem` a `4.5rem` para pantallas más grandes. En el proyecto de **Landing Page** usamos la regla `font-size: clamp(3rem, 5vw + 1rem, 4.5rem);` para los títulos con lo cual nos ahorramos esa media querie extra.
Como queremos usar una *box-shadow* en más de un lugar creamos la custom propertie `--bs` para incrementar el realismo (material design por ejemplo tiene 2 o 3 sombras) utilizamos dos niveles de sombras colocando primero la más grande y luego la más pequeña.
```css
:root {
    --ff-primary: 'Source Sans Pro', sans-serif;
    --ff-secondary: 'Source Code Pro', monospace;
    
    --fw-reg: 300;
    --fw-bold: 900;
    
    --clr-light: #fff;
    --clr-dark: #303030;
    --clr-accent: #16e0bd;
    
    --fs-h1: 3rem;
    --fs-h2: 2.25rem;
    --fs-h3: 1.25rem;
    --fs-body: 1rem;
    
    --bs: 0.25em 0.25em 0.75em rgba(0,0,0,.25),
          0.125em 0.125em 0.25em rgba(0,0,0,.15);
}

@media (min-width: 800px) {
    :root {
        --fs-h1: 4.5rem;
        --fs-h2: 3.75rem;
        --fs-h3: 1.5rem;
        --fs-body: 1.125rem;
    }
}
```


## `<header>`
En la `<img>` de `.logo`, dejamos el atributo `alt=""` vacío ya que no aporta información el hecho de que el *screen reader* reproduzca por ejemplo la palabra "logo".
En el `<button>` usamos `aria-label="toggle navigation"` ya que como el *hamburguer button* no tiene ningún texto, en términos de accesibilidad será importante brindar información.
```html
<header>
    <div class="logo">
        <img src="img/devjane.png" alt="">
    </div>
    <button class="nav-toggle" aria-label="toggle navigation">
        <span class="hamburger"></span>
    </button>
    <nav class="nav">
        <ul class="nav__list">
            <li class="nav__item"><a href="#home" class="nav__link">Home</a></li>
            <li class="nav__item"><a href="#services" class="nav__link">My Services</a></li>
            <li class="nav__item"><a href="#about" class="nav__link">About me</a></li>
            <li class="nav__item"><a href="#work" class="nav__link">My Work</a></li>
        </ul>
    </nav>
</header>
        
```
Utilizamos los ids únicamente con el propósito de que funcionen como anclas, no es aconsejable usarlos con propósitos de *styling*.
## `<section>` `.intro` 
```html
<section class="intro" id="home">
   <h1 class="section__title section__title--intro">
       Hi, I am <strong>Jane Smith</strong>
   </h1>
   <p class="section__subtitle section__subtitle--intro">front-end dev</p>
   <img src="img/dev-jane-01.jpg" alt="a picture of Jane Smith smiling" class="intro__img">
</section>
```
## `<section>` `.my-services` 
```html
<!-- My services -->
<section class="my-services" id="services">
   <h2 class="section__title section__title--services">What I do</h2>
   <div class="services">
       <div class="service">
           <h3>Design + Development</h3>
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod</p>
       </div> 
       
       <div class="service">
           <h3>E-Commerce</h3>
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod</p>
       </div> 
       
       <div class="service">
           <h3>WordPress</h3>
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod</p>
       </div> 
   </div> 
   
   <a href="#work" class="btn">My Work</a>
</section>
        
```

# `<section>` `.about`
```html
<section class="about-me" id="about">
   <h2 class="section__title section__title--about">Who I am</h2>
   <p class="section__subtitle section__subtitle--about">Designer & developer based out of NYC</p>
   
   <div class="about-me__body">
       <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
       <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
   </div>
   
   <img src="img/dev-jane-02.jpg" alt="Jane leaning against a bus" class="about-me__img">
</section>
```

# `<section>` `.work`
```html
<section class="my-work" id="work">
  <h2 class="section__title section__title--work">My work</h2>
  <p class="section__subtitle section__subtitle--work">A selection of my range of work</p>
  
  <div class="portfolio">
      <!-- Portfolio item 01 -->
      <a href="portfolio-item.html" class="portfolio__item">
          <img src="img/portfolio-01.jpg" alt="" class="portfolio__img">
      </a>
      <!-- Portfolio item 02 -->
      <a href="portfolio-item.html" class="portfolio__item">
          <img src="img/portfolio-02.jpg" alt="" class="portfolio__img">
      </a>
      <!-- Portfolio item 03 -->
      <a href="portfolio-item.html" class="portfolio__item">
          <img src="img/portfolio-03.jpg" alt="" class="portfolio__img">
      </a>
 
  </div>
</section>
```

## `footer`
```html
<footer class="footer">
   <!-- replace with your own email address -->
   <a href="mailto:hello@jane.dev" class="footer__link">hello@jane.dev</a>
   <ul class="social-list">
       <li class="social-list__item">
           <a class="social-list__link" href="https://codepen.io">
               <i class="fab fa-codepen"></i>
           </a>
       </li>
       <li class="social-list__item">
           <a class="social-list__link" href="http://dribbble.com">
               <i class="fab fa-dribbble"></i>
           </a>
       </li>
       <li class="social-list__item">
           <a class="social-list__link" href="https://twitter.com">
               <i class="fab fa-twitter"></i>
           </a>
       </li>
       <li class="social-list__item">
           <a class="social-list__link" href="https://github.com">
               <i class="fab fa-github"></i>
           </a>
       </li>
   </ul>
</footer>
```
## General Styles
```css
/* endable this to add smooth scrolling */
/* html {
    scroll-behavior: smooth;
} */

body {
    background: var(--clr-light);
    color: var(--clr-dark);
    margin: 0;
    font-family: var(--ff-primary);
    font-size: var(--fs-body);
    line-height: 1.6;
}

section {
    padding: 5em 2em;
}

strong { font-weight: var(--fw-bold) }

:focus {
    outline: 3px solid var(--clr-accent);
    outline-offset: 3px;
}

```

## Botones
```css
.btn {
    display: inline-block;
    padding: .5em 2.5em;
    background: var(--clr-accent);
    color: var(--clr-dark);
    text-decoration: none;
    cursor: pointer;
    font-size: .8rem;
    text-transform: uppercase;
    letter-spacing: 2px;
    font-weight: var(--fw-bold);
    transition: transform 200ms ease-in-out;
}

.btn:hover {
    transform: scale(1.1);
}


```
## Tipografía
```css
h1,
h2,
h3 {
    line-height: 1;
    margin: 0;
}

h1 { font-size: var(--fs-h1) }
h2 { font-size: var(--fs-h2) }
h3 { font-size: var(--fs-h3) }


.section__title {
    margin-bottom: .25em;
}

.section__title--intro {
    font-weight: var(--fw-reg);
}

.section__title--intro strong {
    display: block;
}

.section__subtitle {
    margin: 0;
    font-size: var(--fs-h3);
}

.section__subtitle--intro,
.section__subtitle--about {
    background: var(--clr-accent);
    padding: .25em 1em;
    font-family: var(--ff-secondary);
    margin-bottom: 1em;
}

.section__subtitle--work {
    color: var(--clr-accent);
    font-weight: var(--fw-bold);
    margin-bottom: 2em;
}

```
## Sección Intro
A la hora de pensar en el diseño responsivo en lugar de pensarlo en términos del dispositivo podemos crear media queries para pantallas genéricas mayores a 300px, para mayores a 600px y para mayores a 900px. Para esta sección lo hacemos para pantallas mayores a 600px. 
Queremos tener un `display: grid` con dos columnas la primera con la imagen y la segunda en la fila superior el `.section__title` y en la inferior `.section__subtitle`. Establecemos `grid-template-columns: min-content max-content;` ya que queremos que la imagen ocupe el menor espacio posible (luego especificamos su ancho en `.intro__img`) y que el texto ocupe el mayor espacio posible. Luego establecemos el `width: min-content;` y lo centramos con `margin: 0 auto;`.
El valor por default de align-self es `align-self: stretch` esto hace que el subtitulo en determinadas medidas ocupe más espacio que el del texto, es por eso que le agregamos `align-self: start`
En cuanto al posicionamiento del subtítulo ponemos `grid-column: -1/1` (ya que queremos que ocupe todo el ancho) y `grid-row: 2` ya que sino iría debajo de la imagen para evitar el *overlap*.
Para lograr que la línea verde del subtítulo sobresalga hacia la izquierda, podríamos recurrir a `position: absolute;` pero como qen ocasiones es mejor evitarlo, utilizamos `position:relative;` `left: -1.5em;`y como eso generará un corrimiento hacia la izquierda, para que vuelva a la posición original le ampliamos el ancho con `width: calc(100% + 1.5em);`
Para lograr que la imagen se ubique por encima de todo `position: relative` y `z-index: 2`
```css
/*  Intro section  */
.intro {
    position: relative; /*creo que finalmente no se usa porque utilizamos posicionamiento relativo para el subtítulo*/
}

.intro__img {
    box-shadow: var(--bs);
}

.section__subtitle--intro {
    display: inline-block;
}

@media (min-width: 600px) {
    .intro {
        display: grid;
        width: min-content;
        margin: 0 auto;
        grid-column-gap: 1em;
        grid-template-areas: 
            "img title"
            "img subtitle";
        grid-template-columns: min-content max-content;
    }
    
    .intro__img {
        grid-area: img;
        min-width: 250px;
        position: relative;
        z-index: 2;
    }    
    
    .section__subtitle--intro {
        align-self: start;
        grid-column: -1 / 1;
        grid-row: 2;
        text-align: right;
        position: relative;
        left: -1.5em;
        width: calc(100% + 1.5em);
    }
}

```

56m