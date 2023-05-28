# VIDEO 06 - Ejercicio tech companies con SQL

En este vídeo debes partir de la API de libros que venimos usando para los ejercicios. Si no tienes el código puedes utilizar nuestra solución o partir de cero:

<https://github.com/The-Valley-School/node-s12-solution-typescript-api>

Debes añadir un CRUD para manejar un listado de compañías de tecnología. Puedes usar estos datos para rellenar tu base de datos:

```tsx
const companyList = [
  {
    name: "Google",
    foundedYear: 1998,
    employeesNumber: 100000,
    headquarters: "Mountain View, California, United States",
    ceo: "Sundar Pichai",
  },
  {
    name: "Apple",
    foundedYear: 1976,
    employeesNumber: 147000,
    headquarters: "Cupertino, California, United States",
    ceo: "Tim Cook",
  },
  {
    name: "Microsoft",
    foundedYear: 1975,
    employeesNumber: 181000,
    headquarters: "Redmond, Washington, United States",
    ceo: "Satya Nadella",
  },
];
```

Debes hacer uso de SQL, crear una base de datos en db4free y crear una tabla para guardar los datos. Como en cualquier API CRIUD debes crear los métodos:

- GET del listado
- GET de un elemento
- PUT para modificar
- POST para crear
- DELETE para eliminar

¡Mucha suerte!

Recuerda que puedes encontrar en este repositorio todo el código que hemos visto durante la sesión:

<https://github.com/The-Valley-School/node-s13-intro-sql>
