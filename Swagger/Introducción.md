# Swagger

> :link: Basado en el [video de TomDoesTech](https://youtu.be/5aryMKiBEKY) - [Repositorio](https://github.com/TomDoesTech/REST-API-Tutorial-Updated)
>
> :link: Basado en el [video de Fazt Code](https://youtu.be/9bO0L1rfkvU) - [Repositorio](https://github.com/FaztWeb/typescript-swagger-lowdb)

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
   * @swagger
   * /ping:
   *  get:
   *     tags:
   *     - Healthcheck
   *     summary: Returns pong
   *     description: Responds if the app is up and running
   *     responses:
   *       200:
   *         description: App is up and running
   */
app.get('/ping', (_req, res) => {
  res.send('pong');
});

/**
   * @swagger
   * /healthcheck:
   *  get:
   *     tags:
   *     - Healthcheck
   *	 summary: Returns OK
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

> Notar que el comentario comienza con doble asterisco `/**` y eso hace que luego se autocompleten los asteriscos cada vez que cambiamos de línea.
>
> Según el autor en algunos casos en lugar de `@swagger` usan `@openapi`.
>
> La indentación la podemos la realizamos primero con un espacio y luego con TABs.
>
> Con `tags` agrupamos los endpoints.



## Schemas Reutilizables

Los componentes en swagger son porciones de código reutilizables, veremos su aplicación a la hora de definir schemas:

```tsx
/**
 * @swagger
 * components:
 *  schemas:
 *    Task:
 *      type: object
 *      properties:
 *        id: 
 *          type: string
 *          description: the auto-generated id
 *        name: 
 *          type: string
 *          description: the name of the task
 *        description:
 *          type: string
 *          description: the description of the task
 *      required:
 *        - name
 *        - description
 *      example:
 *        id: gQBOyGbxcQy6tEp0aZ78X
 *        name: My first Task
 *        description: I have to do Something
 *         
 *
 */
```

Luego podremos importarlo en las rutas que queramos con `$ref: '#/components/schemas/Holiday'`

Por ejemplo en el caso de un GET

```tsx
/**
 * @openapi
 * /tasks:
 *  get:
 *    tags:
 *    - Tasks
 *    summary: Get all tasks
 *    description: Get all tasks
 *    responses:
 *      200:
 *        description: List of tasks
 *        content:
 *          application/json:
 *            schema:
 *              type: array
 *              items:
 *                $ref: '#/components/schemas/Task'
 */
router.get("/tasks", getTasks);
```



> En el [repositorio](https://github.com/TomDoesTech/REST-API-Tutorial-Updated/tree/main/src) del video puede verse una implementación real donde además del archivo con los endpoints cuenta con un archivo con los schemas donde especificamos los campos con los que trabajaremos. Esto es importante ya que para un mismo endpoint tendremos comentarios de Swagger en dos lugares: [1](https://github.com/TomDoesTech/REST-API-Tutorial-Updated/blob/main/src/routes.ts) y [2](https://github.com/TomDoesTech/REST-API-Tutorial-Updated/blob/main/src/schema/user.schema.ts) y es interesante ver como los referenciamos.



Es posible reutilizar  múltiples schemas como vemos a continuación:

```tsx
/**
 * @swagger
 * components:
 *  schemas:
 *    Task:
 *      type: object
 *      properties:
 *        id:
 *          type: string
 *          description: the auto-generated id of task
 *        name:
 *          type: string
 *          description: the name of the task
 *        description:
 *          type: string
 *          description: the description of the task
 *      required:
 *        - name
 *        - description
 *      example:
 *        id: gQBOyGbxcQy6tEp0aZ78X
 *        name: My first Task
 *        description: I have to do Something
 *    TaskNotFound:
 *      type: object
 *      properties:
 *        msg:
 *          type: string
 *          description: A message for the not found task
 *      example:
 *        msg: Task was not found
 *
 */
```



## Tags Reutilizables

Los tags nos permiten agrupar rutas bajo un mismo nombre.  Una forma de hacerlo es con:

```
 *  tags:
 *    - Tasks
 *    summary: Get all tasks
 *    description: Get all task
```



Sin embargo, como esto es algo que se repetirá bastante, podemos definirlo de este modo:

```tsx
/**
 * @swagger
 * tags:
 *  name: Tasks
 *  description: Tasks endpoint
 */
```



Luego podremos reutilizarlo importándolo con `tags: [Tasks]`

```tsx
/**
 * @swagger
 * /tasks:
 *  get:
 *    summary: Returns a list of tasks
 *    tags: [Tasks]
 *    responses:
 *      200:
 *        description: the list of tasks
 *        content:
 *          application/json:
 *            schema:
 *              type: array
 *              items:
 *                $ref: '#/components/schemas/Task'
 */

router.get("/tasks", getTasks);
```



A continuación mostramos como trabajaríamos en un endpoint que devuelve `text/plain`:

```tsx
/**
 * @swagger
 * /tasks/count:
 *  get:
 *    summary: Get a task by Id
 *    tags: [Tasks]
 *    responses:
 *      200:
 *        description: the total number of tasks
 *        content:
 *          text/plain:
 *            schema:
 *              type: integer
 *              example: 15
 *
 */
router.get("/tasks/count", count);
```



## Parámetros Reutilizables

De la misma manera que reutilizamos los schemas es posible reutilizar parámetros que recibe una función.

```tsx
/**
 * @swagger
 * components:
 *  schemas:
 *    Task:
 *      type: object
 *      properties:
 *        id:
 *          type: string
 *          description: the auto-generated id of task
 *        name:
 *          type: string
 *          description: the name of the task
 *        description:
 *          type: string
 *          description: the description of the task
 *      required:
 *        - name
 *        - description
 *      example:
 *        id: gQBOyGbxcQy6tEp0aZ78X
 *        name: My first Task
 *        description: I have to do Something
 *    TaskNotFound:
 *      type: object
 *      properties:
 *        msg:
 *          type: string
 *          description: A message for the not found task
 *      example:
 *        msg: Task was not found
 *
 *  parameters:
 *    taskId:
 *      in: path
 *      name: id
 *      required: true
 *      schema:
 *        type: string
 *      description: the task id
 */
```

Con `in: path` indicamos que el parámetro está en la url. Con `name: id` hacemos referencia al nombre que tiene en el endpoint.

## Endpoint con parámetros

```tsx
/**
 * @swagger
 * /tasks/{id}:
 *  get:
 *    summary: get a task by Id
 *    tags: [Tasks]
 *    parameters:
 *      - $ref: '#/components/parameters/taskId'
 *    responses:
 *      200:
 *        description: The Found Task
 *        content:
 *          application/json:
 *            schema:
 *            $ref: '#/components/schemas/Task'
 *      404:
 *        description: the task was not found
 *        content:
 *          application/json:
 *            schema:
 *              $ref: '#/components/schemas/TaskNotFound'
 */
router.get("/tasks/:id", getTask);
```



## Importar en Postman

Si ingresamos al endpoint docs.json veremos el contenido de la documentación en formato JSON. Luego en Postman podemos ir a Import --> Raw data
