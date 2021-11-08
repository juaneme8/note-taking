# Challenge
Basado en el stream de Gonzy Pozzo https://www.twitch.tv/videos/910267697
Repositorio inicial: https://github.com/goncy/coderio-challenge

Usando la API http://worldtimeapi.org/
En particular estos dos endpoints:
http://worldtimeapi.org/api/timezones
http://worldtimeapi.org/api/timezone/America/Argentina/Salta

Primero en el server express creamos dos rutas `/timezones` que devuelve todas las zonas y `/timezone/:zone` que devuelve datos de una específica.

## `encodeURIComponent()`
Tomando como ejemplo la url `America/Argentina/Salta` vemos que al tener varias barras no será procesado por `/:zone` por lo que desde el cliente debemos usar `encodeURIComponent('America/Argentina/Salta')
` que nos da como resultado `America%2FArgentina%2FSalta` de modo que `http://localhost:5000/timezone/America%2FArgentina%2FSalta` ahora funcionará.

Ahora debemos consumir estas APIs desde nuestra aplicación de front.