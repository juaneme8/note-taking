# Hosting para Sitios Estáticos
* Github Pages
* Netlify

# Deploy en Heroku:
[https://www.heroku.com/](https://www.heroku.com/)

## Deploy con GIT
1) [Instalar Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)
```bash
heroku login
heroku create nombreProyecto 
git add .
git commit -m "Mensaje"
git push heroku master
```

`heroku create` crea una app en blanco con un repositorio en blanco y con `git remove -v` veremos heroku.

En caso de errores podemos verlos con `heroku logs --tail`

En el dashboard de Heroku establecemos las variables de entorno en `Settings -> Config Vars`

## Variables de Entorno
Recordar utilizar variables de entorno a la hora de setear el puerto con el cual escucharemos conexiones entrantes. Las mismas las podemos agregar en el Dashboard de Heroku. Así mismo en el código podemos poner:  `process.env.PORT || 3000` con lo cual nos aseguramos que si la variable de entorno no fue definida (por ejemplo en *development environment*, tome el valor `3000`)

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