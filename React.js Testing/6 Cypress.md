# Cypress

Cypress es un testing framework con el cual podremos crear **tests de integración** y **tests end to end**.

Con ambos vamos a simular que somos un usuario y verificar que la interfaz funcione correctamente. La diferencia es que con los **tests end to end** vamos a asegurar que la aplicación (o una parte de ella) funcione como lo hará en un entorno de producción conectándose a todos los servicios que se conectaría como ser una base de datos (que podrá ser una de testing), pegándole a la API, entre otras cosas permitiéndonos ver cómo responden, en cuanto tiempo lo hacen etc. En los **tests de integración** no utilizamos estos servicios externos y tendremos librerías intermedias que se encargan de interceptar estos requests a APIs o bases de datos.



## Instalación

```
npm install cypress
```

Luego de que tenemos Cypress instalado como dependencia tenemos dos formas de correrlo:

* de forma visual (particularmente útil para obtener un feedback visual cuando estamos creando los tests)
* de forma headless (para cuando tengo un CI/CD)



Para ejecutar los tests corremos

```
npx cypress open
```

También podemos crear un script `"test":"cypress open"`