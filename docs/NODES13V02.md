# VIDEO 02 - Primeras consultas SQL

Después de haber creado nuestra primera tabla vamos a insertar algunos datos, en este caso nos una lista de lenguajes de programación, con su fecha de creación y su popularidad según distintas fuentes:

Github:

<https://madnight.github.io/githut>

PYPL:

<https://pypl.github.io/PYPL.html>

Tyobe:

<https://www.tiobe.com/tiobe-index/>

Podemos insertar los datos a mano mediante la interfaz de phpmyadmin o mediante una consulta SQL:

```sql
INSERT INTO programming_languages(id,name,released_year,githut_rank,pypl_rank,tiobe_rank)
VALUES
(1,'JavaScript',1995,1,3,7),
(2,'Python',1991,2,1,3),
(3,'Java',1995,3,2,2),
(4,'TypeScript',2012,7,10,42),
(5,'C#',2000,9,4,5),
(6,'PHP',1995,8,6,8),
(7,'C++',1985,5,5,4),
(8,'C',1972,10,5,1),
(9,'Ruby',1995,6,15,15),
(10,'R',1993,33,7,9),
(11,'Objective-C',1984,18,8,18),
(12,'Swift',2015,16,9,13),
(13,'Kotlin',2011,15,12,40),
(14,'Go',2009,4,13,14),
(15,'Rust',2010,14,16,26),
(16,'Scala',2004,11,17,34);
```

Después hemos hecho algunas consultas básicas sobre nuestra tabla recién creada:

Recuperar la lista de lenguajes de programación: Para obtener todos los lenguajes de programación de la tabla, usarías una consulta SELECT simple:

```sql
SELECT * FROM programming_languages;
```

Recuperar la lista de lenguajes de programación ordenada por la fecha de creación: Si quieres ordenar los resultados por la fecha de creación, puedes agregar una cláusula ORDER BY:

```sql
SELECT * FROM programming_languages ORDER BY released_year;
```

Insertar un nuevo lenguaje de programación: Para insertar un nuevo lenguaje de programación en la tabla, usarías una consulta INSERT. Aquí hay un ejemplo en el que se inserta Python que fue lanzado en 1991:

```sql
INSERT INTO programming_languages (name, released_year, githut_rank, pypl_rank, tiobe_rank)
VALUES ('Inventado', 1991, 2, 1, 3);
```

Modificar un lenguaje de programación existente: Para modificar un lenguaje de programación existente, puedes usar una consulta UPDATE. Supongamos que quieres cambiar el año de lanzamiento de Python a 1989:

```sql
UPDATE programming_languages
SET released_year = 1989
WHERE name = 'Inventado';
```

Eliminar un lenguaje de programación: Para eliminar un lenguaje de programación, puedes usar una consulta DELETE. Por ejemplo, para eliminar Python de la tabla:

```sql
DELETE FROM programming_languages
WHERE name = 'Inventado';
```

Ten cuidado con la consulta DELETE, ya que eliminará permanentemente los registros de la tabla.

Ten en cuenta que SQL es un lenguaje muy extenso y se pueden realizar muchas operaciones de manera manual haciendo queries, puedes encontrar Cheat Sheets con resúmenes de las operaciones aceptadas, por ejemplo en la web de learnsql:

![sql-basics-cheat-sheet-a4-hoja-1.png](/docs/assets/sql-basics-cheat-sheet-a4-hoja-1.png)

![sql-basics-cheat-sheet-a4-hoja-2.png](/docs/assets/sql-basics-cheat-sheet-a4-hoja-2.png)

