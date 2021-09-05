# TypeScript y el DOM
Al igual que en JS es posible utilizar TS para iteractuar con el DOM, para hacer queries de elementos html, reaccionar a eventos, etc.

```ts
const anchor = document.querySelector('a');
```
Si tenemos un elemento `<a>` en la página al hacer `console.log(anchor)` lo veremos, en caso de no tenerlo veremos `null`. Sin embargo si queremos hacer `console.log(anchor.href)` tengamos o no ese elemento, nos marcará un error diciéndonos `Object is possibly 'null'` ya que no tiene forma de saber si en `index.html` tenemos ese elemento.
```ts
const anchor = document.querySelector('a');
console.log(anchor.href);
```
Esto lo podemos solucionar de dos formas:
1. Utilizando un condicional
```ts
const anchor = document.querySelector('a');
if (anchor) {
	console.log(anchor.href);
}

```
2. En aquellos casos donde estamos seguros que existirá el elemento html, ponemos un signo de exclamación `!` como se ve a continuación:
```ts
const anchor = document.querySelector('a')!;
console.log(anchor.href);
```

Una ventaja de usar TS para interactuar con el DOM es que tiene tipos específicos para cada elemento html, si al hacer `const anchor = document.querySelector('a');` pasamos el mouse por encima de `anchor` veremos `const anchor: HTMLAnchorElement | null` y si ponemos el signo `!` como dijimos anteriormente veremos solo `const anchor: HTMLAnchorElement `.
Gracias a esto el Intellisence nos ayudará a la hora de sugerirnos los métodos y propiedades apropiados para ese elemento.