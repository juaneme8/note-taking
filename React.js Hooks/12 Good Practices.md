# Derived State

Debemos prestar especial atención a los derived states en React. Supongamos que tenemos en pantalla una serie botones que incrementan el valor de un contador que se muestra en pantalla. Además agregamos al lado de esos botones otro botón que nos permita seleccionar cada contador y mostrar en un único lugar el valor de ese contador. 

Una posible implementación de eso sería que al presionar el botón de "Seleccionar" recibamos el ID del botón y en función de eso almacenemos el contador en una variable. Esto ocasionaría que mostremos correctamente el valor del contador pero si después lo incrementamos, este valor no se vea incrementado en pantalla. Es por eso que debemos almacenar el id y no el producto.

Una vez que tenemos el id hacemos un filtro para quedarnos con el contador en particular y de esta manera nos aseguramos que el valor esté siempre actualizado. Como ese filtro se va a ejecutar cada vez que rendericemos debemos utilizar `useMemo`.



## Multiples Promises

Cuando tenemos un array de elementos y queremos llamar a un endpoint por cada uno de ellos, una opción sería usar `for of` (ya que `forEach` no funciona con async/await). Otra opción es utilizando `await Promise.all([p1,p2,p3])` con una implementación con `map` que aunque no podremos trabajar del modo habitual con async await dentro, al ser una async devolverá una promesa.

```jsx
const homeworldUrl = await Promise.all(
	people.map(async (person) => {
		const homeworld = axios.get(person.homeworld)
	})
)
```

