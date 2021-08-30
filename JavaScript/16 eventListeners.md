# Formularios y Eventos
Si tengo un `<input type="Number" id="number'>` y quiero obtener un evento cuando ingrese números o presione las flechas hacia arriba/abajo:
```js
const numberInput = document.getElementById('number');
numberInput.addEventListener('input',()=>{
	...
});
```
Si quiero ocultar el `<input>` puedo hacerlo con `numberInput.style.visibility='hidden'` y luego `numberInput.style.visibility='visible'` para mostrarlo.