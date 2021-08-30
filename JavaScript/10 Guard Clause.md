# Guard Clause
> Basado en https://learningactors.com/javascript-guard-clauses-how-you-can-refactor-conditional-logic/

El concepto de **guard clause** nos permitirá refactorizar los condicionales de modo tal que en lugar de usar `if...else` usemos sólo `if` evitando de este modo los anidamientos. Nos basaremos en el concepto de *early return*, por ejemplo si tenemos:
```js
function userIsAdmin(user) {
	if (user.role == 'admin') {
		if (user.manager == true) {
			return true;
		}
		else {
			return false;
		}
	}
	else {
		return false;
	}
}
```
Este código puede resultar difícil de entender y de mantener, mientras que usando el concepto de **guard clause**:

```js
function userIsAdmin(user) {
 if (user.role != 'admin') return false
 if (user.manager != true) return false
 return true
}
```

Si en lugar de retornar `false` si no es un `admin` quisiéramos mostrar un mensaje en la consola podríamos hacerlo también en una sóla linea de este modo: `if (user.role != 'admin') return console.log('not an admin');`

Si quisiéramos dar un paso mas en la refactorización puede que convenga extraer el código a otras funciones, por ejemplo:

```jsx
function userHasAdminRole(user) {
	return (user.role == 'admin')
}
function userIsSeniorManager(user) {
	return (user.manager == 'senior')
}
```

Luego las utilizamos de este modo:

```js
function userIsAdmin(user) {
	if (userHasAdminRole(user) && userIsSeniorManager(user)) return true;
	
return false
}
```

O bien si queremos reducirlo todavía más:

```js
function userIsAdmin(user) {
	return (userHasAdminRole(user) && userIsSeniorManager(user))
}
```