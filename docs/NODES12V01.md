# VIDEO 01 - Estructura inicial y librerías

En este vídeo hemos empezado la estructura inicial de un repositorio que contendrá un proyecto con express, mongo y typescript.

Hemos instalado varias librerías (cors, dotenv, multer, eslint, typescript…)

Al terminar nuestro package.json ha quedado así:

```tsx
{
  "name": "s12-typescript-api",
  "version": "1.0.0",
  "description": "Node api with typescript, express and mongo",
  "main": "index.ts",
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint --fix .",
    "transpile": "tsc",
    "test": "echo \"Error: no test specified\" && exit 1",
    "prepare": "husky install"
  },
  "keywords": [
    "typescript",
    "node",
    "mongo",
    "express"
  ],
  "author": "Fran Linde",
  "license": "ISC",
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^16.0.3",
    "express": "^4.18.2",
    "mongoose": "^7.2.0",
    "multer": "^1.4.5-lts.1"
  },
  "devDependencies": {
    "@typescript-eslint/eslint-plugin": "^5.59.6",
    "eslint": "^8.41.0",
    "eslint-config-standard-with-typescript": "^34.0.1",
    "eslint-plugin-import": "^2.27.5",
    "eslint-plugin-n": "^15.7.0",
    "eslint-plugin-promise": "^6.1.1",
    "husky": "^8.0.0",
    "typescript": "^5.0.4"
  }
}
```
