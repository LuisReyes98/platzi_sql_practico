# Curso practico de SQL

## Breve historia de SQL

### SQL (Structured Query Language - Lenguaje de consulta estructurada)

es un lenguaje que se basó en 2 principios fundamentales:

Teoría de conjuntos.
Álgebra relacional de Edgar Codd (científico informático inglés).
⠀
⠀
SQL fue creada en 1974 por IBM.
Originalmente fue llamado SEQUEL, posteriormente se cambió el nombre por problemas de derechos de autor.

### Relation Company (actualmente con el nombre Oracle)

Creó el software Oracle V2 en 1979.

Mas adelante SQL se convertiría en un lenguaje estándar que unifica todo dentro de las bases de datos relacionales, se convierte en una norma ANSI o ISO.

## Algebra relacional

Álgebra relacional
El álgebra relacional estudia basicamente las operaciones que se pueden realizar entre diversos conjuntos de datos.
⠀

⠀
No confundir las relaciones del álgebra relacional con las relaciones de una base de datos relacional.

Las relaciones de una base de datos es cuando unes dos tablas.
Las relaciones en álgebra relacional se refiere a una tabla.

- La diferencia es conceptual: Las tablas pueden tener tuplas repetidas pero en el álgebra relacional cada relación no tiene un cuerpo, no tiene un primer ni último row.
⠀
Tipos de operadores
Operadores unarios.- Requiere una relación o tabla para funcionar.
- Proyección (π): Equivale al comando Select. Saca un número de columnas o atributos sin necesidad de hacer una unión con una segunda tabla.
π<Nombre, Apellido, Email>(Tabla_Alumno)
⠀
- Selección (σ): Equivale al comando Where. Consiste en el filtrado de de tuplas.
σ<Suscripción=Expert>(Tabla_Alumno)
⠀
Operadores binarios.- Requiere más de una tabla para operar.
- Producto cartesiano (x): Toma todos los elementos de una tabla y los combina con los elementos de la segunda tabla.
Docentes_Quinto_A x Alumnos_Quinto_A
⠀
- Unión (∪): Obtiene los elementos que existen en una de las tablas o en la otra de las tablas.
Alumnos_Quinto_A x Alumnos_Quinto_B
⠀
- Diferencia (-): Obtiene los elementos que existe en una de las tablas pero que no corresponde a la otra tabla.

## Proyección

```sql
SELECT *;
```

```sql
SELECT field AS alias;
```

```sql
SELECT COUNT(id), SUM(quantity), AVG(age);
```

```sql
SELECT MIN(date), MAX(quantity);
```

Estructuras de control

```sql

SELECT IF(500<1000, "YES", "NO");

SELECT OrderID, Quantity
CASE
  WHEN Quantity > 30 THEN "OVER 30"
  WHEN Quantity = 30 THEN "Equal 30"
  ELSE "UNDER 30"
END AS QuantityText

```

#### Notas estudiantes

Proyección significa elegir QUE columnas (o expresiones) la consulta debe retornar.
Selección significa CUALES SON las columnas retornadas.

Supongamos la siguiente consulta:  

```sql
select * Columna1, Columna2, Columna3* from *table_1* where ***n=3***;
```

La proyección sería: Columna1, Columna2 y Columna3 y la parte de Selección es n*=3,* debido a esto es que denominamos a la clausula SELECT como la proyección y los filtros aplicados en la clausula WHERE como Selección.

La Consulta más Básica sería

```sql
SELECT * FROM Tabla1;
```

El astrisco (*) es un comodín que indica que queremos traer una proyección completa (Todos los campos o columnas existentes en la tabla)

Alias
Un alias (..AS…), es otra forma de llamar a una tabla o a una columna, y se utiliza para simplificar las sentencias SQL cuando los nombres de tablas o columnas son largos o complicados.

SELECT Columna1 AS Alias1 FROM Tabla1; (Para referirnos a la Columna1, podremos denominarla como Alias1)

…
.

Funciones de agregación
Las funciones de agregación en SQL nos permiten efectuar operaciones sobre un conjunto de resultados, pero devolviendo un único valor agregado para todos ellos.
Es decir, nos permiten obtener medias, máximos, etc… sobre un conjunto de valores.

Las funciones de agregación básicas que soportan todos los gestores de datos son las siguientes:

COUNT: devuelve el número total de filas seleccionadas por la consulta, como particularidad se puede usar COUNT()* donde contará todos los registros de la tabla incluyendo nulos.
MIN: devuelve el valor mínimo del campo que especifiquemos.
MAX: devuelve el valor máximo del campo que especifiquemos.
SUM: suma los valores del campo que especifiquemos. Sólo se puede utilizar en columnas numéricas.
AVG: devuelve el valor promedio del campo que especifiquemos. Sólo se puede utilizar en columnas numéricas.
.
Función IF()
Función que evalúa una sola expresión y retorna lo que se le especifica en el caso que sea Verdadera o Falsa

IF (expresion, resultado_true, resultado_else)
Función CASE()
Sirve para evaluar una lista de condiciones y retornar uno o múltiples posibles resultados.

Comienza con la sentencia CASE, luego evalua expresiones comenzando con WHEN y en caso que sea verdadera, devolverá el resultado especificado para esa condición luego del THEN…
.
La sentencia ELSE es opcional y devolverá este valor en caso que todas las condiciones WHEN anteriores sean Falsas.

Si todas las condiciones son falsas, y no existe la clausula ELSE, se devolverá NULL.

```
CASE 
   WHEN eval_1 THEN resultado_1
   WHEN eval_2 THEN resultado_2
      ...
   WHEN value_n THEN resultado_n
   ELSE resultado
END AS ValorResultado;
```

## Origen (FROM)

From de una tabla

```sql
SELECT * FROM tabla
```

From multiples tablas

```sql
SELECT *
FROM tabla_diaria AS td
  JOIN tabla_mensual AS tm
  ON td.pk = tm.fk
```

From multiples bases de datos

```SQL
-- dblink es una funcion de postgres
SELECT *
FROM dblink('
  dbname=somedb
  port=5432 host=someserver
  user=someuser
  password=somepwd',
  'SELECT gid, area, perimeter,
          state, county,
          tract, blockgroup,
')
AS blockgroups
```

### Notas Estudiantes

Con SELECT se especifica que columnas queremos obtener de una tabla determinada y con FROM se indica de donde se va a obtener la información que se va proyectar con SELECT. FROM va después de SELECT

```sql
SELECT *
FROM base_de_datos.tabla
```

En la sentencia anterior el manejador de base de datos (DBMS) va al esquema y proyecta lo solicitado.

Las sentencias SQL no son sensibles a minúsculas o mayúsculas pero se recomienda escribir las palabras claves en MAÝUSCULAS y el resto en minúsculas

JOIN es un complemento de FROM.

También se puede obtener la información de una base de datos remota, es decir que el esquema de donde que queremos obtener información se encuentra en otro DBMS.

Para obtener información de una tabla que se encuentra remotamente se utiliza la función dblink, dicha función recibe dos parámetros:

configuración de conexión al DBMS remoto
consulta SQL

Ejemplo de dblink

```sql
SELECT *
FROM dblink('
  dbname=somedb
  port=5432 host=someserver
  user=someuser
  password=somepwd',
  'SELECT gid, area, parimeter,
      state, country,
      tract, blockgroup,
      block, the_geom
  FROM massgis.cens2000blocks')
  AS blockgroups
```

## Productos cartesianos (JOIN)

Un join puede ser un productocartesiano lo cual uniria todos los elementos de una tabla con todos los elementos de la otra

por lo cual se debe tener cuidado ya que puede volverse caotico al relacionar todos con todos ademas de que mientras mas relaciones se agreguen la duracion se vuelve exponencial

```sql
SELECT *
FROM tabla_diaria AS td
  JOIN tabla_mensual AS tm
  ON td.pk = tm.fk
```

## Selección (WHERE)

WHERE es usado para filtrar registros.
WHERE es cuando para extraer solamente las condiciones que cumplen con esa condición.

```SQL
SELECT *
FROM tabla_diaria
WHERE id=1;

SELECT *
FROM tabla_diaria
WHERE cantidad>10;

SELECT *
FROM tabla_diaria
WHERE cantidad<100;

```

Este puede ser combinado con AND ,OR y NOT.

AND y OR son usados para filtrar registros de más de una condición.

AND muestra un registro si todas las condiciones separadas por AND son TRUE.
OR muestra un registro si alguna de las condiciones separadas por OR son TRUE.

```SQL
SELECT *
FROM tabla_diaria
WHERE cantidad > 10
  AND cantidad < 100;

SELECT *
FROM tabla_diaria
WHERE cantidad BETWEEN 10
  AND cantidad < 100;
BETWEEN puede ser usado para definir límites.

La separación por paréntesis es muy importante.

-- Trae todos los Israel Vázquez y todos los Israel López
SELECT *
FROM users
WHERE name = "Israel"
  AND (
  lastname = "Vázquez"
  OR
  lastname = "López"
);


-- Trae todos los Israel Vázquez y los que se apelliden López
SELECT *
FROM users
WHERE name = "Israel"
  AND
  lastname = "Vázquez"
  OR
  lastname = "López";
```

En el primero va a devolver todos los que son Israel Vázquez o Israel López, en el segundo devolverá a todos los que se llaman Israel Vázquez o se apellida López(sólo apellido).

NOT valida que un dato no sea TRUE.

SELECT column1, column2, ...
FROM table_name
WHERE NOT condition;
Para especificar patrones en una columna usamos LIKE. Podemos mostrar diferentes cosas que buscamos.

```SQL
-- que inicie con 'Is'
SELECT *
FROM users
WHERE name LIKE "Is%";

-- que inicie con 'Is'tenga una letra y siga'ael'
SELECT *
FROM users
WHERE name LIKE "Is_ael";

SELECT *
FROM users
WHERE name NOT LIKE "Is_ael";

```

En el primero, colocamos un % para representar que se va a buscar lo que tenga is% pero que no importa los carácteres después de %.
En el segundo le estamos diciendo con _ que puede haber lo que sea en medio de Is y ael.
Y en el último le decimos que ponga todas las filas que no sean igual a lo que arriba estabamos buscando.
Igual podemos decir que nos traiga registros que estén vacíos o que no lo estén.

```SQL
SELECT *
FROM users
WHERE name IS NULL;

SELECT *
FROM users
WHERE name IS NOT NULL;
Y para seleccionar filas con datos específicos, usamos IN.

SELECT *
FROM users
WHERE name IN ('Israel','Laura','Luis');
```

## Ordenamiento (ORDER BY)

Ordenamiento ORDER BY
ORDER BY se usa para ordenar de forma ascendente o descendente columnas en base a una fila determinada. Por defecto lo hace de forma DESC. Organiza todo alfabéticamente o numéricamente si son números.

```SQL
-- por defecto es ASC
SELECT *
FROM tabla_diaria
ORDER BY fecha;

SELECT *
FROM tabla_diaria
ORDER BY fecha ASC;

SELECT *
FROM tabla_diaria
ORDER BY fecha DESC;
```

### Resumen de la clase

Las partes más importantes y más usadas de los queries de SQL son:
-La proyección (con SELECT)
-El origen de los datos (Con FROM y sus JOIN)
-WHERE, que nos ayuda a filtrar las tuplas dependiendo de las condiciones que se cumplan.

Hay otras partes de los queries que son opcionales, que no encontraremos en todos los queries pero que igual juegan un papel muy importantes. Una de ellas es el ordenamiento (ORDER BY)

#### INDICES

excelentes para búsquedas y ordenamientos, cuando estamos haciendo queries complejos, cuando estamos tratando de extraer información constantemente, haciendo joins complicados a través de esos campos y nos sirve tener un índice que guarde el orden de ese campo en particular para hacer extracciones muy rápidas.

##### CUIDAR PARA ALTA TRANSACCIONALIDAD

la contrapartida de esto es que cuando tienes un índice, la parte de escritura, cuando llegas a hacer un Update de esa columna en particular en la que pusiste un índice, entonces cada escritura tardará un poco más que la anterior porque básicamente agrega un nuevo elemento, revisa todos elementos anteriores y posteriores y vuelve a ordenarlos en el índice y si agregamos otro elemento vuelve a revisar todos y a ordenarlos.

No debemos poner índices por poner sino ser muy cuidadosos de aquellas columnas que hacemos JOIN muy seguido o que tienen muchísimos datos y siempre estamos ordenando por esa columna. Es válido ponerlo pero siempre teniendo en cuenta que cuando tenemos que ingerir muchos datos o hacer muchos INSERT no es bueno tener índices en esas tablas (o tener la menor cantidad de índices posible). Es importante tener en mente cuántas lecturas Vs cuántas escrituras haces en una tabla o en un campo particular para decidir si conviene poner un índice. Si vas a hacer muchas escrituras por segundo no es conveniente poner índices o tratar de mantenerlo al mínimo. En cambio, si realmente escribes muy poco en esa tabla pero haces búsquedas constantes y muchos Joins sobre esa tabla entonces vale la pena usar el índice sobre ese campo en particular donde haces el Join o donde haces el ordenamiento porque es un gran complemento para nuestra cláusula ORDER BY.

Si una tabla escribe mucho por segundo un indice no es recomendado

Un indice vale la pena utilizarse cuando se lee mucho en una tablay se escribe poco en ella, ademas de considerar cual es la columna por la cual se va a indexar.

## Agregación y limitantes (GROUP BY y LIMIT)

Agregación y limitantes GROUP BYyLIMIT(SELECT TOP porque no existe LIMIT en SQL)

GROUP BY es una sentencia que agrupa filas que tienen el mismo valor en columnas con el sumatorio.
Como decirle ‘encuentra el número de clientes en cada país’.

Suele usarse frecuentemente con las funciones COUNT MAX MIN MAX SUM AVG a un grupo de una o más columnas.

```sql
SELECT *
FROM tabla_diaria
GROUP BY marca;

SELECT *
FROM tabla_diaria;
GROUP BY marca, modelo;

SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
ORDER BY COUNT(CustomerID) DESC;
```

Limit

```sql
SELECT *
FROM tabla_diaria
LIMIT 1500;

-- OFFSET te permite saltar un numero de elementos
-- esto es ultimo para realizar sentencias de paginacion
SELECT *
FROM tabla_diaria
OFFSET 1500
LIMIT 1500;
```

### SELECT TOP

En SQL Sever no existe el comando limit, en cambio se usa el comando TOP que va despues del SELECT

SELECT TOP es usada para especificar el número de registros que se van a retornar.

SELECT TOP es útil para tablas con miles de registros, pues un gran número de registros puede afectar el rendimiento.

```sql
SELECT TOP number
FROM table1
WHERE condition;

SELECT TOP 1500
FROM tabla_diaria;
```

## Ejercicios

## El primero

Extraer el primer registro de una tabla

```SQL
-- tomando el primer registro especifico
SELECT *
FROM alumnos
FETCH FIRST 1 ROWS ONLY;

-- limit 1 ya que se ordenan por defecto en el orden de guardado
SELECT *
FROM alumnos
LIMIT 1;

-- se agrega la columna row_id para cada row en base a su orden y entonces se pide el primero
SELECT *
FROM (
  SELECT ROW_NUMBER() OVER() AS row_id, *
  FROM alumnos
) AS alumnos_with_row_num
WHERE row_id = 1;
```

Reto Extraer los primeros 5 registros de la tabla usando los metodos anteriores

```SQL
SELECT *
FROM alumnos
FETCH FIRST 5 ROWS ONLY;

SELECT *
FROM alumnos
LIMIT 5;

SELECT *
FROM (
  SELECT ROW_NUMBER() OVER() AS row_id, *
  FROM alumnos
) AS alumnos_with_row_num
WHERE row_id BETWEEN 1 AND 5;
```

## El segundo más alto

Buscar la segunda colegiatura mas alta de todos los alumnos

```sql
SELECT DISTINCT colegiatura
FROM alumnos AS a1
WHERE 2 = (
  SELECT COUNT( DISTINCT colegiatura) FROM alumnos AS a2
  WHERE a1.colegiatura <= a2.colegiatura
);

SELECT DISTINCT colegiatura
FROM alumnos
ORDER BY colegiatura DESC
LIMIT 1 OFFSET 1;

-- para un tutor especifico
SELECT DISTINCT colegiatura
FROM alumnos
WHERE tutor_id = 20
ORDER BY colegiatura DESC
LIMIT 1 OFFSET 1;

-- todos los alumnos que coinciden con la colegiatura del segundo quey
SELECT *
FROM alumnos AS datos_alumnos
INNER JOIN (
  SELECT DISTINCT colegiatura
  FROM alumnos
  WHERE tutor_id = 20
  ORDER BY colegiatura DESC
  LIMIT 1 OFFSET 1
) AS segunda_mayor_colegiatura
ON datos_alumnos.colegiatura = segunda_mayor_colegiatura.colegiatura;

SELECT *
FROM alumnos AS datos_alumnos
WHERE colegiatura = (
  SELECT DISTINCT colegiatura
  FROM alumnos
  WHERE tutor_id = 20
  ORDER BY colegiatura DESC
  LIMIT 1 OFFSET 1
);

```

Reto traer la segunda mitad de los registros de la tabla de alumnos

```sql
-- total de registros
SELECT COUNT(id) FROM alumnos;

-- la mitad de los registros
SELECT COUNT(id)/2 AS num FROM alumnos;


-- Trae la segunda mitad si es del ultimo a al medio
SELECT *
FROM alumnos
ORDER BY id DESC
LIMIT (SELECT COUNT(id)/2 AS num FROM alumnos);

-- Trae la segunda mitad de los registros
-- del medio al ultimo
SELECT *
FROM alumnos
ORDER BY id ASC
LIMIT (SELECT COUNT(id)/2 AS num FROM alumnos)
OFFSET (SELECT COUNT(id)/2 AS num FROM alumnos);

SELECT *
FROM alumnos
ORDER BY id ASC
OFFSET (SELECT COUNT(id)/2 AS num FROM alumnos);
```

```sql
SELECT ROW_NUMBER() OVER() AS row_id, *
FROM alumnos
OFFSET(
  SELECT COUNT(*) / 2
  FROM alumnos
);
```

## Seleccionar de un set de opciones

```sql
SELECT *
FROM (
  SELECT ROW_NUMBER() OVER() AS row_id, *
  FROM alumnos
) AS alumnos_with_row_num
WHERE row_id IN (1,5,10,15,20);

SELECT *
FROM alumnos
WHERE id IN (
  SELECT id
  FROM alumnos
  WHERE tutor_id = 30
);

SELECT *
FROM alumnos
WHERE id IN (
  SELECT id
  FROM alumnos
  WHERE tutor_id = 30
    AND carrera_id = 31
);
```

Reto seleccionar valores de los alumnos que no se encuentran en la lista

```sql
SELECT *
FROM alumnos
WHERE id NOT IN (
  SELECT id
  FROM alumnos
  WHERE tutor_id = 30
);
```

## En mis tiempos

```sql
-- el EXTRACT parace ser mas performante que el DATE_PART

SELECT EXTRACT(YEAR FROM fecha_incorporacion) AS year_incorporation
FROM alumnos;


SELECT DATE_PART('YEAR', fecha_incorporacion) AS year_incorporation
FROM alumnos;

SELECT DATE_PART('YEAR', fecha_incorporacion) AS year_incorporation,
  DATE_PART('MONTH', fecha_incorporacion) AS month_incorporation,
  DATE_PART('DAY', fecha_incorporacion) AS day_incorporation
FROM alumnos;
```

Reto agregar hora, minutos y segundos a las queries anteriores

```sql


SELECT DATE_PART('YEAR', fecha_incorporacion) AS year_incorporation,
  DATE_PART('MONTH', fecha_incorporacion) AS month_incorporation,
  DATE_PART('DAY', fecha_incorporacion) AS day_incorporation,
  DATE_PART('HOUR', fecha_incorporacion) AS hour_incorporation,
  DATE_PART('MINUTE', fecha_incorporacion) AS minute_incorporation,
  DATE_PART('SECOND', fecha_incorporacion) AS second_incorporation
FROM alumnos;

SELECT EXTRACT(YEAR FROM fecha_incorporacion) AS year_incorporation,
  EXTRACT(MONTH FROM fecha_incorporacion) AS month_incorporation,
  EXTRACT(DAY FROM fecha_incorporacion) AS day_incorporation,
  EXTRACT(HOUR FROM fecha_incorporacion) AS hour_incorporation,
  EXTRACT(MINUTE FROM fecha_incorporacion) AS minute_incorporation,
  EXTRACT(SECOND FROM fecha_incorporacion) AS second_incorporation
FROM alumnos;
```

## Seleccionar por año

```SQL
SELECT *
FROM alumnos
WHERE (EXTRACT(YEAR FROM fecha_incorporacion)) = 2019;


SELECT *
FROM alumnos
WHERE (DATE_PART('YEAR', fecha_incorporacion)) = 2018;

SELECT *
FROM (
  SELECT *,
    DATE_PART('YEAR', fecha_incorporacion) as year_incorporation
  FROM alumnos
) as alumnos_con_year
WHERE year_incorporation = 2020;
```

Reto extraer los alumnos que se incorporaron en un año y un mes especifico

```sql
-- MAYO 2018
SELECT *
FROM alumnos
WHERE (DATE_PART('YEAR', fecha_incorporacion)) = 2018
  AND (DATE_PART('MONTH', fecha_incorporacion)) = 5;
```

## Duplicados

```sql
-- duplicado en base al id
SELECT *
FROM alumnos as ou
WHERE (
  SELECT COUNT(*)
  FROM alumnos AS inr
  WHERE ou.id = inr.id
) > 1;

-- contea la combinacion de todos los campos separados por comas para saber cuantos valores duplicados hay, aunque como se considera el id tampoco muestra los duplicados con id diferente
SELECT (alumnos.*)::text, COUNT(*)
FROM alumnos
GROUP BY alumnos.*
HAVING COUNT(*) > 1;


-- verificando los duplicados correctamente ya que evalua todos los campos menos el id
SELECT (
  alumnos.nombre,
  alumnos.apellido,
  alumnos.email,
  alumnos.colegiatura,
  alumnos.fecha_incorporacion,
  alumnos.carrera_id,
  alumnos.tutor_id
)::text, COUNT(*)
FROM alumnos
GROUP BY alumnos.nombre,
  alumnos.apellido,
  alumnos.email,
  alumnos.colegiatura,
  alumnos.fecha_incorporacion,
  alumnos.carrera_id,
  alumnos.tutor_id
HAVING COUNT(*) > 1;


SELECT *
FROM (
  SELECT id,
  ROW_NUMBER() OVER(
    PARTITION BY
      nombre,
      apellido,
      email,
      colegiatura,
      fecha_incorporacion,
      carrera_id,
      tutor_id
    ORDER BY id ASC
    ) AS row,
    *
    FROM alumnos
  ) AS duplicados
WHERE duplicados.row > 1
```

Reto usando la query anterior borrar los registros Duplicados

```sql
-- CANTIDAD DE Duplicados menos 1
SELECT COUNT(*) - 1 as num FROM alumnos WHERE id IN (SELECT id
  FROM (
    SELECT
    ROW_NUMBER() OVER(
    PARTITION BY
      nombre,
      apellido,
      email,
      colegiatura,
      fecha_incorporacion,
      carrera_id,
      tutor_id
    ORDER BY id ASC
    ) AS row,
    *
    FROM alumnos
    ) AS duplicados
  WHERE duplicados.row > 1
)

-- borrando todos los duplicados
DROP FROM alumnos WHERE id IN (SELECT id
FROM (
  SELECT
  ROW_NUMBER() OVER(
    PARTITION BY
      nombre,
      apellido,
      email,
      colegiatura,
      fecha_incorporacion,
      carrera_id,
      tutor_id
    ORDER BY id ASC
    ) AS row,
    *
    FROM alumnos
  ) AS duplicados
WHERE duplicados.row > 1;


-- borrando todos los de id null
DELETE FROM alumnos WHERE id IS NULL;

-- TODOS los ids de los duplicados menos 1
SELECT id FROM alumnos WHERE id IN (SELECT id
  FROM (
    SELECT
    ROW_NUMBER() OVER(
    PARTITION BY
      nombre,
      apellido,
      email,
      colegiatura,
      fecha_incorporacion,
      carrera_id,
      tutor_id
    ORDER BY id ASC
    ) AS row,
    *
    FROM alumnos
    ) AS duplicados
  WHERE duplicados.row > 1
) LIMIT (SELECT COUNT(*) - 1 as num
  FROM alumnos
  WHERE id IN (SELECT id
  FROM (
    SELECT
    ROW_NUMBER() OVER(
    PARTITION BY
      nombre,
      apellido,
      email,
      colegiatura,
      fecha_incorporacion,
      carrera_id,
      tutor_id
    ORDER BY id ASC
    ) AS row,
    *
    FROM alumnos
    ) AS duplicados
  WHERE duplicados.row > 1
))

-- borrando todos los duplicados menos 1 es decir el original
DELETE FROM alumnos WHERE ctid IN (
  SELECT ctid
FROM alumnos
WHERE id IN (SELECT id
  FROM (
    SELECT
    ROW_NUMBER() OVER(
    PARTITION BY
      nombre,
      apellido,
      email,
      colegiatura,
      fecha_incorporacion,
      carrera_id,
      tutor_id
    ORDER BY id ASC
    ) AS row,
    *
    FROM alumnos
    ) AS duplicados
  WHERE duplicados.row > 1
) LIMIT (SELECT COUNT(*) - 1 as num
  FROM alumnos
  WHERE id IN (SELECT id
  FROM (
    SELECT
    ROW_NUMBER() OVER(
    PARTITION BY
      nombre,
      apellido,
      email,
      colegiatura,
      fecha_incorporacion,
      carrera_id,
      tutor_id
    ORDER BY id ASC
    ) AS row,
    *
    FROM alumnos
    ) AS duplicados
  WHERE duplicados.row > 1
))
)
-- nota ctid es un identificador transaccional de postgreSQL cada fila posee uno unico pero cambia tras una transaccion o actulizacion, funciona para identificar filas durante una transaccion pero para identificacion logica y a largo plazo se deben usar los ids

```
