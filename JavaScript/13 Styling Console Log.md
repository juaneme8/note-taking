# Styling Console Log
```js
const label = "Tensión";
const value = 220;


console.log(`%c ${label}`, "background-color:red");
console.log(`%c ${label} %c {value}`, "background-color:red", "");
console.log(
  `%c ${label} %c {value}`,
  "background-color:red",
  "background-color: blue"
);
```

Debemos colocar `%c` y a continuación el contenido que queremos que tenga un determinado estilo, especificando luego como segundo argumento ese estilo. 

Si queremos resetear el estilo, colocamos un segundo `%c` y como segundo estilo pasamos un string vacío.