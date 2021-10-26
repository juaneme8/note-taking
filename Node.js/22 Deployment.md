# Deploy Node.js

A la hora de deployar microservicios de backend podemos recurrir a alternativas como Heroku o Digital Ocean.

# Heroku

[https://www.heroku.com/](https://www.heroku.com/)

## Deploy con GIT

En primer lugar debemos instalar [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli#download-and-install).

```bash
heroku login

heroku create nombreProyecto 

git add .
git commit -m "Mensaje del commit"
git push origin main
git push heroku main
```

Notar que pusheamos tanto a GitHub como a Heroku.



* Con `heroku create` creamos una app en blanco con un repositorio en blanco
* Con `git remote -v` veremos además  del remote `origin` de GitHub uno nuevo llamado  `heroku`.
* Con `heroku ps:scale web=1` nos aserguramos tener al menos una instancia de la aplicación corriendo.
* Con `heroku logs --tail` visualizamos los logs.
* Con `heroku open` abrimos la aplicación sin tener que ingresar manualmente la URL.

Si el proyecto ya fue deployado no podremos utilizar `heroku create nombreProyecto` para el remote `heroku` por lo que tendremos que hacerlo con:

```
heroku git:remote -a nombreProyecto
```



## Archivo `Procfile`

El archivo `Procfile` ubicado en el directorio raíz para especificar explícitamente qué comando debe ser ejecutado cuando iniciemos la aplicación.

Queremos declarar un proceso único de tipo `web`

```
web: npm start
```


## Variables de Entorno

En el dashboard de Heroku establecemos las variables de entorno en `Settings -> Config Vars`

Por ejemplo a la hora de setear el puerto con el cual escucharemos conexiones entrantes lo seteamos mediante variable de entorno. 



# Deploy Node Application con Vercel

1) Ir a [https://vercel.com/](https://vercel.com/), realizar login con GitHub.

2) `npm install now -g`

3) Ingresar `now` en la consola y nos dirá que no encontró credenciales que debemos loguearnos. Ingresamos el mail y nos enviará un correo en el que debemos hacer click en "Verify". 

> Si obtenemos `Error! The specified token is not valid` ingresar `now login`.

4) Utiliza el start script indicado en `package.json`

5) Desde el directorio raíz ingresamos `now` para el deployment process. Obviamente también admite configuraciones de variables de entorno, uso de `.env`, etc

6) Obtendremos la URL en la consola.

Para obtener la página subida a `https://juanocho.now.sh` el directorio (y el `"name"` en `package.json`) debe ser  juanocho y debemos ejecutar `now --prod` sino lo subirá a `juanocho.juaneme8.now.sh` 

## Importante: 

Esto funcionará para sitios estáticos, pero caso contrario debemos migrar de Serverful APIs (modo de trabajo de Express.js por ejemplo) a Serverless APIs para trabajar con Vercel. Más información en [vercel.com/guides/migrate-to-vercel](https://vercel.com/guides/migrate-to-vercel)



# Deploy Sitios Estáticos

* Github Pages
* Netlify