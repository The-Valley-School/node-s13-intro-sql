# VIDEO 05 - Typescript con live reload

Por último hemos visto la librería ts-node-dev. n resumen, la librería ts-node-dev es una herramienta útil en el desarrollo con TypeScript, ya que permite ejecutar y reiniciar automáticamente archivos TypeScript sin necesidad de compilar manualmente. Esto agiliza el proceso de desarrollo, facilita la depuración y mejora la productividad del desarrollador.

Para instalarla debemos ejecutar:

```tsx
npm i ts-node-dev --save-dev
```

Y debemos crear un comando en el package.json para ejecutarla en desarrollo (start) y otro para compilar y levantar la app en producción (start:pro):

```tsx
"start": "ts-node-dev ./src/index.ts",
"start:pro": "npm run build && node ./dist/index.js",
```

Recuerda que puedes encontrar en este repositorio todo el código que hemos visto durante la sesión:

<https://github.com/The-Valley-School/node-s12-typescript-api>
