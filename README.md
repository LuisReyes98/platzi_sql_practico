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

Funciones de Agregación y limitantes GROUP BY y LIMIT(SELECT TOP porque no existe LIMIT en MySQL)

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

## Selectores de rango

```SQL
SELECT *
FROM alumnos
WHERE tutor_id IN (1,2,3,4);

SELECT * FROM alumnos
WHERE tutor_id >= 1
  AND tutor_id <= 10;

SELECT *
FROM alumnos
WHERE tutor_id BETWEEN 1 AND 10;

--  rango de 10 a 20 , se encuentra el 3 ? booleano
SELECT int4range(10,20) @> 3;

-- se solapan los rangos? booleano
SELECT numrange(11.1, 22.2) && numrange(20.0, 30.0);

-- el valor mayor del rango
SELECT UPPER(int8range(15,25));

-- El valor menor del rango
SELECT LOWER(int8range(15,25));

-- retorna un rango conformado por los limites superior e inferior de en donde los rangos se intersectan
SELECT int4range(10, 20) * int4range(15,25);

-- True si el rango esta vacio
SELECT ISEMPTY(numrange(1,5));

-- los tutor_id cuyos valor este entre el rango de 10 y 20
SELECT *
FROM alumnos
WHERE int4range(10,20) @> tutor_id
```

RETO, interseccion entre los ids de tutores y los id de carreras

```SQL
SELECT NUMRANGE(
  (SELECT MIN(tutor_id) FROM alumnos),
  (SELECT MAX(tutor_id) FROM alumnos)
) * NUMRANGE(
  (SELECT MIN(carrera_id) FROM alumnos),
  (SELECT MAX(carrera_id) FROM alumnos)
)
```

## Eres lo máximo

```SQL

-- fecha de incorporacion mas reciente
SELECT fecha_incorporacion
FROM alumnos
ORDER BY fecha_incorporacion DESC
LIMIT 1

-- al agrupar con las carreras trae todos los registros lo cual no es util
SELECT carrera_id, fecha_incorporacion
FROM alumnos
GROUP BY carrera_id, fecha_incorporacion
ORDER BY fecha_incorporacion DESC;

-- agrupando por carreras con la fecha de incorporacion mas reciente
SELECT carrera_id,
  MAX(fecha_incorporacion)
FROM alumnos
GROUP BY carrera_id
ORDER BY carrera_id;
```

Reto traer el minimo nombre alfabeticamente

```sql
SELECT nombre
FROM alumnos
ORDER BY nombre ASC
LIMIT 1;

SELECT MIN(nombre)
FROM alumnos;
```

y el minimo por id de tutor

```sql
SELECT tutor_id, MIN(nombre)
FROM alumnos
GROUP BY tutor_id;

SELECT tutor_id, MIN(nombre)
FROM alumnos
GROUP BY tutor_id
ORDER BY tutor_id;
```

## Egoísta (selfish)

```sql
-- Self join
SELECT a.nombre,
  a.apellido,
  t.nombre,
  t.apellido
FROM alumnos AS a
  INNER JOIN alumnos AS t
  ON a.tutor_id = t.id;

SELECT CONCAT(a.nombre, ' ', a.apellido) AS alumno,
  CONCAT(t.nombre, t.apellido) AS tutor
FROM alumnos AS a
  INNER JOIN alumnos AS t
  ON a.tutor_id = t.id;

SELECT CONCAT(t.nombre, t.apellido) AS tutor,
  COUNT(*) AS alumnos_por_tutor
FROM alumnos AS a
  INNER JOIN alumnos AS t
  ON a.tutor_id = t.id
GROUP BY tutor
ORDER BY alumnos_por_tutor DESC;

-- TOP 10
SELECT CONCAT(t.nombre, t.apellido) AS tutor,
  COUNT(*) AS alumnos_por_tutor
FROM alumnos AS a
  INNER JOIN alumnos AS t
  ON a.tutor_id = t.id
GROUP BY tutor
ORDER BY alumnos_por_tutor DESC
LIMIT 10;
```

Reto promedio de alumnos por tutor

```sql
SELECT AVG(alumnos_por_tutor) AS promedio_alumnos_por_tutor FROM
(SELECT CONCAT(t.nombre, t.apellido) AS tutor,
  COUNT(*) AS alumnos_por_tutor
FROM alumnos AS a
  INNER JOIN alumnos AS t
  ON a.tutor_id = t.id
GROUP BY tutor
ORDER BY alumnos_por_tutor DESC) AS promedio;
```

## Resolviendo diferencias

```sql
-- alumnos por carreras
SELECT carrera_id, COUNT(*) AS cuenta
FROM alumnos
GROUP BY carrera_id
ORDER BY cuenta DESC;

-- left join exclusive
-- todos los alumnos cuya carrera ya no existe
SELECT 	a.nombre,
    a.apellido,
    a.carrera_id,
    c.id,
    c.carrera
FROM alumnos AS a
  LEFT JOIN carreras AS c
  ON a.carrera_id = c.id
WHERE c.id IS NULL
ORDER BY a.carrera_id;
```

RETO realizar el left join mostrando los datos de toda la tabla alumnos

```sql

-- todos los alumnos con carrera y el nombre de sus carreras
SELECT a.nombre,
    a.apellido,
    a.carrera_id,
    c.id,
    c.carrera
FROM alumnos AS a
  INNER JOIN carreras AS c
  ON a.carrera_id = c.id
ORDER BY a.carrera_id;

-- todos los alumnos con y sin carrera y sus carreras
SELECT a.nombre,
    a.apellido,
    a.carrera_id,
    c.id,
    c.carrera
FROM alumnos AS a
  LEFT JOIN carreras AS c
  ON a.carrera_id = c.id
ORDER BY a.carrera_id;

-- todos los alumnos con y sin carrera y todas las carreras incluso las que no tienen alumnos
SELECT a.nombre,
    a.apellido,
    a.carrera_id,
    c.id,
    c.carrera
FROM alumnos AS a
  FULL OUTER JOIN carreras AS c
  ON a.carrera_id = c.id
ORDER BY a.carrera_id;
```

## Todas las uniones

```sql

-- FULL OUTER JOIN
SELECT a.nombre,
  a.apellido,
  a.carrera_id,
  c.id,
  c.carrera
FROM alumnos AS a
  FULL OUTER JOIN carreras AS c
  ON a.carrera_id = c.id
ORDER BY c.id DESC;

-- INNER JOIN
SELECT a.nombre,
  a.apellido,
  a.carrera_id,
  c.id,
  c.carrera
FROM alumnos AS a
  INNER JOIN carreras AS c
  ON a.carrera_id = c.id
ORDER BY c.id DESC;

-- LEFT JOIN
SELECT a.nombre,
  a.apellido,
  a.carrera_id,
  c.id,
  c.carrera
FROM alumnos AS a
  LEFT JOIN carreras AS c
  ON a.carrera_id = c.id
ORDER BY c.id DESC;

-- LEFT JOIN EXCLUSIVO
SELECT a.nombre,
  a.apellido,
  a.carrera_id,
  c.id,
  c.carrera
FROM alumnos AS a
  LEFT JOIN carreras AS c
  ON a.carrera_id = c.id
WHERE c.id IS NULL;

-- right join
SELECT a.nombre,
  a.apellido,
  a.carrera_id,
  c.id,
  c.carrera
FROM alumnos AS a
  RIGHT JOIN carreras AS c
  ON a.carrera_id = c.id
ORDER BY c.id DESC;

-- right join exclusivo
SELECT a.nombre,
  a.apellido,
  a.carrera_id,
  c.id,
  c.carrera
FROM alumnos AS a
  RIGHT JOIN carreras AS c
  ON a.carrera_id = c.id
WHERE a.id IS NULL
ORDER BY c.id DESC;

-- diferencia simetrica
SELECT a.nombre,
  a.apellido,
  a.carrera_id,
  c.id,
  c.carrera
FROM alumnos AS a
  FULL OUTER JOIN carreras AS c
  ON a.carrera_id = c.id
WHERE a.id IS NULL
 OR c.id IS NULL
ORDER BY a.carrera_id DESC
  ,c.id DESC;
```

## Triangulando

```sql
-- da un padding left que contenga el primer string, con n de largo del string
-- y los espacios faltantes los rellena con '*'
SELECT LPAD('sql', 15, '*');

SELECT LPAD('*', id, '*')
FROM alumnos
WHERE id < 10;

SELECT LPAD('*', id, '*')
FROM alumnos
LIMIT 100;

SELECT LPAD('*', id, '*')
FROM alumnos
WHERE id < 10
ORDER BY carrera_id;


SELECT LPAD('*', CAST(row_id AS int), '*')
FROM (
  SELECT ROW_NUMBER() OVER() AS row_id, *
  FROM alumnos
) AS alumnos_with_row_id
WHERE row_id <= 5;

SELECT LPAD('h', CAST(row_id AS int), '*')
FROM (
  SELECT ROW_NUMBER() OVER(ORDER BY carrera_id) AS row_id, *
  FROM alumnos
) AS alumnos_with_row_id
WHERE row_id <= 5
ORDER BY carrera_id;


SELECT RPAD('h', CAST(row_id AS int), '*')
FROM (
  SELECT ROW_NUMBER() OVER(ORDER BY carrera_id) AS row_id, *
  FROM alumnos
) AS alumnos_with_row_id
WHERE row_id <= 5
ORDER BY carrera_id;
```

## Generando rangos

```sql
-- SERIE QUE VA DEL 1 AL 4
SELECT *
FROM GENERATE_SERIES(1,4)


SELECT *
FROM GENERATE_SERIES(1,4);

SELECT *
FROM GENERATE_SERIES(5,4);

-- GENERATE_SERIES(Inicio , final , paso)
SELECT *
FROM GENERATE_SERIES(5,1, -2);



SELECT *
FROM GENERATE_SERIES(4,3,1);

SELECT *
FROM GENERATE_SERIES(3,4, -1);

SELECT *
FROM GENERATE_SERIES(1.1,4, 1.3);

SELECT *
FROM GENERATE_SERIES(1.1,4, 1.3) AS s(a);

SELECT current_date + s.a AS dates
FROM generate_series(0,14,7) AS s(a);

-- serie de fechas
SELECT *
FROM generate_series('2020-09-01 00:00:00'::timestamp,
          '2020-09-04 12:00:00'::timestamp, '10 hours');

SELECT a.id ,
  a.nombre,
  a.apellido,
  a.carrera_id,
  s.a
FROM alumnos AS a
  INNER JOIN generate_series(0, 10) AS s(a)
  ON s.a = a.carrera_id
ORDER BY a.carrera_id;

```

Reto hacer el triangulo de padding con un rango generado

```sql
SELECT LPAD('h', a, '*')
FROM generate_series(0, 15) AS s(a);
```

## Regularizando expresiones

```sql
/**
  * RETO: Resuelve el triángulo con generate_series
  */
SELECT  lpad ('*', generate_series, '*')
FROM    generate_series(1,10);

-- Generate independent ordinality --
SELECT  *
FROM    generate_series(1,10) WITH ordinality;

-- Use ordinality for lpad --
SELECT  lpad('*', CAST(ordinality AS int), '*')
FROM    generate_series(1,10) WITH ordinality;

-- Probando en series distintas --
SELECT  lpad('*', CAST(ordinality AS int), '*')
FROM    generate_series(10,2, -2) WITH ordinality;

```

validando strings con expresiones regulares

```sql
SELECT email
FROM alumnos
WHERE email ~*'[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}'

-- todos los email con alguna terminacion de google
SELECT email
FROM alumnos
WHERE email ~*'[A-Z0-9._%+-]+@google[A-Z0-9.-]+\.[A-Z]{2,4}';
```

## Bases de datos distribuidas

Resumen:
Las bases de datos distribuidas: es una colección de múltiples bases de datos separadas físicamente que se comunican mediante una red informática.

VENTAJAS:

-desarrollo modular.
-incrementa la confiabilidad.
-mejora el rendimiento.
-mayor disponibilidad.
-rapidez de respuesta.

DESVENTAJAS:

-Manejo de seguridad.
-complejidad de procesamiento.
-Integridad de datos más compleja.
-Costo.

TIPOS:

Homogéneas: mismo tipo de BD, manejador y sistema operativo. (aunque esté distribuida).
Heterogénea: puede que varíen alguna de los anteriores características.
-OS
-Sistema de bases de datos.
-Modelo de datos.

ARQUITECTURAS:
-** cliente- servidor**: donde hay una BD principal y tiene varias BD que sirven como clientes o como esclavas que tratarán de obtener datos de la principal, a la que normalmente se hacen las escrituras.

Par a par (peer 2 peer): donde todos los puntos en la red de bd son iguales y se hablan como iguales sin tener que responder a una sola entidad.
multi manejador de bases de datos.
ESTRATEGIA DE DISEÑO:

top down: es cuando planeas muy bien la BD y la vas configurando de arriba hacia abajo de acuerdo a tus necesidades.
bottom up: ya existe esa BD y tratas de construir encima.
ALMACENAMIENTO DISTRIBUIDO:

-Fragmentación: qué datos van en dónde.

fragmentación horizontal: (sharding) partir la tabla que estás utilizando en diferentes pedazos horizontales.

fragmentación vertical: cuando parto por columnas.

fragmentación mixta: cuando tienes algunas columnas y algunos datos en un lugar y algunas columnas y algunas tuplas en otro lugar.

-Replicación: tienes los mismos datos en todas ala BBDD no importa donde estén.

-replicación completa: cuando toda al BD está en varias versiones a lo largo del globo, toda la información está igualita en todas las instancias de BD.
-replicación parcial: cuando algunos datos están replicados y compartidos en varias zonas geográficas
-sin replicación: no estás replicando nada de los datos, cada uno está completamente separa y no tienen que estarse hablando para sincronizar datos entre ellas.

DISTRIBUCIÓN DE DATOS:

-Distribución: cómo va a pasar la data entre una BD y otra. Tiene que ver mucho con networking, tiempos, latencia, etc. Pueden ser:

Centralizada: cuando la distribuyes des un punto central a todas las demás
Particionada: está partida en cada una de las diversas zonas geográficas y se comparten información entre ellas.
Replicada: tener la misma información en todas y entre ellas se hablan para siempre tener la misma versión.

## Queries distribuídos

En el caso que la base de datos se encuentra distribuida en multiples regiones y la informacion se encuentra fragmentada la construccion de queries debe tener un debido nivel de analisis.

Debido a que en una sola base de datos responder una misma pregunta puede tener multiples soluciones con diferencias de ejecuccion en el rango de milisegundos.
Pero en las bases de datos distribuidas dependiendo de en donde se encuentre la informacion fisicamente, que informacion hay en cada region y que pregunta se quiere contestar, las diferentes formas de resolver la pregunta mediante una consulta SQL puede tener vastas diferencias en el tiempo de ejecucion,como ejemplo algunas soluciones pudiesen llegar al rango de 5 horas para ejecutarse y otras solamente necesitando 0.10 segundos.

## Sharding

Es un tipo de partición horizontal para nuestras bases de datos. Divide las tuplas de nuestras tablas en diferentes ubicaciones de acuerdo a ciertos criterios de modo que para hacer consultas, las tendremos que dirigir al shard o parte que corresponda.

### Cuándo usar

Cuando tenemos grandes volúmenes de información estática que representa un problema para obtener solo unas cuantas tuplas en consultas frecuentes.

### Inconvenientes

- Cuando debamos hacer joins frecuentemente entre shards
- Baja elasticidad. Los shards crecen de forma irregular unos más que otros y vuelve a ser necesario usar sharding (subsharding)
- La llave primaria pierde utilidad

## Window functions

### ¿Qué son?

Realizan cálculos en algunas tuplas que se encuentran relacionadas con la tupla actual.

### ¿Para que sirven?

Evitan el uso de self joins y reduce la complejidad alrededor de la analítica, agregaciones y uso de cursores.

```sql
SELECT *,
  AVG(colegiatura) OVER(PARTITION BY carrera_id)
FROM alumnos;


SELECT *,
  AVG(colegiatura) OVER()
FROM alumnos;

SELECT *,
  SUM(colegiatura) OVER(ORDER BY colegiatura)
FROM alumnos;


SELECT *,
  SUM(colegiatura) OVER(PARTITION BY carrera_id ORDER BY colegiatura)
FROM alumnos;


SELECT *,
  RANK() OVER(PARTITION BY carrera_id ORDER BY colegiatura DESC) AS brand_rank
FROM alumnos
ORDER BY carrera_id, brand_rank;


SELECT *,
  RANK() OVER(PARTITION BY carrera_id ORDER BY colegiatura DESC) AS brand_rank
FROM alumnos
-- WHERE brand_rank < 3
ORDER BY carrera_id, brand_rank;


SELECT *
FROM (
  SELECT *,
  RANK() OVER(PARTITION BY carrera_id ORDER BY colegiatura DESC) AS brand_rank
  FROM alumnos
) AS colegiatura_por_carrera
WHERE brand_rank < 3
ORDER BY carrera_id, brand_rank;
```

## Particiones y agregación

```sql
SELECT ROW_NUMBER() OVER() AS row_id, *
FROM alumnos;


SELECT ROW_NUMBER() OVER(ORDER BY fecha_incorporacion) AS row_id, *
FROM alumnos;


SELECT FIRST_VALUE(colegiatura) OVER(PARTITION BY carrera_id) AS primera_colegiatura, *
FROM alumnos;


SELECT LAST_VALUE(colegiatura) OVER(PARTITION BY carrera_id) AS ultima_colegiatura, *
FROM alumnos;


SELECT NTH_VALUE(colegiatura, 3) OVER(PARTITION BY carrera_id) AS ultima_colegiatura, *
FROM alumnos;


SELECT *,
  RANK() OVER(PARTITION BY carrera_id ORDER BY colegiatura DESC) AS colegiatura_rank
FROM alumnos
ORDER BY carrera_id, colegiatura_rank;


SELECT *,
  DENSE_RANK() OVER(PARTITION BY carrera_id ORDER BY colegiatura DESC) AS colegiatura_rank
FROM alumnos
ORDER BY carrera_id, colegiatura_rank;


-- (rank - 1) / (total_rows - 1)
SELECT *,
  PERCENT_RANK() OVER(PARTITION BY carrera_id ORDER BY colegiatura DESC) AS colegiatura_rank
FROM alumnos
ORDER BY carrera_id, colegiatura_rank;
```

- ROW_NUMBER(): nos da el numero de la tupla que estamos utilizando en ese momento.
- OVER([PARTITION BY column] [ORDER BY column DIR]): nos deja Particionar y Ordenar la window function.
- PARTITION BY(column/s): es un group by para la window function, se coloca dentro de OVER.
- FIRST_VALUE(column): devuelve el primer valor de una serie de datos.
- LAST_VALUE(column): Devuelve el ultimo valor de una serie de datos.
- NTH_VALUE(column, row_number): Recibe la columna y el numero de row que queremos devolver de una serie de datos
- RANK(): nos dice el lugar que ocupa de acuerdo a el orden de cada tupla, deja gaps entre los valores.
- DENSE_RANK(): Es un rango mas denso que trata de eliminar los gaps que nos deja RANK.
- PERCENT_RANK(): Categoriza de acuerdo a lugar que ocupa igual que los anteriores pero por porcentajes

## El futuro de SQL

SQL como lenguaje tiene un  gran futuro por delante y nuevos manejadores de bases de datos utilizan un lenguage similar al SQL por lo cual aprender SQL sigue siendo fundamental en la peticion de datos.

## Integridad de datos

- [integridad de datos conceptos](https://es.wikipedia.org/wiki/Integridad_de_datos#:~:text=El%20t%C3%A9rmino%20integridad%20de%20datos,perderse%20de%20muchas%20maneras%20diferentes.)
- [Data integrity](https://en.wikipedia.org/wiki/Data_integrity)


- Integridad de dominio: La integridad de dominio es la validez de las restricciones que debe cumplir una determinada columna de la tabla.
Datos Requeridos: establece que una columna tenga un valor no NULL. Se define efectuando la declaración de una columna es NOT NULL cuando la tabla que contiene las columnas se crea por primera vez, como parte de la sentencia CREATE TABLE.
Chequeo de Validez: cuando se crea una tabla cada columna tiene un tipo de datos y el DBMS asegura que solamente los datos del tipo especificado sean ingresados en la tabla.
- Integridad de entidad: establece que la clave primaria de una tabla debe tener un valor único para cada fila de la tabla; si no, la base de datos perderá su integridad. Se especifica en la sentencia CREATE TABLE. El DBMS comprueba automáticamente la unicidad del valor de la clave primaria con cada sentencia INSERT Y UPDATE. Un intento de insertar o actualizar una fila con un valor de la clave primaria ya existente fallará.
- Integridad referencial: asegura la integridad entre las llaves foráneas y primarias (relaciones padre/hijo). Existen cuatro actualizaciones de la base de datos que pueden corromper la integridad referencial:
  La inserción de una fila hijo se produce cuando no coincide la llave foránea con la llave primaria del padre.
  La actualización en la llave foránea de la fila hijo, donde se produce una actualización en la clave ajena de la fila hijo con una sentencia UPDATE y la misma no coincide con ninguna llave primaria.
  La supresión de una fila padre, con la que, si una fila padre -que tiene uno o más hijos- se suprime, las filas hijos quedarán huérfanas.
  La actualización de la clave primaria de una fila padre, donde si en una fila padre, que tiene uno o más hijos se actualiza su llave primaria, las filas hijos quedarán huérfanas.
