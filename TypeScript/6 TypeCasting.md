## TypeCasting
Continuando con lo visto hasta ahora si tenemos en nuestro html un formulario, podemos acceder a él haciendo: `const form = document.querySelector('form')!;` En ese caso al pasar el mouse por encima nos dirá que se trata de un `HTMLFormElement`.

Si embargo si ese mismo formulario tiene una clase `.new-item-form` y hacemos `const form = document.querySelector('.new-item-form')!;` nos aparecerá como `Element` ya que estamos haciendo el query de  una clase y como tal puede ser aplicada a cualquier elemento por lo que TS no tiene forma de saber cuál en particular estamos seleccionando.

En esos caso podemos usar TypeCasting con lo cual indicamos qué tipo de elemento será, lo casteamos para que sea de tipo HTMLFormElement

```ts
const form = document.querySelector('.new-item-form') as HTMLFormElement;
```
Notar que no se usa con el `!` ya que al usar TypeCasting no nos aparecerá como que posiblemente sea null. Lógicamente si efectivamente ese elemento no existe al mostrarlo en pantalla una vez compilado lo veremos como `null`

De manera similar a lo visto hasta ahora si tenemos este html:
```html
<form>
	<select id="service">
		<option value="invoice">Invoice</option>
		<option value="payment">Payment</option>
	</select>
	<div class="field">
		<label for=""> To / From </label>
		<input type="text" id="tofrom" />
	</div>
	<button>Add</button>
</form>
```
Podremos acceder al `form`, al `select` y al `input` de la siguiente forma:
```ts
const form = document.querySelector('.new-item-form') as HTMLFormElement;
const service= document.querySelector('#service') as HTMLSelectElement;
const tofrom = document.querySelector('#tofrom') as HTMLInputElement;
```


A la hora de trabajar con eventos, indicamos que `e` es de tipo `Event`
```ts
form.addEventListener('submit', (e: Event) => {
	e.preventDefault();
	console.log(types.value);
	console.log(tofrom.valueAsNumber);
	console.log(amount.valueAsNumber);
});
```

> A la hora de mostrar los valores de un input con `type="number"` podríamos usar `.valueAsNumber` para obtener el número en lugar de `.value` que nos lo mostraría como string.