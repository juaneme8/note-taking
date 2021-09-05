# date-fns
[date-fns](https://date-fns.org/) es un paquete que nos permite formatear fechas de manera simple. Podemos instalarlo con `npm i date-fns` 

```jsx
import {format} from 'date-fns'

console.log( 
	format(new Date(), 'do MMMM Y') 
)
//Veremos por ejemplo 2nd March 2021
```

> Notar que este paquete es utilizado cuando incorporamos DatePickers en Material UI.