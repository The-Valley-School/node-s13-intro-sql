# VIDEO 03 - SQL con express y Typescript

Express permite la conexión nativa con muchas bases de datos:

<https://expressjs.com/en/guide/database-integration.html>

En concreto existe un driver para Mysql que es la tecnología que tenemos en nuestro db4free:

<https://expressjs.com/en/guide/database-integration.html#mysql>

En este video vamos a partir del código que teníamos realizado en la sesión 12 y añadiremos soporte para mysql, haremos que ambas bases de datos (mongo y sql) convivan.

Nuestro fichero de conexión a SQL ha quedado así:

```tsx
import mysql, { type Connection, type ConnectionOptions } from "mysql2/promise";
import dotenv from "dotenv";
dotenv.config();

const SQL_HOST: string = process.env.SQL_HOST as string;
const SQL_USER: string = process.env.SQL_USER as string;
const SQL_PASSWORD: string = process.env.SQL_PASSWORD as string;
const SQL_DATABASE: string = process.env.SQL_DATABASE as string;

const config: ConnectionOptions = {
  host: SQL_HOST,
  user: SQL_USER,
  password: SQL_PASSWORD,
  database: SQL_DATABASE,
};

export const sqlConnect = async (): Promise<Connection> => {
  const connection: Connection = await mysql.createConnection(config);
  return connection;
};

export const sqlQuery = async (sqlQuery: string): Promise<any> => {
  const connection = await sqlConnect();
  const [results] = await connection.execute(sqlQuery);
  return results;
};
```

Y nuestras rutas de languages así:

```tsx
import express, { type NextFunction, type Response, type Request } from "express";
import { sqlQuery } from "../databases/sql-db";
export const languagesRouter = express.Router();

// CRUD: READ
languagesRouter.get("/", async (req: Request, res: Response, next: NextFunction) => {
  try {
    const rows = await sqlQuery(`
      SELECT *
      FROM programming_languages
    `);
    const response = { data: rows };
    res.json(response);
  } catch (error) {
    next(error);
  }
});

// CRUD: READ
languagesRouter.get("/:id", async (req: Request, res: Response, next: NextFunction) => {
  try {
    const id = req.params.id;
    const rows = await sqlQuery(`
      SELECT *
      FROM programming_languages
      WHERE id=${id}
    `);

    if (rows?.[0]) {
      const response = { data: rows?.[0] };
      res.json(response);
    } else {
      res.status(404).json({ error: "Languange not found" });
    }
  } catch (error) {
    next(error);
  }
});

// CRUD: CREATE
languagesRouter.post("/", async (req: Request, res: Response, next: NextFunction) => {
  try {
    return res.status(201).json({});
  } catch (error) {
    next(error);
  }
});

// CRUD: DELETE
languagesRouter.delete("/:id", async (req: Request, res: Response, next: NextFunction) => {
  try {
    const brandDeleted = null;
    if (brandDeleted) {
      res.json(brandDeleted);
    } else {
      res.status(404).json({});
    }
  } catch (error) {
    next(error);
  }
});

// CRUD: UPDATE
languagesRouter.put("/:id", async (req: Request, res: Response, next: NextFunction) => {
  try {
    const brandUpdated = null;
    if (brandUpdated) {
      res.json(brandUpdated);
    } else {
      res.status(404).json({});
    }
  } catch (error) {
    next(error);
  }
});
```

