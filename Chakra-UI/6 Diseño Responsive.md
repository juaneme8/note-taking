# Diseño Responsivo
Con Chakra podremos realizar el equivalente a los media queries pasándole props al componente. Esto se conoce como *responsive styles*.
Por ejemplo si queremos tener un `Stack` que tenga una dirección `column` para tamaños de pantalla chicos y `row` para `sm` en adelante podremos especificar:

```jsx
<Stack direction={{ base: 'column', sm: 'row' }}>
  <Text bg="red.100" padding={4}>
    1
  </Text>
  <Text bg="red.100" padding={4}>
    2
  </Text>
  <Text bg="red.100" padding={4}>
    3
  </Text>
</Stack>
```
Otro ejemplo podríamos ser si queremos tener un color de texto al hacer hover en pantallas chicas y otro en pantallas grandes:
`<Text _hover={{color: {base:"red.500", sm:"blue.500"}}}`

> Notar que ponemos propiedad : objeto responsivo y en este caso pseudo= {{propiedad: objeto}}

