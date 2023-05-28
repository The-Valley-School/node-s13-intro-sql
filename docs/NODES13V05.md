# VIDEO 05 - Prevenir ataque por inyección de SQL

Un ataque por inyección de SQL, o SQL Injection, es una técnica de ciberataque que se utiliza para explotar vulnerabilidades en una aplicación web. El atacante utiliza esta técnica para manipular o controlar las consultas de la base de datos de la aplicación.

En una inyección de SQL, el atacante inserta una sentencia SQL maliciosa en un formulario de entrada de datos o en cualquier otro lugar donde la aplicación pueda esperar una entrada de usuario. Si la aplicación no valida adecuadamente estas entradas, la sentencia SQL maliciosa puede ser ejecutada por la base de datos.

Estos ataques pueden resultar en una variedad de problemas, que incluyen la revelación no autorizada de datos, la modificación o destrucción de datos, e incluso el control completo de la base de datos y, en algunos casos, del servidor en el que se ejecuta.

Para protegerse contra los ataques de inyección de SQL, las aplicaciones web deben utilizar técnicas como la validación de entrada, las consultas parametrizadas y los procedimientos almacenados seguros. También es importante mantener el software de la base de datos y la aplicación web actualizados para asegurar que se apliquen todas las correcciones de seguridad relevantes.

En este video hemos visto como podemos prevenir un ataque de inyección de SQL haciendo uso de los params de la librería mysql2, para ello hemos modificado nuestro controlador para pasar todas las variables como params:

```tsx
import express, { type NextFunction, type Response, type Request } from "express";
import { sqlQuery } from "../databases/sql-db";
import { type ProgrammingLanguageBody } from "../models/sql/ProgrammingLanguage";
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
    const { name, releasedYear, githutRank, pyplRank, tiobeRank } = req.body as ProgrammingLanguageBody;

    const query: string = `
      INSERT INTO programming_languages (name, released_year, githut_rank, pypl_rank, tiobe_rank)
      VALUES (?, ?, ?, ?, ?)
    `;
    const params = [name, releasedYear, githutRank, pyplRank, tiobeRank];

    const result = await sqlQuery(query, params);

    if (result) {
      return res.status(201).json({});
    } else {
      return res.status(500).json({ error: "Language not created" });
    }
  } catch (error) {
    next(error);
  }
});

// CRUD: DELETE
languagesRouter.delete("/:id", async (req: Request, res: Response, next: NextFunction) => {
  try {
    const id = parseInt(req.params.id);
    await sqlQuery(`
      DELETE FROM programming_languages
      WHERE id = ${id}
    `);

    res.json({ message: "Language deleted!" });
  } catch (error) {
    next(error);
  }
});

// CRUD: UPDATE
languagesRouter.put("/:id", async (req: Request, res: Response, next: NextFunction) => {
  try {
    const id = req.params.id;
    const { name, releasedYear, githutRank, pyplRank, tiobeRank } = req.body as ProgrammingLanguageBody;

    const query = `
      UPDATE programming_languages
      SET name = ?, released_year = ?, githut_rank = ?, pypl_rank = ?, tiobe_rank = ?
      WHERE id = ?
    `;
    const params = [name, releasedYear, githutRank, pyplRank, tiobeRank, id];
    await sqlQuery(query, params);

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
```

Y hemos modificado nuestro fichero de utilidades SQL para que la funcion sqlQuery utilice params:

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

export const sqlQuery = async (sqlQuery: string, params?: any[]): Promise<any> => {
  const connection = await sqlConnect();
  const [results] = await connection.execute(sqlQuery, params);
  return results;
};
```

Recuerda que puedes encontrar en este repositorio todo el código que hemos visto durante la sesión:

<https://github.com/The-Valley-School/node-s13-intro-sql>

