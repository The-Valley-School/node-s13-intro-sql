# VIDEO 01 - Intro a SQL y db4free

SQL, o Structured Query Language (Lenguaje de Consulta Estructurado), es un lenguaje de programación diseñado específicamente para gestionar y manipular bases de datos relacionales. Este tipo de bases de datos se organiza en tablas y SQL permite al usuario realizar diversas operaciones sobre estos datos, como consultas para recuperar información específica, así como modificaciones para insertar, actualizar o eliminar datos.

**Historia de SQL**

SQL se desarrolló en los años 70 en los laboratorios de IBM. Fue creado por Donald D. Chamberlin y Raymond F. Boyce. El objetivo inicial era manejar y manipular los datos almacenados en el modelo de base de datos relacional de IBM, propuesto por Edgar F. Codd. SQL se estandarizó por primera vez por ANSI en 1986 y luego por la International Organization for Standardization (ISO) en 1987. A lo largo de los años, SQL ha evolucionado y ha habido varias versiones y variantes creadas por diferentes empresas y organizaciones.

**Adopción de SQL en la Industria**

Hoy en día, casi todas las organizaciones, desde empresas emergentes hasta corporaciones multinacionales, usan SQL de alguna forma. Las empresas como Facebook, Google, Amazon, y muchas más, utilizan SQL para acceder, actualizar y analizar los datos que almacenan en sus bases de datos. SQL también es fundamental para las operaciones de muchos servicios gubernamentales, bancos y compañías de seguros.

Además, el análisis de datos se ha vuelto cada vez más importante en la toma de decisiones empresariales, y SQL es una herramienta clave en el arsenal de cualquier científico de datos o analista de negocios.

**Tecnologías de Base de Datos SQL**

Existen múltiples sistemas de gestión de bases de datos relacionales (RDBMS) que implementan SQL, incluyendo MySQL, PostgreSQL, SQLite, Oracle Database y Microsoft SQL Server, entre otros. Cada uno de estos sistemas tiene sus propias características y ventajas, pero todos comparten la base común del lenguaje SQL.

- MySQL: Es uno de los sistemas de base de datos de código abierto más populares. Es conocido por su facilidad de uso, eficiencia y velocidad. Es utilizado por empresas como Facebook y Twitter.
- PostgreSQL: Es un potente sistema de base de datos de código abierto que se destaca por su conformidad con los estándares y su capacidad para manejar tareas complejas.
- SQLite: Es un motor de base de datos que se destaca por su ligereza y por ser autocontenido. No necesita de un servidor para funcionar, los datos se guardan directamente en un archivo.
- Oracle Database: Es un sistema de base de datos comercial ampliamente utilizado que es conocido por su robustez y capacidad para manejar grandes cantidades de datos.
- Microsoft SQL Server: Es un sistema de base de datos empresarial proporcionado por Microsoft, utilizado en muchas aplicaciones empresariales que se ejecutan en entornos Windows.

Como ya explicamos en la primera sesión de Mongo, SQL es una base de datos relacional y basada en tablas. Si quisiéramos almacenar la información de personas y sus coches asociados, tendríamos que hacerlo mediante relaciones entre tablas:

**Ejemplo de Base de datos relacional, por ejemplo SQL:**

Tabla de usuarios:

| ID  | Nombre | Apellido  | Edad | Vehiculo_ID |
| --- | ------ | --------- | ---- | ----------- |
| 1   | Juan   | Perez     | 30   | 1           |
| 2   | Maria  | Rodriguez | 25   | NULL        |
| 3   | Pedro  | Sanchez   | 40   | 2           |

Tabla de vehículos:

| ID  | Marca  | Modelo  | Año  |
| --- | ------ | ------- | ---- |
| 1   | Toyota | Corolla | 2010 |
| 2   | Ford   | Mustang | 2015 |

En este caso, la tabla de usuarios tiene una columna llamada "Vehiculo_ID" que se relaciona con la tabla de vehículos. Esto permite establecer una relación entre un usuario y su vehículo correspondiente.

Para hacer uso de SQL podríamos trabajar instalando en nuestras máquinas alguno de los softwares mencionados, o contratando un servidor en alguno de los proveedores más famosos:

- Amazon Web Services (AWS) RDS: AWS RDS soporta múltiples motores de base de datos SQL, incluyendo MySQL, PostgreSQL, MariaDB, Oracle y SQL Server.
- Google Cloud SQL: Google Cloud SQL soporta MySQL y PostgreSQL.
- Microsoft Azure SQL Database: Microsoft Azure proporciona soluciones SQL Server en su plataforma en la nube.
- Oracle Cloud: Oracle ofrece tanto Oracle Database como MySQL en su plataforma en la nube.
- Heroku Postgres: Heroku ofrece PostgreSQL en su plataforma.

Todos estos servicios suelen ser de pago, o requieren una tarjeta de crédito para crear un servidor aunque se utilice la cuota gratuita inicialmente, por lo que para aprender y “trastear” un poco con SQL vamos a utilizar una alternativa que aunque es menos conocida, sí que ofrece almacenamiento SQL gratuito basado en MySQL:

<https://db4free.net/>

Es importante recalcar que para aplicaciones reales y profesionales se desaconseja usar db4free y contratar el servicio en alguna de las empresas mencionadas previamente.

Después de crear la cuenta en db4free hemos creado una tabla mediante la interfaz de phpmyadmim, aunque también podríamos hacer uso de este comando SQL:

```sql
CREATE TABLE `programming_languages`
(
  `id`            INT(11) NOT NULL auto_increment ,
  `name`          VARCHAR(255) NOT NULL ,
  `released_year` INT NOT NULL ,
  `githut_rank`   INT NULL ,
  `pypl_rank`     INT NULL ,
  `tiobe_rank`    INT NULL ,
  PRIMARY KEY (`id`),
  UNIQUE `idx_name_unique` (`name`(255))
)
engine = innodb charset=utf8mb4 COLLATE utf8mb4_general_ci;
```

