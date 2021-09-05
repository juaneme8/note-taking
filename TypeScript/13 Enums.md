## Enums
Los enums nos permiten establecer una serie de constantes o keywords y asociarlos con un valor numérico.
Supongamos que tenemos un campo `resourceType` que es un número que representa a un recurso en particular, por ejemplo 3 representa un libro, 4 una persona, etc.
```ts
interface Resource<T> {
	uid: number;
	resourceType: number;
	data: T;
}

const docOne: Resource<object> = {
	uid: 1,
	resourceType: 3,
	data: { title: 'name of the wind' },
};

const docTwo: Resource<object> = {
	uid: 10,
	resourceType: 4,
	data: { name: 'yoshi' },
};
```
 Esto tiene la desventaja que a largo plazo será de difícil mantenimiento pues tendremos que recordar qué número hace referencia a qué recurso. 
 Como solución a esto podemos usar **enums** de modo tal de definir constantes descriptivas y asociarlas a un valor numérico.
```ts
enum ResourceType {
	BOOK,
	AUTHOR,
	FILM,
	DIRECTOR,
	PERSON,
}
interface Resource<T> {
	uid: number;
	resourceType: ResourceType;
	data: T;
}

const docOne: Resource<object> = {
	uid: 1,
	resourceType: ResourceType.BOOK,
	data: { title: 'name of the wind' },
};

const docTwo: Resource<object> = {
	uid: 10,
	resourceType: ResourceType.AUTHOR,
	data: { name: 'yoshi' },
};

```