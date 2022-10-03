# MySQL

SQL (Structured Query Lenguaje) es un lenguaje implementado para el manejo de base de datos.

Hay 2 tipos de base de datos:

- Relational Database (RDBMS)
- Non relational database (No SQL)

Básicamente en una `relational database` tienes una tabla, cuyo contenido, hace referencia al contenido de otra tabla que a su vez es otra base de datos, etc.

_ejemplo:_

![](./imgs/MySQL.png)
![](./imgs/MySQL2.png)

SQL es manejado por distintos programas:
DBMS (Database Management SQL):

- Microsoft SQL Server
- MySQL
- PostgreSQL
- Oracle

El trabajo de esos programas a fin de cuentas es manejar tu base de datos.
Cada uno tiene su manera de hacerlo, pero todos usan SQL.

<br>

---

<br>

## `Cómo extraer data de una tabla`

### _`USE`_ `y` _`SELECT`_ `statements`

Esto lo haces escribiendo un query.

Asumiendo que ya tienes distintas bases de datos, creas una nueva tab de query en tu workbench.

Primero tienes que usar la palabra reservada
'**USE**', a manera de import cual react.

```SQL
USE sql_store
```

Con esa linea es como que estas ya targeteando a esa base de datos.

Luego tienes que usar el select statement.
El SELECT statement va seguido de las columns que quieres extraer (sin son varias, las separas con una coma), seguido de un _`FROM`_, después del cual, va escrito la tabla en donde están encontradas las columnas que le especificaste al _`SELECT`_

```SQL
USE sql_store;

SELECT costomer_id, first_name FROM customers
```

Si quieras extraer todas las columnas, simplemente escribirías con un asterisco, así:

```SQL
USE sql_store;

SELECT * FROM customers
```

Nota como se usa punto y coma despues de cada statement.

Al correr este código, deberías tener ya generada, una tabla con los elementos de la tabla que especificaste :)

<br>

### _`WHERE`_

Hasta ahora has usado las cláusulas `select` y `from`, pero existen muchas más.
Por ejemplo, puedes usar la cláusula `where` para filtrar la data, y especificar que por ejemplo, te devuelva un cliente, cuyo id sea el de 1:

```SQL
USE sql_store;

SELECT * FROM customers WHERE customer_id = 1
```

Ese código o expresión ahora te devolvería un solo cliente en tu tabla.

<br>

### _`ORDER BY`_

Con esto podrías ordenarlos como en orden albatético, numérico, etc, dependiendo de la columna que le especifiques.

```SQL
USE sql_store;

SELECT * FROM customers WHERE customer_id = 1

ORDER BY first_name
```

En este específico caso no tendría ningún efecto, ya que por defecto, esta expresión solo te devuelve un cliente.
Para eso comentaríamos el segundo statement, así:

```SQL
USE sql_store;

-- SELECT * FROM customers WHERE customer_id = 1

ORDER BY first_name
```

Más adelante entraremos más en detalle sobre estas clauses, pero lo que debes saber hasta ahora, es que:

- from, where y order by son opcionales
- cuando son escritas, deben ser escritas en ese orden.

<br>

---

<br>

## `Clauses en detalle`

### _**`Select`**_

Puedes pedir toda la tabla con

```SQL
SELECT *
FROM customers
```

<br>
Puedes especificar que columnas deseas, para no tener que traer una pila de data innecesaria:

```SQL
SELECT first_name, last_name
FROM customers
```

Esto te devuelve dos columnas, en el orden que las especificaste.

<br>

Otro ejemplo, pero ahora con 3 columnas:

```SQL
SELECT first_name, last_name, points
FROM customers
```

<br>

Puedes regresar los puntos y otra columna que tenga una expresión matemática, como por ejemplo, sumarle un valor a los puntos pre-existentes:

```SQL
SELECT first_name, last_name, points, points + 10
FROM customers
```

_`Esta de arriba de devolvería 4 columnas. Siendo la 4ta una suma`_

<br>

Cuando tienes ya una linea muy larga, puedes escribir cada elemento en su propia linea, correctamente tabulado:

```SQL
SELECT
    first_name,
    last_name,
    points,
    points + 10 * 100
FROM customers
```

_`Nótese que en una expresión matemática, primero se resuelven las multiplicaciones, luego las sumas`_

Puedes usar paréntesis para especificar:

```SQL
SELECT
    first_name,
    last_name,
    points,
    (points + 10) * 100
FROM customers
```

<br>

Ahora, por ejemplo, el código de arriba te devolvería 4 columnas.
Cada uno con su respectivo nombre, pero como podrás ver, la 4ta columna se te creará, con ese nombre.
Lo que puedes hacer es usar el alias '`as`', para editar el título de la nueva columna:

```SQL
SELECT
    first_name,
    last_name,
    points,
    (points + 10) * 100 AS discount_factor
FROM customers
```

| first_name | last_name | points | discount_factor |
| ---------- | --------- | ------ | --------------- |
| Juan       | Enderica  | 500    | 51000           |
| Manuel     | Fayad     | 600    | 61000           |

#### _**`DISTINCT`**_ keyword

Digamos que en tu tabla de clientes, tienes una columa con resspectivas ciudades o estados:
| first_name | last_name | points | state |
| ---------- | --------- | ------ | --------------- |
| Juan | Enderica | 500 | Guayas |
| Manuel | Fayad | 600 | Quito |
| Pablo | Jiemnez | 600 | Cuenca |
| Josué | Jiemnez | 700 | Cuenca |

Si tu piedes, con el select, la columan de state, el código te devolverá una tabla, con los estados tal cual, y en caso de haber duplicados, igual te los regresa:
| state |
| ------ |
| Guayas |
| Quito |
| Cuenca |
| Cuenca |

<br>

Con la keyword `distinct`, el código te regresa todos los estados, pero sin duplicados:

```SQL
SELECT DISTINCT state
FROM customers
```

| state  |
| ------ |
| Guayas |
| Quito  |
| Cuenca |

<br>

---

<br>

<!-- ## `Clauses en detalle` -->

### _**`Where`**_

Como está especificado más arriba, la where clause se la usa para filtrar data.

Por ejemplo en este código:

```SQL
SELECT *
FROM customers
WHERE points > 3000
```

Lo que le estas diciendo es 'tráeme todas las columnas de customers, pero incluye solo a las que tengo mas de 3000 puntos'
Eso lo logra, iterando por todas las filas en la tablas de customers, y filtrando las que no cumplan la condición.

### **operadores de comparación**:

- mayor >
- mayor o igual >=
- menor <
- menor o igual <=
- = igual
- != no igual
- <> no igual

Por ejemplo, teniendo la siguiente tabla:
| first_name | last_name | points | state |
| ---------- | --------- | ------ | --------------- |
| Juan | Enderica | 500 | Guayas |
| Manuel | Fayad | 600 | Quito |
| Pablo | Jiemnez | 600 | Cuenca |
| Josué | Jiemnez | 700 | Cuenca |

Para elegir solo los que viven en Cuenca:

```SQL
SELECT *
FROM customers
WHERE state = 'Cuenca'
```

_`nótese las comillas, pueden ser t_ambien doble, y tampoco importan las mayúsculas`_

<br>

### **fechas**:

Teniendo la siguiente tabla:
| first_name | nacimiento | points | state |
| ---------- | --------- | ------ | --------------- |
| Juan | 1991-07-20 | 500 | Guayas |
| Manuel | 1234-06-18 | 600 | Quito |
| Pablo | 1990-07-09 | 500 | Cuenca |
| Josué | 1928-12-12 | 700 | Cuenca |

Con fechas tienes que tener claro que el standar en MySQL es:
`año(4dígitios)-mes(2dïgitos)-día`

ejemplo:

    1991-20-08

Entonces, si tu quisieras pedir a los clientes que sean del 90 en adelante, lo pides asi:

```SQL
SELECT *
FROM customers
WHERE nacimiento >= '1990-01-01'
```

##### _`nótese que las fechas no son strings, pero de igual manera las debes escribir dentro de comillas`_

<br>

---

<br>

### _**`AND, OR y NOT`**_ operators

Asumiendo que en la siguiente tabla, quiereas extraer, al igual que el ejemplo anterior, los clientes que son del 90 en adelante, pero tambien que tengan más de 600 puntos.

La tabla:
| first_name | nacimiento | points | state |
| ---------- | --------- | ------ | --------------- |
| Juan | 1991-07-20 | 500 | Guayas |
| Manuel | 1234-06-18 | 600 | Quito |
| Pablo | 1990-07-09 | 500 | Cuenca |
| Josué | 1928-12-12 | 700 | Cuenca |

El código:

```SQL
SELECT *
FROM customers
WHERE nacimiento >= '1990-01-01' AND points > 600
```

Una vez que ejecutes ese query, te devolverá clientes que cumplan con las dos condiciones.

En contraste a `and`, tenemos el operador `or`, el cual te devolvería clientes con tal de que una de las condiciones se cumplan.

ejemplo:

```SQL
SELECT *
FROM customers
WHERE nacimiento >= '1990-01-01' OR points > 600
```

Puedes mezclar los dos en una linea:

```SQL
SELECT *
FROM customers
WHERE nacimiento >= '1990-01-01' OR points > 600 AND state = 'Guayas'
```

Eso te devolvería cualquier cliente que

- o nació del 90 para arriba
- o tenga más de 600 puntos pero que tambien haya nacido en Guayas

**_`cabe recalcar que el and, precede al or, por lo cual es evaluado antes en la ejecución`_**

Una manera más limpia de escribir el código de arriba, para que se entienda mejor, sería:

```SQL
SELECT *
FROM customers
WHERE nacimiento >= '1990-01-01' OR
    (points > 600 AND state = 'Guayas')
```

El operado `not` se lo usa para negar una condición.
Es decir, asumiendo que tenemos el siguiente código que ya hemos usado más arriba:

```SQL
SELECT *
FROM customers
WHERE nacimiento >= '1990-01-01' OR points > 600
```

Eso te devolvería clientes que cumplan con cualquier de esas condiciones.
Ahora, que pasa si queremos a los clientes que **NO** cumplan con ninguna de esas condiciones:

```SQL
SELECT *
FROM customers
WHERE NOT (nacimiento >= '1990-01-01' OR points > 600)
```

**_`Puedes no usar paréntesis, pero se entiende mejor si los usas`_**

Eso entonces te devuelve clientes que no nacieron en los 90's o después, pero que tampoco tienen más de 600 puntos, ya que ni una de las condiciones se puede cumplir.

Es decir, este código que acabamos de usar:

```SQL
SELECT *
FROM customers
WHERE NOT (nacimiento >= '1990-01-01' OR points > 600)
```

Es exactamente lo mismo a esto:

```SQL
SELECT *
FROM customers
WHERE (nacimiento <= '1990-01-01' AND points <= 600)
```

<br>

---

<br>

### _**`IN`**_ operator

Para entender el in, primero hay que tener claro algo.
El operador _or_, combina condiciones. Entonces, digamos que queremos regresar clientes que son o de guayaquil o de cuenca o de quito. La manera correcta de escribirlo sería:

```SQL
SELECT *
FROM customers
WHERE state = 'Cuenca' OR state = 'Guayaquil' OR state = 'Quito'
```

La manera incorrecta:

```SQL
SELECT *
FROM customers
WHERE state = 'Cuenca' OR 'Guayaquil' OR 'Quito'
```

La razón por la cual no funciona es que OR combina condiciones. Pero como puedes ver en el ejemplo de arriba, al lado derecho del OR, no hay una condición. Hay una string. Por eso es que debes escribirlo como en el penúltimo ejemplo.

El penúltimo ejemplo te devolvería cualquier cliente que sea de cualquiera de esas ciudades.
Aquí es cuando entra el operado **_in_**

Este operador te permite escribir lo del ejemplo anterior, pero de una manera mas limpia.
Entonces, en vez de combinar varias condiciones usando el operador OR, como aquí:

```SQL
SELECT *
FROM customers
WHERE state = 'Cuenca' OR state = 'Guayaquil' OR state = 'Quito'
```

Podemos usar el operador _in_ así:

```SQL
SELECT *
FROM customers
WHERE state IN ('Cuenca','Guayaquil','Quito')
```

Este query hace exactamente lo mismo que el anterior, es decir, te devuelve clientes que tengan cualquiera de esas ciudades.
El orden en que pases las ciudades no importa.

Podemos mezclar el **_IN_** con el **_NOT_**
Asumamos que queremos agarrar a cualquier cliente que no tenga NI UNA, de esas ciudades:

```SQL
SELECT *
FROM customers
WHERE state NOT IN ('Cuenca','Guayaquil','Quito')
```

<br>

---

<br>

### _**`BETWEEN`**_ operator

Teniendo esta tabla:

| first_name | last_name | points | state  |
| ---------- | --------- | ------ | ------ |
| Juan       | Enderica  | 300    | Guayas |
| Manuel     | Fayad     | 100    | Quito  |
| Pablo      | Jiemnez   | 600    | Cuenca |
| Josué      | Jiemnez   | 900    | Cuenca |

Digamos que queremos obtener los clientes que tengan entre 500 y 900 puntos.

```SQL
SELECT *
FROM customers
WHERE points >= 500 AND points <= 900
```

Ese query básicamente te devuelve clientes que cumplan con esas dos condiciones al mismo tiempo, devolviéndote los que tienen entre 500 y 900 puntos.

En ese caso, puedes usar el operador _between_, logrando lo mismo.
Este operador lo puedes usar siempre que necesites analizar un rango de valores, dentro de un mismo atributo. En este caso, el atributo es points.
Esto hace tu código más corto y más limpio.
Lo escribirías así:

```SQL
SELECT *
FROM customers
WHERE points BETWEEN 500 AND 900;
```

Esto te devuelve exactamente lo smimo al código anterior.
Cabe recalcar que es inclusivo, es decir, incluye el 500, y el 900, al igual que un **>=** o **<=**.

<br>

---

<br>

### _**`LIKE`**_ operator

Con este operador se puede regresar rows que hagan match a ciertos patrones de string que le especifiquemos.
Por ejemplo, digamos que queremos los clientes cuyo apellido empieza con la letra 'J':

```SQL
SELECT *
FROM customers
WHERE last_name LIKE 'J%';
```

El % significa que después de la 'j' pueden ir cualquier cantidad de caracteres, desde 0 a infinito.
Tampoco importa si la j esta en mayúsculas.

Otro ejemplo sería, conseguir los clientes que tengan la letra e en cualquier lugar de su apellido.

```SQL
SELECT *
FROM customers
WHERE last_name LIKE '%J%';
```

Otro ejemplo: Conseguir clientes que sus apellidos terminen con la letra 'z':

```SQL
SELECT *
FROM customers
WHERE last_name LIKE '%Z ';
```

Dentro de LIKE, así como tenemos _%_, tambien tenemos \_\__.
El subguión sirve para representar a un \_cualquier único caracter_
Es decir: si pones \_Y , estás queriendo decir que estás buscando a dos y solo dos caracteres, de los cuales el primero puede ser cualquiera, y el segundo una 'y'.
Puedes usar varios subguiones seguidos como quieras, cada uno representando a un caracter.
De manera que, si usas:

```SQL
SELECT *
FROM customers
WHERE last_name LIKE '______Z ';
```

ese query te devolvería clientes con apellido Jimenez.
Este código tambien te devolvería Jimenez, ya que reemplazé el primer subguión por una 'J':

```SQL
SELECT *
FROM customers
WHERE last_name LIKE 'J_____Z ';
```

Como explicado anteriormente en otros operadores, en caso de querer todo menos los matches de ese patrón, puedes anteponer el operador NOT:

```SQL
SELECT *
FROM customers
WHERE last_name NOT LIKE 'J_____Z ';
```

<br>

---

<br>

### _**`REGEXP`**_ operator

| first_name | last_name | points | state  |
| ---------- | --------- | ------ | ------ |
| Juan       | Enderica  | 300    | Guayas |
| Manuel     | Fayad     | 100    | Quito  |
| Pablo      | Jiemnez   | 600    | Cuenca |
| Josué      | Jiemnez   | 900    | Cuenca |

Digamos que de esa tabla, quieres las columnas de los clientes que tengan'nez' en cualquier lugar de su apellido.
Usando el like operator, lo tipearías así:

```SQL
SELECT *
FROM customers
WHERE last_name LIKE '%nez%';
```

Esto te devolvería a Pablo y a Josué.
Ahora, podemos hacer exactamente lo smimo, con el regexp operator:

```SQL
SELECT *
FROM customers
WHERE last_name REGEXP 'field';
```

Eso hace lo mismo que su anterior.

Con el operador _`REGEXP`_, hay a disposición varios símbolos los cuales sirven para especificar detalles de la string que quieres retornar.

Ahora haré una lista de ejemplos de esos símbolos:

<br>

_`^`_

Significa que la string debe empezar, con lo que le escribas despues del sīmbolo.

En este caso:
El string debe empezar con 'field'

```SQL
SELECT *
FROM customers
WHERE last_name REGEXP '^field';
```

<br>

_`$`_

A diferencia de anterior, este representa el final de una string.

El apellido debe terminar con field:

```SQL
SELECT *
FROM customers
WHERE last_name REGEXP 'field$';
```

<br>

_`|`_

Retorna cualquiera de los matches que le escribas.

El apellido tiene que tener o la palabra field o la palabra jimenez:

```SQL
SELECT *
FROM customers
WHERE last_name REGEXP 'field|jimenez';
```

El apellido puede o empezar con field, o tener jimenez en cualquier lugar de su string o terminar con rica.

```SQL
SELECT *
FROM customers
WHERE last_name REGEXP '^field|jimenez|rica$';
```

<br>
<br>
El apellido tiene que tener la letra 'e' en cualquier parte:

```SQL
SELECT *
FROM customers
WHERE last_name REGEXP 'e';
```

<br>
<br>
[]

El apellido tiene que tener la letra e en cualquier parte, pero FIJO, antes de la 'e' tiene que haber o una 'g', o 'i' o 'm'.

```SQL
SELECT *
FROM customers
WHERE last_name REGEXP '[gim]e';
```

El apellido tiene que tener la letra e en cualquier parte, pero FIJO, después de la 'e' tiene que haber o una 'g', o 'i' o 'm'.

```SQL
SELECT *
FROM customers
WHERE last_name REGEXP 'e[gim]';
```

El apellido tiene que tener la letra e, pero antes puede ir cualquier letra que sea de la 'a' a la 'f' (a o b o c o d o e o f)

```SQL
SELECT *
FROM customers
WHERE last_name REGEXP '[a-f]e';
```

<br>

---

<br>

### _**`NULL`**_ operator

Con esto puedes buscard por registro faltantes de un atributo.
Por ejemplo, clientes que no tengan un dato, teniendo esta tabla:
| first_name | last_name | points | state |
| ---------- | --------- | ------ | ------ |
| Juan | Enderica | 300 | Guayas |
| Manuel | Fayad | 100 | Quito |
| Pablo | Jiemnez | null | Cuenca |
| Josué | Jiemnez | 900 | Cuenca |

```SQL
SELECT *
FROM customers
WHERE points IS NULL;
```

Esto te devolvería a Pablo ya que el no tiene puntos.

```SQL
SELECT *
FROM customers
WHERE points IS NOT NULL;
```

Puedes anteponer el operador 'NOT', a manera que el query te devuelva a todos los clientes que SI tienen puntos.

<br>

---

<br>

### _**`ORDER BY`**_ operator

Para entender esto, primero se debe saber como funciona SQL por default.
Si tienes una tabla de clientes, y corres el siguiente query:

```SQL
SELECT *
FROM customers
```

Tu tendrías una tabla mas o menos así:

| id  | first_name | last_name | points | state           |
| --- | ---------- | --------- | ------ | --------------- |
| 1   | Juan       | Enderica  | 300    | Guayas          |
| 2   | Manuel     | Fayad     | 100    | Quito           |
| 3   | Pablo      | Jiemnez   | null   | Cuenca          |
| 4   | Josué      | Jiemnez   | 900    | Cuenca          |
| 5   | Josué      | Guevara   | 50     | Morona Santiago |

Como puedes ver, por default, SQL te los regresa ordenados, segun la columna ID.

En base de datos relacional, cada tabla, tiene su ` primary key column`, y por convención, los valores de esa columna, tienen que identificar a esa fila de manera individual, por eso es que cada una tiene un distinto número.

Por eso es que cuando haces un query para pedir una tabla, sin especificar como ordenarlos, el query te devuelve una tabla ya ordenada, segun esa primera columna.

Sabiendo eso, ya puedes saber que uno puedes pedir una tabla, pero hacer que la tengas ordenada segun los valores de una columna distinta.

Ejemplo:

```SQL
SELECT *
FROM customers
ORDER BY last_name
```

Ese query te devolvería:
| id | first_name | last_name | points | state |
| -- | ---------- | --------- | ------ | ------ |
|1| Juan | Enderica | 300 | Guayas |
|2| Manuel | Fayad | 100 | Quito |
|5| Josué | Guevara | 50 | Morona Santiago |
|3| Pablo | Jiemnez | null | Cuenca |
|4| Josué | Jiemnez | 900 | Cuenca |

Como puedes ver, te los ordeno, según orden alfabético, en la columna que le especificaste (last_name)

Puedes hacer en sea en orden alfabético pero de manera descendente con la palabra `DESC`:

```SQL
SELECT *
FROM customers
ORDER BY last_name DESC
```

Eso te devuelve:
| id | first_name | last_name | points | state |
| -- | ---------- | --------- | ------ | ------ |
|3| Pablo | Jiemnez | null | Cuenca |
|4| Josué | Jiemnez | 900 | Cuenca |
|5| Josué | Guevara | 50 | Morona Santiago |
|2| Manuel | Fayad | 100 | Quito |
|1| Juan | Enderica | 300 | Guayas |

<br>

Puedes poner varias columnas dentro del ORDER BY, a manera que primero los ordena segun su primer valor, y luego, en caso de haber ese primer valor repetido, los ordena segun el segundo criterio que le dijiste:

```SQL
SELECT *
FROM customers
ORDER BY state, last_name
```

Este query te los ordenará por ciudad, y luego, en el caso de Cuenca ya que hay dos clientes de Cuenca, te los ordena en orden alfabético según su apellido:
| id | first_name | last_name | points | state |
| -- | ---------- | --------- | ------ | ------ |
|3| Pablo | Jiemnez | null | Cuenca |
|4| Josué | Jiemnez | 900 | Cuenca |
|1| Juan | Enderica | 300 | Guayas |
|5| Josué | Guevara | 50 | Morona Santiago |
|2| Manuel | Fayad | 100 | Quito |

Una ventaja de MySQL es que puedes ordenarlas dando como referencia una columna, como has venido haciendo hasta ahora con el ORDER BY, pero puedes no necesariamente pedir ver esas columnas, es decir:

```SQL
SELECT first_name, last_name
FROM customers
ORDER BY state, last_name
```

Esto te da:

| first_name | last_name |
| ---------- | --------- |
| Pablo      | Jiemnez   |
| Josué      | Jiemnez   |
| Juan       | Enderica  |
| Josué      | Guevara   |
| Manuel     | Fayad     |

Date cuenta como te los devolvío organizando según llos filtros que le especificaste, pero no estas viendo las columnas de esos filtros, solo estas viendo las columnas que le pediste en SELECT.
En otros sistemas de manejo de datos no se puede hacer esto, usualmente te estaría botando errores.

<br>

Puedes ser mas breve tambien, y en vez de especificar bajo que columna ordenarla, puedes poner el número de la columna **siempre y cuando esta esté especificada dentro del _`select`_**:

```SQL
SELECT first_name, last_name
FROM customers
ORDER BY 1, 2
```

Esto te las odena primero según su first_name y luego según su last_name.

<br>

### _**`AS`**_ operator

Puedes inventarte una columna extra al momento de generar tu tabla. Dentro de esa celda puedes inventarte lo que quieras, puntos, sumas, multiplicaciones, calcular porcentajes usando otras celdas, por ejemplo:

```SQL
SELECT first_name, last_name, 10 AS goles
FROM customers
ORDER BY 1, 2
```

Esto te da:

| first_name | last_name | goles |
| ---------- | --------- | ----- |
| Josué      | Guevara   | 10    |
| Josué      | Jiemnez   | 10    |
| Juan       | Enderica  | 10    |
| Manuel     | Fayad     | 10    |
| Pablo      | Jiemnez   | 10    |

Ten cuidado usando esto porque si en un futuro llega a cambiar el orden o el valor de las columnas que se le pasan a _select_, el código te devolvería algo que no quieres.

<br>

<br>

---

<br>

### _**`LIMIT`**_ operator

Con esta clause puedes limitar el nombre de registros retornados por tu query.
Por ejemplo, si ejecutas este query:

```SQL
SELECT *
FROM customers
```

Retornarías todos los matches. Ahora, si quisieras regresar solo los 3 primeros clientes de ese query:

```SQL
SELECT *
FROM customers
LIMIT 3
```

En caso de que solo hayan dos clientes, igual te trae todos. la idea es que no pase de 3.

Pueses pedir un max que 300 por ejemplo, y si hay menos, ye los trae todos igual:

```SQL
SELECT *
FROM customers
LIMIT 300
```

Puedes hacer que al retornarte, el query skipee un numero de clientes, y que ahi recien retorne x cantidad de clientes. Ejemplo:

```SQL
SELECT *
FROM customers
LIMIT 6, 3
```

Ese código se salta los 6 primeros clientes de tu query, y de ahi te bota los siguientes 3.
Digamos que quieres obtener tu top 3 de clientes con mas puntos por ejemplo:

```SQL
SELECT *
FROM customers
ORDER BY points DESC
LIMIT 3
```

La limit clause siempre debe ser espicificada al final de tu query.

<br>

<br>

---

<br>

### _**`INNER JOINS`**_ operator

Hasta ahora solo hemos seleccionado data de una sola tabla, pero en realidad vas bastantes veces a tener que extraer data de distintas tablas al mismo tiempo.

Digamos que tienes la siguiente tabla orders:
| order id| customer id| status |
| ---------- | --------- | ----- |
| 1 | 6 | 1 |
| 2 | 7 | 12 |
| 3 | 1 | 1 |
| 4 | 2 | 1 |
| 5 | 4 | 3 |

Como pudes ver, es tu tabla de ordenes. Y en la columna de clientes, en vez de tener todo la info del cliente, solo tiene el id.
Este id es el id del cliente en otra tabla clients. La cual si posee toda la info del cliente, nombre, mail, etc.
Esto se lo hace de esta manera cosa que si cambia la info del cliente, solo lo cambias en la tabla de clientes, ya que en esta tabla de ordenes, se lo esta referenciando de esa tabla.
Esos son los beneficios de relational data base.

Ahora, que pasa si quieres acceder a unas órdenes de la tabla de orders, pero quieres que con ese query, muestre datos completos del cliente, más no solo el client id (que es lo unico del cliente que hay en la tabla orders):

```SQL
SELECT *
FROM orders
```

Hasta ahi estas seleccionado todas las ordenes de la tabla orders.
Ahora lo que tienes que hacer es combinar, las columnas que quieras de esta orders table, con las columnas del client table:

```SQL
SELECT *
FROM orders
JOIN customers
    ON orders.customer_id = customers.customer_id
```

Es litarealmente decerle a SQL 'mira, hazme una tabla juntando la de orders con la de customers, y bajo que criterio las unes? Pues bajo la condicion de que el customer id en orders sea igual al customer id en cosumers.

Ese query te hace algo como una concatenación. Y como estamos seleccionando todo, te devuelve una tabla gigante.
Lo que puedes hacer tambien es minimizar la cantidad de columnas:

```SQL
SELECT order_id, first_name, last_name
FROM orders
JOIN customers
    ON orders.customer_id = customers.customer_id
```

Eso te traería no mas 3 columnas.

<br>
<br>
<br>

```SQL
SELECT order_id, orders.customer_id first_name, last_name
FROM orders
JOIN customers
    ON orders.customer_id = customers.customer_id
```

Cuando tienes columnas repetidas en tablas distintas, tienes que espcificar poniendo como prefijo el nombre de la tabla.

<br>
<br>
<br>

Como puedes ver, en el código anterior repites palabras orders y customers bastante.
Puedes darle alias a las palabras cosa que en vez de escribirlas completas, solo escribes su alias. Esto hace que tu código se vea un poco mas limpio.

```SQL
SELECT order, o.customer_id first_name, last_name
FROM orders o
JOIN customers c
    ON o.c_id = c.customer_id
```

<br>

<br>

---

<br>

### _**`COMBINAR COLUMNAS DE DISTINTAS TABLAS DE MULTIPLES BASE DE DATOS`**_

A veces, tendras que acceder a una tabla que se encuentra en otra base de datos. Ejemplo:

```SQL
SELECT *
FROM orders_items oi
JOIN ssql_inventory.products p
    ON oi.product_id = p.product_id
```

En el código de arriba, estas diciendo que le sumes a tu tabla de orders, la tabla de products que se encuentra en la base de datos inventory.
Bajo la condición de que la columna de productos en tu tabla de orders sea la misma que la columna de productos de tu tabla de productos.

<br>

<br>

---

<br>

### _**`SELF JOINS`**_

Habrá veces en las que quieres unir una tabla, con una columna de esa misma tabla.

Digamos que tienes una tabla de empleados, cada uno con su ID, y asi mismo, esa tabla tiene una columna de managers (reports to). Es decir, el manager de cada empleado.
En esa columna, esta el id del manager a quien el empleado responde, quien a su vez, se encuentra dentro de la misma tabla, ya que el manager tambien es un empleado.

Las reglas son llamar a la misma columna que se repite pero con diferentes alias y prejifos.
Y lo unes usando la condición dentro de **JOIN**

Digamos que quieres traer una tabla y que se le concatene el nombre del manager de cada uno, a la derecha:

```SQL
USE sql_hr;

SELECT *
FROM employees e
JOIN employees m
    ON e.reports_to = m.employee_id
```

El ON basicamente es decir que fila unir con que fila. En este caso estas diciendo, cuando te matcheen el reports_to de esta tabla, con ell employee_id de esta tabla, bajo ese criterio unelos.
Así mismo date cuenta que en el JOIN, traes a la misma tabla pero TIENES QUE traerla bajo otro alias.

Ese query lo que te traer es toda la tabla de empleados, y a su derecha, se le une el empleado cuyo id, es igual al de reports to.
EXPLICACION CLARISIMA :)

Ahora, si queremos solo el nombre del empleado, su id y el manager:

```SQL
USE sql_hr;

SELECT
    e.employee_id,
    e.first_name,
    m.first_name AS manager
FROM employees e
JOIN employees m
    ON e.reports_to = m.employee_id
```

En ese query, estás trayendo 3 columnas.

- Tu tabla sera una concatenacion de dos partes. La primera es id y nombre y la segunda es el manager.
- Por eso primero seleccionas de 'e', el id y el nombre.
- A eso se le junta la 3ra columna que la traes del alias m, que son los managers.
- El criterio es que el reports to sea igual a employee id.
- La traes esa columna como 'manager'
