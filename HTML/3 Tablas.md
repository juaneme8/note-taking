# Tablas
Son utilizadas únicamente para mostrar información tabular.
```html
<table>
	<caption>
		Sueldos Promedio
	</caption>
	<tr>
		<th>Puesto</th>
		<th>Salario</th>
	</tr>
	<tr>
		<td>sSR</td>
		<td>80000</td>
	</tr>
</table>
```
Con `<caption>` le damos un título a la tabla,
`<th>` es el header y tiene importancia tanto semántica como en cuanto a estilo (en negrita y centrado)
`<tr>` representa una fila (row) y `<td>` los datos de la celda.

## Alineación en tablas
La alineación debemos llevarla a cabo con CSS, para alineación horizontal con `<td style="text-align:right">$100</td>` y para alineación vertical con `<td style="vertical-align:bottom">January</td>` (el valor default es `middle`).

Es decir que no utilizamos los atributos `align` y `valign` de html.

## `colspan`
Si queremos tener una celda que abarque dos columnas lo hacemos con `colspan`. Pensarlo como que *spans x columns*

```html
<table>  
	<tr>  
		<th>Month</th>  
		<th>Savings</th>  
	</tr>  
	<tr>  
		<td>January</td>  
		<td>$100</td>  
	</tr>  
	<tr>  
		<td colspan="2">Sum: $180</td>  
	</tr>  
</table>
```
## `rowspan`
Si queremos tener una celda que ocupe dos filas lo hacemos con `rowspan`
```html
<table>  
	<tr>  
		<th>Month</th>  
		<th>Savings</th>  
		<th>Savings for holiday!</th>  
	</tr>  
	<tr>  
		<td>January</td>  
		<td>$100</td>  
		<td rowspan="2">$50</td>  
	</tr>  
	<tr>  
		<td>February</td>  
		<td>$80</td>  
	</tr>  
</table>
```