# Swagger

> :link: Basado en el [video de TomDoesTech](https://youtu.be/5aryMKiBEKY) - [Repositorio](https://github.com/TomDoesTech/REST-API-Tutorial-Updated)

Swagger o OpenAPI es una herramienta Open Source que nos permite documentar APIs.



# Proyecto

:rotating_light: Muchos de los pasos descriptos a continuación son similares a los apuntes de la sección **Node y TypeScript**.

```bash
mkdir md-swagger
cd md-swagger
npm init -y
```



## Instalación Dependencias

```bash
npm i express
npm i swagger-jsdoc swagger-ui-express

npm i typescript -D
npm i ts-node-dev -D
npm i @types/swagger-jsdoc @types/swagger-ui-express -D
```



## Creación de Scripts

```
"build":"tsc",
```

```
"dev": "ts-node-dev src/app.ts",
```



### Creación de `tsconfig.json`

Ejecutamos `npm run build -- --init`

Luego en `tsconfig.json` dejamos las opciones que vienen descomentadas por default y agregamos las siguientes:

```json
{
  "compilerOptions": {
    "outDir": "build",
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "resolveJsonModule": true
  }
}
```

> `"resolveJsonModule": true` lo colocamos para poder importar `package.json` y utilizar la versión en nuestra documentación generada.



## Estructura del Proyecto

Creamos carpeta `src` con `index.ts`, luego el código compilado estará en la carpeta `build`.

En el directorio `utils` creamos un archivo `swagger.ts`



```tsx
import { Express, Request, Response } from "express";

import swaggerJSDoc from 'swagger-jsdoc';
import swaggerUI from 'swagger-ui-express';
import {version} from '../../package.json';

const options: swaggerJSDoc.Options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'REST API Docs',
      version,
      description: 'Using Swagger in Express Server',
    },
  },
  apis: ['./src/index.ts'],
};

const specs = swaggerJSDoc(options);

const swaggerDocs = (app: Express, port: number) => {
  // Swagger page
  app.use('/docs', swaggerUI.serve, swaggerUI.setup(specs));

  // Docs in JSON format
  app.get('/docs.json', (_req: Request, res: Response) => {
    res.setHeader('Content-Type', 'application/json');
    res.send(specs);
  });

  console.log(`Docs available at http://localhost:${port}/docs`);
};

export default swaggerDocs;
```



En un caso mas complejo donde utilicemos autenticación:

```tsx
const options: swaggerJsdoc.Options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'REST API Docs',
      version,
      description: 'Using Swagger in Express Server',
    },
    components: {
      securitySchemas: {
        bearerAuth: {
          type: 'http',
          scheme: 'bearer',
          bearerFormat: 'JWT'
        }
      }
    },
    security: [
      {
        bearerAuth: []
      }
    ]
  },
  apis:['./src/index.ts']
}
```



Luego en `index.ts` donde implementamos un servidor simple con dos endpoints llamaremos  a `swaggerDocs(app, port)`.

```tsx
import express from 'express';
import swaggerDocs from './utils/swagger';

const app = express();

app.use(express.json());

const PORT = 3000;

app.get('/ping', (_req, res) => {
  res.send('pong');
});

swaggerDocs(app, port);

app.listen(PORT, () => {
  console.log(`Server is listening on port ${PORT}`); 
});
```



A continuación agregamos comentarios antes de cada endpoint de manera tal que nos genere la documentación deseada:

```tsx
import express from 'express';
import swaggerDocs from './utils/swagger';

const app = express();

app.use(express.json());

const PORT = 3000;

/**
   * @openapi
   * /ping:
   *  get:
   *     tags:
   *     - Healthcheck
   *     description: Responds if the app is up and running
   *     responses:
   *       200:
   *         description: App is up and running
   */
app.get('/ping', (_req, res) => {
  res.send('pong');
});

/**
   * @openapi
   * /healthcheck:
   *  get:
   *     tags:
   *     - Healthcheck
   *     description: Responds if the app is up and running
   *     responses:
   *       200:
   *         description: App is up and running
   */
app.get('/healthcheck', (_req, res) => {
  res.sendStatus(200);
});

swaggerDocs(app, PORT);

app.listen(PORT, () => {
  console.log(`Server is listening on port ${PORT}`);
});
```

> Con `tags` agrupamos los endpoints.
>
> Notar que el comentario comienza con doble asterisco `/**` y eso hace que luego se autocompleten los asteriscos cada vez que cambiamos de línea.



> En cuanto a los POST Requests, en el [repositorio](https://github.com/TomDoesTech/REST-API-Tutorial-Updated/tree/main/src) del video puede verse una implementación en la cual además del archivo donde tiene los endpoints cuenta con un archivo con los schemas donde especificamos los campos con los que trabajaremos. Esto es importante ya que para un mismo endpoint tendremos comentarios de Swagger en dos lugares: [1](https://github.com/TomDoesTech/REST-API-Tutorial-Updated/blob/main/src/routes.ts) y [2](https://github.com/TomDoesTech/REST-API-Tutorial-Updated/blob/main/src/schema/user.schema.ts) y es interesante ver como los referenciamos.



## Importar en Postman

Si ingresamos al endpoint docs.json veremos el contenido de la documentación en formato JSON. Luego en Postman podemos ir a Import --> Raw data
