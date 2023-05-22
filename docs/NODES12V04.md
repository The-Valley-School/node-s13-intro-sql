# VIDEO 04 - Migración de isAuth a Typescript

En este vídeo hemos migrado el middleware isAuth a Typescript:

```tsx
import { type NextFunction, type Response } from "express";
import { User } from "../models/User";
import { verifyToken } from "../utils/token";

export const isAuth = async (req: any, res: Response, next: NextFunction): Promise<null> => {
  try {
    const token = req.headers.authorization?.replace("Bearer ", "");

    if (!token) {
      throw new Error("No tienes autorización para realizar esta operación");
    }

    // Descodificamos el token
    const decodedInfo = verifyToken(token);
    const user = await User.findOne({ email: decodedInfo.userEmail }).select("+password");
    if (!user) {
      throw new Error("No tienes autorización para realizar esta operación");
    }

    req.user = user;
    next();

    return null;
  } catch (error) {
    res.status(401).json(error);
    return null;
  }
};

module.exports = { isAuth };
```

Para ello hemos necesitado también del fichero de utils token.ts:

```tsx
// Importamos jwt
import jwt from "jsonwebtoken";
// Cargamos variables de entorno
import dotenv from "dotenv";
dotenv.config();

export const generateToken = (id: string, email: string): string => {
  if (!email || !id) {
    throw new Error("Email or userId missing");
  }

  const payload = {
    userId: id,
    userEmail: email,
  };

  const token = jwt.sign(payload, process.env.JWT_SECRET as string, { expiresIn: "1d" });
  return token;
};

export const verifyToken = (token: string): any => {
  if (!token) {
    throw new Error("Token is missing");
  }

  const result = jwt.verify(token, process.env.JWT_SECRET as string);
  return result;
};
```
