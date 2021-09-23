#   Validación de Entrada:
Como buena práctica siempre verificar que lo que envía el cliente (por eso hablamos de **validación de entrada que será distinto a la validación de mongoose** que veremos más adelante) mediante un post o put request sea acorde a lo que esperamos.
Si bien esto podemos hacerlo manualmente mediante condicionales y caso de detectar algo erróneo enviar un error 400 (bad request) y retornar en aplicaciones de cierta complejidad el contenido de   `req.body` tendrá varias propiedades, por lo que un chequeo manual demandará bastante trabajo. 
Para evitar esto podemos utilizar un paquete encargado de la validación.
Al estudiar el curso de Mosh proponían el uso de `joi` pero actualmente ese paquete está obsoleto y fue movido a `@hapi/joi`.

## Paquete @hapi/joi:
**Aclaración 1:** Anteriormente se llamaba joi, pero en npm aparece como deprecated y movido a @hapi/joi.
**Aclaración 2:** En npmtrends podemos ver que este paquete no es muy utilizado en la actualidad siendo superado por otros paquetes como  `ajv (another json validator)`

`npm install @hapi/joi` 

Lo primero que tenemos que hacer es definir un **schema** con las propiedades y su tipo, junto con sus características como ser longitud mínima, máxima, etc. 


# CRUD sin DB + Validación
```js
// Código express simplificado parte de require, listen, uso middleware, etc dejando sólo los post y put handlers. 
const  express  =  require('express');
const  Joi  =  require('@hapi/joi'); //retorna una clase (PascalCase)

const  app  =  express();
app.listen(3000, () =>  console.log('Escuchando en Puerto 3000'));
app.use(express.urlencoded({ extended: true }));
app.use(express.json());

const  validarCurso  = (body) => {
	const  schema  =  Joi.object({
		titulo: Joi.string().min(3).max(10).required(),
		resumen: Joi.string().min(3).max(10).required(),
	});
	return  schema.validate(body, { abortEarly: false });
	//{ abortEarly: false } nos permite obtener todos los errores y no sólo uno
};

const  cursos  = [
{ id: 1, titulo: 'curso1', resumen: 'es un curso1' },
{ id: 2, titulo: 'curso2', resumen: 'es un curso2' },
{ id: 3, titulo: 'curso3', resumen: 'es un curso3' },
{ id: 4, titulo: 'curso4', resumen: 'es un curso4' },
];
```
## GET
```js
app.get('/api/cursos', (req, res) => {
	res.send(cursos);
});

app.get('/api/cursos/:id', (req, res) => {
	const  curso  =  cursos.find((c) =>  c.id  ===  parseInt(req.params.id));
	if (!curso) {
		//404 not found
		res.status(404).send('id inexistente');
	}
	res.send(curso);
});
```


## POST
```js
app.post('/api/cursos', (req, res) => {
	//utilizamos destructuring para obtener la propiedad error (null si los datos son correctos)
	const { error } =  validarCurso(req.body);
	if (error) {
		//si quiero retornar sólo el primer error
		//res.status(400).send(error.details[0].message);
		res.status(400).send(error)
		return;
	}
	//creamos un nuevo curso con los datos recibidos
	const  curso  = {
		id: cursos.length  +  1,
		titulo: req.body.titulo,
	};
	  
	//agregamos el curso al array
	cursos.push(curso);
	//entregamos al cliente el curso agregado
	// 201 resource created
	res.status(201).send(curso);
});
```
## PUT
```js
app.put('/api/cursos/:id', (req, res) => {
	//busco el curso
	//Obtenemos una ref.al primer elemento del array que cumple la condición.
	//Como se trata de una ref. si luego lo modificamos cambia el original.
	const  curso  =  cursos.find((c) =>  c.id  ===  parseInt(req.params.id));
	//si no existe retorno 404 (not found)
	if (!curso) {
		res.status(404).send('id inexistente');
		return;
	}
	//valido los datos de entrada
	const { error } =  validarCurso(req.body);
	//si no son válidos retorno 400 (bad request)
	if (error) {
		res.status(400).send(error);
		return;
	}
	//como llegué hasta aca modifico el curso
	curso.titulo  =  req.body.titulo;
	//devuelvo el curso actualizado
	res.send(curso);
});
```
## DELETE
```js
app.delete('/api/cursos/:id', (req, res) => {
	//busco el curso
	const  curso  =  cursos.find((c) =>  c.id  ===  parseInt(req.params.id));
	if (!curso) {
		res.status(404).send('id inexistente');
		return;
	}
	//obtengo el índice del curso en el array
	const  index  =  cursos.indexOf(curso);
	//quiero eliminar desde la posición index 1 elemento
	cursos.splice(index, 1);
	//retorno curso eliminado
	res.send(curso);
});
```

En lugar de `res.status(404).send('id inexistente');` podríamos haber puesto `res.status(404).end()`

`schema.validate(body, { abortEarly: false });` retorna un objeto del cual nos interesa analizar la propiedad `error`, es por eso que utilizo *object destructuring*. 

A continuación mostramos el contenido de `error` cuando el dato tiene dos errores de validación: Por ejemplo `"titulo":"aa"` y `"resumen":""`:

Si bien hasta ahora retornamos `error` también es posible no poner el objeto de opciones `{ abortEarly: false }` y retornar `error.details[0].message` con lo cual el cliente recibirá de a un error a la vez.
```json
{
	"_original": {
		"titulo": "aa",
		"resumen": ""
	},
	"details": [
		{
			"message": "\"titulo\" length must be at least 3 characters long",
			"path": [
			"titulo"
			],
		"type": "string.min",
			"context": {
				"limit": 3,
				"value": "aa",
				"label": "titulo",
				"key": "titulo"
			}
		},
		{
			"message": "\"resumen\" is not allowed to be empty",
			"path": [
				"resumen"
			],
			"type": "string.empty",
			"context": {
				"label": "resumen",
				"value": "",
				"key": "resumen"
			}
		}
	]
}
```

### Limpieza de Código:
En el código anterior hicimos lo siguiente:
```js
if (error) {
	res.status(400).send(error);
	return;
}
```
Podemos remplazarlo por una versión más compacta.
```js
if (error)
	return res.status(400).send(error);

```