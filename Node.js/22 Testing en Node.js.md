# Testing 

A la hora de testear el backend lo primero que debemos hacer es modificar el archivo `package.json` en particular los scripts para asegurarnos cargar con un valor distinto la variable de entorno `NODE_ENV` según ejecutemos estemos en *development* `npm run dev`, *production* `npm start` o *testing* `npm test`.

Cuando no utilizamos 

Luego en función del valor de `NODE_ENV` será que usaremos uno u otro connection

