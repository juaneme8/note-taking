# Recursividad

Un método recursivo es aquel que se llama a si mismo hasta alcanzar una condición que detiene la recursividad.

La recursividad consiste en tomar un problema grande y partirlo en un montón de subproblemas más pequeños.

Como primera analogía podríamos tomar el caso de una persona haciendo fila en un cajero que desea saber cuanta gente tiene adelante. Le preguntará a la persona de adelante y esta a la de adelante hasta llegar al último que dirá que es el número 1. A partir de esto todos sabrán qué número son y el primero obtendrá su resultado.

En términos de código llamaremos a la función recursivamente cambiando el valor con el cual la llamamos hasta alcanzar la condición de finalización.

```js
function getMyPositionInLine(person){
	if(person.nextInLine===null){
		return 1;
	}
	return 1+getMyPosition(person.nextPersonInLine)
}
```



# Ventajas y Desventajas

Con recursividad logramos un código elegante para situaciones complejas obteniendo resultados con pocas líneas de código. Como contrapartida puede que tengamos una mayor carga de CPU y lentitud.

La recursividad reduce la necesidad de loops complejos y estructuras auxiliares de datos. Sin embargo puede ocasionar errores de *out of memory* o excepciones de *stack overflow*. 



