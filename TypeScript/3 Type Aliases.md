## Type Aliases 
En ocasiones al trabajar con funciones con parámetros tenemos especificaciones de tipo bastante largas o complejas que muchas veces tendremos que repetir para distintas funciones:
```ts
const logDetails = (uid: string | number, item: string) => {
	console.log(`${item} hasta uid of ${uid}`);
};

const greet = (user: { name: string; uid: string | number }) => {
	console.log(`${user.name} says hello`);
};

```
En esos casos es conveniente utilizar alias de tipo con lo cual nos queda mucho más compacto:
```ts
type StringOrNum = string | number;
type objWithName = { name: string; uid: StringOrNum };

const logDetails = (uid: StringOrNum, item: string) => {
	console.log(`${item} hasta uid of ${uid}`);
};

const greet = (user: objWithName) => {
	console.log(`${user.name} says hello`);
};

```