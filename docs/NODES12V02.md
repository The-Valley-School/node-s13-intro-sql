# VIDEO 02 - Migración de index.js y db.js a Typescript

En este vídeo hemos comenzado a migrar distintos ficheros del API que teníamos en Javascript a Typescript, en concreto el fichero inicial que arranca el API (index.ts) y el que realiza la conexión a la base de datos (db.ts)

El fichero index.ts ha quedado así:

```tsx
// const { userRouter } = require("./routes/user.routes.js");
// const { carRouter } = require("./routes/car.routes.js");
// const { brandRouter } = require("./routes/brand.routes.js");
// const { fileUploadRouter } = require("./routes/file-upload.routes.js");
import { type Request, type Response, type NextFunction, type ErrorRequestHandler } from "express";

import express from "express";
import cors from "cors";
import { connect } from "./db";

const main = async (): Promise<void> => {
  // Conexión a la BBDD
  const database = await connect();

  // Configuración del server
  const PORT = 3000;
  const app = express();
  app.use(express.json());
  app.use(express.urlencoded({ extended: false }));
  app.use(
    cors({
      origin: "http://localhost:3000",
    })
  );

  // Rutas
  const router = express.Router();
  router.get("/", (req: Request, res: Response) => {
    res.send(`Esta es la home de nuestra API. Estamos utilizando la BBDD de ${database?.connection?.name as string} `);
  });
  router.get("*", (req: Request, res: Response) => {
    res.status(404).send("Lo sentimos :( No hemos encontrado la página solicitada.");
  });

  // Middlewares de aplicación, por ejemplo middleware de logs en consola
  app.use((req: Request, res: Response, next: NextFunction) => {
    const date = new Date();
    console.log(`Petición de tipo ${req.method} a la url ${req.originalUrl} el ${date.toString()}`);
    next();
  });

  // Usamos las rutas
  // app.use("/user", userRouter);
  // app.use("/car", carRouter);
  // app.use("/brand", brandRouter);
  app.use("/public", express.static("public"));
  // app.use("/file-upload", fileUploadRouter);
  app.use("/", router);

  // Middleware de gestión de errores
  app.use((err: ErrorRequestHandler, req: Request, res: Response, next: NextFunction) => {
    console.log("*** INICIO DE ERROR ***");
    console.log(`PETICIÓN FALLIDA: ${req.method} a la url ${req.originalUrl}`);
    console.log(err);
    console.log("*** FIN DE ERROR ***");

    // Truco para quitar el tipo a una variable
    const errorAsAny: any = err as unknown as any;

    if (err?.name === "ValidationError") {
      res.status(400).json(err);
    } else if (errorAsAny.errmsg?.indexOf("duplicate key") !== -1) {
      res.status(400).json({ error: errorAsAny.errmsg });
    } else {
      res.status(500).json(err);
    }
  });

  app.listen(PORT, () => {
    console.log(`Server levantado en el puerto ${PORT}`);
  });
};

void main();
```

Y el fichero db.ts ha quedado así:

```tsx
// Cargamos variables de entorno
// Importamos librerías
import mongoose from "mongoose";
import dotenv from "dotenv";
dotenv.config();

const DB_CONNECTION: string = process.env.DB_URL as string;
const DB_NAME: string = process.env.DB_NAME as string;

// Configuración de la conexión a Mongo
const config = {
  useNewUrlParser: true,
  useUnifiedTopology: true,
  serverSelectionTimeoutMS: 5000,
  dbName: DB_NAME,
};

export const connect = async (): Promise<mongoose.Mongoose | null> => {
  try {
    const database: mongoose.Mongoose = await mongoose.connect(DB_CONNECTION, config);
    const name = database.connection.name;
    const host = database.connection.host;
    console.log(`Conectado a la base de datos ${name} en el host ${host}`);

    return database;
  } catch (error) {
    console.error(error);
    console.log("Error en la conexión, intentando conectar en 5s...");
    // eslint-disable-next-line @typescript-eslint/no-misused-promises
    setTimeout(connect, 5000);

    return null;
  }
};
```

En el fichero package.json podemos encontrar un comando start para compilar el typescript y levantar la aplicación:

```tsx
"scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint --fix .",
    "start": "npm run build && node ./dist/index.js",
    "build": "tsc",
    "test": "echo \"Error: no test specified\" && exit 1",
    "prepare": "husky install"
},
```

También hemos necesitado instalar muchas librerías de tipos para que Typescript sepa cómo funcionan las librerías que usamos:

```tsx
npm i @types/cors --save-dev
npm i @types/express --save-dev
npm i @types/mongoose --save-dev
```
