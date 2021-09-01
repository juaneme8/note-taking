# Favicons
Es posible utilizar imágenes en formato SVG como Favicons, es algo soportado por la mayoría de los navegadores modernos, debemos colocar dentro del `<head>`:
```css
<link rel="icon" type="image/svg+xml" href="/favicon.svg">  
<link rel="alternate icon" href="/favicon.ico">  
```
Si el navegador no soporta favicons SVG, ignorará la primera declaración `<link>` y continuará hacia la segunda. Podremos ver que `rel=alternate` con lo cual le indicamos al navegador que el `.ico` es exclusivamente una alternativa.

