##  EXAMEN BASES DE DATOS POSTGRES

### Consultas

#Consultas sobre una tabla

1. Devuelve un listado con todos los pedidos que se han realizado. Los pedidos deben estar ordenados por la fecha de realización, mostrando en primer lugar los pedidos más recientes.

```sql
	SELECT * FROM pedido
	ORDER BY pedido.fecha DESC;
```


```sql	
	 id |  total  |   fecha    | id_cliente | id_comercial 
	----+---------+------------+------------+--------------
	 16 | 2389.23 | 2019-03-11 |          1 |            5
	 15 |  370.85 | 2019-03-11 |          1 |            5
	 13 |  545.75 | 2019-01-25 |          6 |            1
	  8 | 1983.43 | 2017-10-10 |          4 |            6
	  1 |   150.5 | 2017-10-05 |          5 |            2
	  3 |   65.26 | 2017-10-05 |          2 |            1
	  5 |   948.5 | 2017-09-10 |          5 |            2
	 12 |  3045.6 | 2017-04-25 |          2 |            1
	 14 |  145.82 | 2017-02-02 |          6 |            1
	  9 |  2480.4 | 2016-10-10 |          8 |            3
	  2 |  270.65 | 2016-09-10 |          1 |            5
	 11 |   75.29 | 2016-08-17 |          3 |            7
	  4 |   110.5 | 2016-08-17 |          8 |            3
	  6 |  2400.6 | 2016-07-27 |          7 |            1
	  7 |    5760 | 2015-09-10 |          2 |            1
	 10 |  250.45 | 2015-06-27 |          8 |            2
	  
```

 
 2. Devuelve todos los datos de los dos pedidos de mayor valor.

```sql
	SELECT * FROM pedido
	ORDER BY total DESC
	LIMIT 2;
```

```sql	
	 id | total  |   fecha    | id_cliente | id_comercial 
	----+--------+------------+------------+--------------
	  7 |   5760 | 2015-09-10 |          2 |            1
	 12 | 3045.6 | 2017-04-25 |          2 |            1
 
	 
```

3. Devuelve un listado con los identificadores de los clientes que han realizado algún pedido. Tenga en cuenta que no debe mostrar identificadores que estén repetidos.

```sql
	SELECT DISTINCT cliente.id,cliente.nombre,cliente.apellido1,cliente.apellido2 FROM cliente
	JOIN pedido ON cliente.id = pedido.id_cliente;
```

```sql	
	 id | nombre | apellido1 | apellido2 
	----+--------+-----------+-----------
	  1 | Aarón  | Rivero    | Gómez
	  3 | Adolfo | Rubio     | Flores
	  6 | María  | Santana   | Moreno
	  2 | Adela  | Salas     | Díaz
	  4 | Adrián | Suárez    | 
	  7 | Pilar  | Ruiz      | 
	  8 | Pepe   | Ruiz      | Santana
	  5 | Marcos | Loyola    | Méndez
	 
	 
```

4. Devuelve un listado de todos los pedidos que se realizaron durante el año 2017, cuya cantidad total sea superior a 500€.

```sql
	SELECT * FROM pedido
	WHERE pedido.total >= 500 AND EXTRACT (YEAR FROM fecha) = 2017;
		
```

```sql	
	 id |  total  |   fecha    | id_cliente | id_comercial 
	----+---------+------------+------------+--------------
	  5 |   948.5 | 2017-09-10 |          5 |            2
	  8 | 1983.43 | 2017-10-10 |          4 |            6
	 12 |  3045.6 | 2017-04-25 |          2 |            1
		 
	 
```

5. Devuelve un listado con el nombre y los apellidos de los comerciales que tienen una comisión entre 0.05 y 0.11.

```sql
	SELECT comercial.nombre, comercial.apellido1, comercial.apellido2 FROM comercial
	WHERE comercial.comision = '0.05' OR  comercial.comision = '0.11';
```

```sql	
	 nombre  | apellido1 | apellido2 
	---------+-----------+-----------
	 Diego   | Flores    | Salas
	 Antonio | Vega      | Hernández
	 Alfredo | Ruiz      | Flores
	 
```

6. Devuelve el valor de la comisión de mayor valor que existe en la tabla comercial.

```sql
	SELECT MAX(comision) AS comision_de_mayor_valor  FROM comercial;
	
```

```sql	
	 comision_de_mayor_valor 
	-------------------------
		            0.15
		 
```

7. Devuelve el identificador, nombre y primer apellido de aquellos clientes cuyo segundo apellido no es NULL. El listado deberá estar ordenado alfabéticamente por apellidos y nombre.

```sql
	SELECT cliente.nombre, cliente.apellido1 FROM cliente
	WHERE cliente.apellido2 IS NOT NULL
	ORDER BY apellido1,nombre;
```

```sql
	
	  nombre   | apellido1 
	-----------+-----------
	 Guillermo | López
	 Marcos    | Loyola
	 Aarón     | Rivero
	 Adolfo    | Rubio
	 Pepe      | Ruiz
	 Adela     | Salas
	 Daniel    | Santana
	 María     | Santana
	 
```

8. Devuelve un listado de los nombres de los clientes que empiezan por A y terminan por n y también los nombres que empiezan por P. El listado deberá estar ordenado alfabéticamente.

```sql
	SELECT cliente.nombre FROM cliente
	WHERE nombre ILIKE 'A%N' OR nombre ILIKE 'P%'
	ORDER BY nombre ASC;
```

```sql	
	 nombre 
	--------
	 Aarón
	 Adrián
	 Pepe
	 Pilar 
	 
```


9. Devuelve un listado de los nombres de los clientes que no empiezan por A. El listado deberá estar ordenado alfabéticamente.

```sql
	SELECT cliente.nombre FROM cliente
	WHERE nombre NOT ILIKE 'A%'
	ORDER BY nombre ASC;
```

```sql	

	  nombre   
	-----------
	 Daniel
	 Guillermo
	 Marcos
	 María
	 Pepe
	 Pilar
 	 
```


10. Devuelve un listado con los nombres de los comerciales que terminan por el o o. Tenga en cuenta que se deberán eliminar los nombres repetidos.

```sql
	SELECT DISTINCT nombre
	FROM comercial
	WHERE nombre ILIKE '%o';
```

```sql	
	 nombre  
	---------
	 Diego
	 Antonio
	 Alfredo
	 
	 
```

# Consultas multitabla (Composición interna)

1. Devuelve un listado con el identificador, nombre y los apellidos de todos los clientes que han realizado algún pedido. El listado debe estar ordenado alfabéticamente y se deben eliminar los elementos repetidos.

```sql
	SELECT DISTINCT cliente.id,cliente.nombre,cliente.apellido1,cliente.apellido2 FROM cliente
	JOIN pedido ON cliente.id = pedido.id_cliente;
	ORDER BY nombre,apellido1,apellido2;
```

```sql	
	 id | nombre | apellido1 | apellido2 
	----+--------+-----------+-----------
	  1 | Aarón  | Rivero    | Gómez
	  3 | Adolfo | Rubio     | Flores
	  6 | María  | Santana   | Moreno
	  2 | Adela  | Salas     | Díaz
	  4 | Adrián | Suárez    | 
	  7 | Pilar  | Ruiz      | 
	  8 | Pepe   | Ruiz      | Santana
	  5 | Marcos | Loyola    | Méndez
	 
	 
```


2. Devuelve un listado que muestre todos los pedidos que ha realizado cada cliente. El resultado debe mostrar todos los datos de los pedidos y del cliente. El listado debe mostrar los datos de los clientes ordenados alfabéticamente.

```sql
	SELECT cliente.id, cliente.nombre, cliente.apellido1, pedido.id AS pedido_id, pedido.total, pedido.fecha FROM cliente
	JOIN pedido ON cliente.id = pedido.id_cliente
	ORDER BY nombre,apellido1,apellido2;
```

```sql	
	 id | nombre | apellido1 | pedido_id |  total  |   fecha    
	----+--------+-----------+-----------+---------+------------
	  1 | Aarón  | Rivero    |        16 | 2389.23 | 2019-03-11
	  1 | Aarón  | Rivero    |         2 |  270.65 | 2016-09-10
	  1 | Aarón  | Rivero    |        15 |  370.85 | 2019-03-11
	  2 | Adela  | Salas     |        12 |  3045.6 | 2017-04-25
	  2 | Adela  | Salas     |         3 |   65.26 | 2017-10-05
	  2 | Adela  | Salas     |         7 |    5760 | 2015-09-10	
	  3 | Adolfo | Rubio     |        11 |   75.29 | 2016-08-17
	  4 | Adrián | Suárez    |         8 | 1983.43 | 2017-10-10
	  5 | Marcos | Loyola    |         1 |   150.5 | 2017-10-05
	  5 | Marcos | Loyola    |         5 |   948.5 | 2017-09-10
	  6 | María  | Santana   |        13 |  545.75 | 2019-01-25
	  6 | María  | Santana   |        14 |  145.82 | 2017-02-02
	  8 | Pepe   | Ruiz      |        10 |  250.45 | 2015-06-27
	  8 | Pepe   | Ruiz      |         9 |  2480.4 | 2016-10-10
	  8 | Pepe   | Ruiz      |         4 |   110.5 | 2016-08-17
	  7 | Pilar  | Ruiz      |         6 |  2400.6 | 2016-07-27
		 
	 
```

3. Devuelve un listado que muestre todos los pedidos en los que ha participado un comercial. El resultado debe mostrar todos los datos de los pedidos y de los comerciales. El listado debe mostrar los datos de los comerciales ordenados alfabéticamente.

```sql
	SELECT comercial.*, pedido.* FROM pedido
	JOIN comercial ON pedido.id_comercial = comercial.id
	WHERE pedido.id_comercial IS NOT NULL
	ORDER BY comercial.nombre ASC;
```

```sql	

	 id | nombre  | apellido1 | apellido2 | comision | id |  total  |   fecha    | id_cliente | id_comercial 
	----+---------+-----------+-----------+----------+----+---------+------------+------------+--------------
	  5 | Antonio | Carretero | Ortega    |     0.12 | 16 | 2389.23 | 2019-03-11 |          1 |            5
	  5 | Antonio | Carretero | Ortega    |     0.12 | 15 |  370.85 | 2019-03-11 |          1 |            5
	  5 | Antonio | Carretero | Ortega    |     0.12 |  2 |  270.65 | 2016-09-10 |          1 |            5
	  7 | Antonio | Vega      | Hernández |     0.11 | 11 |   75.29 | 2016-08-17 |          3 |            7
	  1 | Daniel  | Sáez      | Vega      |     0.15 |  7 |    5760 | 2015-09-10 |          2 |            1
	  1 | Daniel  | Sáez      | Vega      |     0.15 | 14 |  145.82 | 2017-02-02 |          6 |            1
	  1 | Daniel  | Sáez      | Vega      |     0.15 | 13 |  545.75 | 2019-01-25 |          6 |            1
	  1 | Daniel  | Sáez      | Vega      |     0.15 | 12 |  3045.6 | 2017-04-25 |          2 |            1
	  1 | Daniel  | Sáez      | Vega      |     0.15 |  3 |   65.26 | 2017-10-05 |          2 |            1
	  1 | Daniel  | Sáez      | Vega      |     0.15 |  6 |  2400.6 | 2016-07-27 |          7 |            1
	  3 | Diego   | Flores    | Salas     |     0.11 |  9 |  2480.4 | 2016-10-10 |          8 |            3
	  3 | Diego   | Flores    | Salas     |     0.11 |  4 |   110.5 | 2016-08-17 |          8 |            3
	  2 | Juan    | Gómez     | López     |     0.13 | 10 |  250.45 | 2015-06-27 |          8 |            2
	  2 | Juan    | Gómez     | López     |     0.13 |  1 |   150.5 | 2017-10-05 |          5 |            2
	  2 | Juan    | Gómez     | López     |     0.13 |  5 |   948.5 | 2017-09-10 |          5 |            2
	  6 | Manuel  | Domínguez | Hernández |     0.13 |  8 | 1983.43 | 2017-10-10 |          4 |            6
	 
	 
```

4. Devuelve un listado que muestre todos los clientes, con todos los pedidos que han realizado y con los datos de los comerciales asociados a cada pedido.

```sql
	SELECT cliente.*, pedido.id AS id_pedido, pedido.total, pedido.fecha, comercial.* FROM cliente
	JOIN pedido ON cliente.id = pedido.id_comercial
	JOIN comercial ON pedido.id_comercial = comercial.id
	ORDER BY cliente.nombre;
	
```

```sql	
	 id | nombre | apellido1 | apellido2 | ciudad  | categoria | id_pedido |  total  |   fecha    | id | nombre  | apellido1 | apellido2 | comision 
	----+--------+-----------+-----------+---------+-----------+-----------+---------+------------+----+---------+-----------+-----------+----------
	  1 | Aarón  | Rivero    | Gómez     | Almería |       100 |         7 |    5760 | 2015-09-10 |  1 | Daniel  | Sáez      | Vega      |     0.15
	  1 | Aarón  | Rivero    | Gómez     | Almería |       100 |        13 |  545.75 | 2019-01-25 |  1 | Daniel  | Sáez      | Vega      |     0.15
	  1 | Aarón  | Rivero    | Gómez     | Almería |       100 |         3 |   65.26 | 2017-10-05 |  1 | Daniel  | Sáez      | Vega      |     0.15
	  1 | Aarón  | Rivero    | Gómez     | Almería |       100 |        14 |  145.82 | 2017-02-02 |  1 | Daniel  | Sáez      | Vega      |     0.15
	  1 | Aarón  | Rivero    | Gómez     | Almería |       100 |        12 |  3045.6 | 2017-04-25 |  1 | Daniel  | Sáez      | Vega      |     0.15
	  1 | Aarón  | Rivero    | Gómez     | Almería |       100 |         6 |  2400.6 | 2016-07-27 |  1 | Daniel  | Sáez      | Vega      |     0.15
	  2 | Adela  | Salas     | Díaz      | Granada |       200 |         1 |   150.5 | 2017-10-05 |  2 | Juan    | Gómez     | López     |     0.13
	  2 | Adela  | Salas     | Díaz      | Granada |       200 |         5 |   948.5 | 2017-09-10 |  2 | Juan    | Gómez     | López     |     0.13
	  2 | Adela  | Salas     | Díaz      | Granada |       200 |        10 |  250.45 | 2015-06-27 |  2 | Juan    | Gómez     | López     |     0.13
	  3 | Adolfo | Rubio     | Flores    | Sevilla |           |         9 |  2480.4 | 2016-10-10 |  3 | Diego   | Flores    | Salas     |     0.11
	  3 | Adolfo | Rubio     | Flores    | Sevilla |           |         4 |   110.5 | 2016-08-17 |  3 | Diego   | Flores    | Salas     |     0.11
	  5 | Marcos | Loyola    | Méndez    | Almería |       200 |         2 |  270.65 | 2016-09-10 |  5 | Antonio | Carretero | Ortega    |     0.12
	  5 | Marcos | Loyola    | Méndez    | Almería |       200 |        15 |  370.85 | 2019-03-11 |  5 | Antonio | Carretero | Ortega    |     0.12
	  5 | Marcos | Loyola    | Méndez    | Almería |       200 |        16 | 2389.23 | 2019-03-11 |  5 | Antonio | Carretero | Ortega    |     0.12
	  6 | María  | Santana   | Moreno    | Cádiz   |       100 |         8 | 1983.43 | 2017-10-10 |  6 | Manuel  | Domínguez | Hernández |     0.13
	  7 | Pilar  | Ruiz      |           | Sevilla |       300 |        11 |   75.29 | 2016-08-17 |  7 | Antonio | Vega      | Hernández |     0.11
 
	 
```

5. Devuelve un listado de todos los clientes que realizaron un pedido durante el año 2017, cuya cantidad esté entre 300 € y 1000 €.

```sql
	SELECT DISTINCT cliente.nombre,cliente.apellido1,cliente.apellido2, pedido.total,pedido.fecha FROM cliente
	JOIN pedido ON cliente.id = pedido.id_cliente
	WHERE EXTRACT (YEAR FROM fecha) = 2017 AND pedido.total BETWEEN 300 AND 1000;
```

```sql	
	 nombre | apellido1 | apellido2 | total |   fecha    
	--------+-----------+-----------+-------+------------
	 Marcos | Loyola    | Méndez    | 948.5 | 2017-09-10
 
	 
```

6. Devuelve el nombre y los apellidos de todos los comerciales que ha participado en algún pedido realizado por María Santana Moreno. 

```sql
	SELECT DISTINCT comercial.nombre,comercial.apellido1,comercial.apellido2 FROM comercial
	JOIN pedido ON comercial.id = pedido.id_comercial
	JOIN cliente ON pedido.id_cliente = cliente.id
	WHERE cliente.nombre= 'María' AND cliente.apellido1= 'Santana' AND cliente.apellido2 = 'Moreno';
		
```

```sql	
	 nombre | apellido1 | apellido2 
	--------+-----------+-----------
	 Daniel | Sáez      | Vega 
	 
```

7. Devuelve el nombre de todos los clientes que han realizado algún pedido con el comercial Daniel Sáez Vega.

```sql
	SELECT DISTINCT cliente.nombre FROM cliente
	JOIN pedido ON cliente.id = pedido.id_cliente
	JOIN comercial ON pedido.id_comercial = comercial.id
	WHERE comercial.nombre='Daniel' AND comercial.apellido1 = 'Sáez' AND comercial.apellido2 = 'Vega';
```

```sql	

	 nombre 
	--------
	 Adela
	 María
	 Pilar
 
	 
```

# Consultas multitabla (Composición externa)

1. Devuelve un listado con todos los clientes junto con los datos de los pedidos que han realizado. Este listado también debe incluir los clientes que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los clientes.

```sql
	SELECT DISTINCT cliente.*,pedido.* FROM cliente
	LEFT JOIN pedido ON cliente.id = pedido.id_cliente
	ORDER BY apellido1,apellido2,nombre;
```

```sql	
	 id |  nombre   | apellido1 | apellido2 | ciudad  | categoria | id |  total  |   fecha    | id_cliente | id_comercial 
	----+-----------+-----------+-----------+---------+-----------+----+---------+------------+------------+--------------
	  9 | Guillermo | López     | Gómez     | Granada |       225 |    |         |            |            |             
	  5 | Marcos    | Loyola    | Méndez    | Almería |       200 |  1 |   150.5 | 2017-10-05 |          5 |            2
	  5 | Marcos    | Loyola    | Méndez    | Almería |       200 |  5 |   948.5 | 2017-09-10 |          5 |            2
	  1 | Aarón     | Rivero    | Gómez     | Almería |       100 |  2 |  270.65 | 2016-09-10 |          1 |            5
	  1 | Aarón     | Rivero    | Gómez     | Almería |       100 | 15 |  370.85 | 2019-03-11 |          1 |            5
	  1 | Aarón     | Rivero    | Gómez     | Almería |       100 | 16 | 2389.23 | 2019-03-11 |          1 |            5
	  3 | Adolfo    | Rubio     | Flores    | Sevilla |           | 11 |   75.29 | 2016-08-17 |          3 |            7
	  8 | Pepe      | Ruiz      | Santana   | Huelva  |       200 |  4 |   110.5 | 2016-08-17 |          8 |            3
	  8 | Pepe      | Ruiz      | Santana   | Huelva  |       200 |  9 |  2480.4 | 2016-10-10 |          8 |            3
	  8 | Pepe      | Ruiz      | Santana   | Huelva  |       200 | 10 |  250.45 | 2015-06-27 |          8 |            2
	  7 | Pilar     | Ruiz      |           | Sevilla |       300 |  6 |  2400.6 | 2016-07-27 |          7 |            1
	  2 | Adela     | Salas     | Díaz      | Granada |       200 |  3 |   65.26 | 2017-10-05 |          2 |            1
	  2 | Adela     | Salas     | Díaz      | Granada |       200 |  7 |    5760 | 2015-09-10 |          2 |            1
	  2 | Adela     | Salas     | Díaz      | Granada |       200 | 12 |  3045.6 | 2017-04-25 |          2 |            1
	 10 | Daniel    | Santana   | Loyola    | Sevilla |       125 |    |         |            |            |             
	  6 | María     | Santana   | Moreno    | Cádiz   |       100 | 13 |  545.75 | 2019-01-25 |          6 |            1
	  6 | María     | Santana   | Moreno    | Cádiz   |       100 | 14 |  145.82 | 2017-02-02 |          6 |            1
	  4 | Adrián    | Suárez    |           | Jaén    |       300 |  8 | 1983.43 | 2017-10-10 |          4 |            6
	 
	 
```

2. Devuelve un listado con todos los comerciales junto con los datos de los pedidos que han realizado. Este listado también debe incluir los comerciales que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los comerciales.

```sql
	SELECT DISTINCT comercial.* , pedido.* FROM comercial
	LEFT JOIN pedido ON comercial.id = pedido.id_comercial
	ORDER BY apellido1,apellido2,nombre;
```

```sql	
	 id | nombre  | apellido1 | apellido2 | comision | id |  total  |   fecha    | id_cliente | id_comercial 
	----+---------+-----------+-----------+----------+----+---------+------------+------------+--------------
	  5 | Antonio | Carretero | Ortega    |     0.12 |  2 |  270.65 | 2016-09-10 |          1 |            5
	  5 | Antonio | Carretero | Ortega    |     0.12 | 15 |  370.85 | 2019-03-11 |          1 |            5
	  5 | Antonio | Carretero | Ortega    |     0.12 | 16 | 2389.23 | 2019-03-11 |          1 |            5
	  6 | Manuel  | Domínguez | Hernández |     0.13 |  8 | 1983.43 | 2017-10-10 |          4 |            6
	  3 | Diego   | Flores    | Salas     |     0.11 |  4 |   110.5 | 2016-08-17 |          8 |            3
	  3 | Diego   | Flores    | Salas     |     0.11 |  9 |  2480.4 | 2016-10-10 |          8 |            3
	  2 | Juan    | Gómez     | López     |     0.13 |  1 |   150.5 | 2017-10-05 |          5 |            2
	  2 | Juan    | Gómez     | López     |     0.13 |  5 |   948.5 | 2017-09-10 |          5 |            2
	  2 | Juan    | Gómez     | López     |     0.13 | 10 |  250.45 | 2015-06-27 |          8 |            2
	  4 | Marta   | Herrera   | Gil       |     0.14 |    |         |            |            |             
	  8 | Alfredo | Ruiz      | Flores    |     0.05 |    |         |            |            |             
	  1 | Daniel  | Sáez      | Vega      |     0.15 |  3 |   65.26 | 2017-10-05 |          2 |            1
	  1 | Daniel  | Sáez      | Vega      |     0.15 |  6 |  2400.6 | 2016-07-27 |          7 |            1
	  1 | Daniel  | Sáez      | Vega      |     0.15 |  7 |    5760 | 2015-09-10 |          2 |            1
	  1 | Daniel  | Sáez      | Vega      |     0.15 | 12 |  3045.6 | 2017-04-25 |          2 |            1
	  1 | Daniel  | Sáez      | Vega      |     0.15 | 13 |  545.75 | 2019-01-25 |          6 |            1
	  1 | Daniel  | Sáez      | Vega      |     0.15 | 14 |  145.82 | 2017-02-02 |          6 |            1
	  7 | Antonio | Vega      | Hernández |     0.11 | 11 |   75.29 | 2016-08-17 |          3 |            7	 
	 
```

3. Devuelve un listado que solamente muestre los clientes que no han realizado ningún pedido.

```sql
	SELECT * FROM cliente
	LEFT JOIN pedido ON cliente.id = pedido.id_cliente
	WHERE pedido.id_cliente IS NULL;
```

```sql	
	 id |  nombre   | apellido1 | apellido2 | ciudad  | categoria | id | total | fecha | id_cliente | id_comercial 
	----+-----------+-----------+-----------+---------+-----------+----+-------+-------+------------+--------------
	 10 | Daniel    | Santana   | Loyola    | Sevilla |       125 |    |       |       |            |             
	  9 | Guillermo | López     | Gómez     | Granada |       225 |    |       |       |            |             
 
	 
```


4. Devuelve un listado que solamente muestre los comerciales que no han realizado ningún pedido.

```sql
	SELECT * FROM comercial
	LEFT JOIN pedido ON comercial.id = pedido.id_comercial
	WHERE pedido.id_comercial IS NULL;
```

```sql	

	 id | nombre  | apellido1 | apellido2 | comision | id | total | fecha | id_cliente | id_comercial 
	----+---------+-----------+-----------+----------+----+-------+-------+------------+--------------
	  8 | Alfredo | Ruiz      | Flores    |     0.05 |    |       |       |            |             
	  4 | Marta   | Herrera   | Gil       |     0.14 |    |       |       |            |           	           
 
	 
```

5. Devuelve un listado con los clientes que no han realizado ningún pedido y de los comerciales que no han participado en ningún pedido. Ordene el listado alfabéticamente por los apellidos y el nombre. En en listado deberá diferenciar de algún modo los clientes y los comerciales.

```sql
	SELECT 'Cliente' AS tipo, cliente.nombre, cliente.apellido1, cliente.apellido2
	FROM cliente
	LEFT JOIN pedido ON cliente.id = pedido.id_cliente
	WHERE pedido.id IS NULL
	UNION ALL
	SELECT 'Comercial' AS tipo, comercial.nombre, comercial.apellido1, comercial.apellido2
	FROM comercial
	LEFT JOIN pedido ON comercial.id = pedido.id_comercial
	WHERE pedido.id IS NULL
	ORDER BY apellido1, apellido2, nombre;
```

```sql	
	   tipo    |  nombre   | apellido1 | apellido2 
	-----------+-----------+-----------+-----------
	 Comercial | Marta     | Herrera   | Gil
	 Cliente   | Guillermo | López     | Gómez
	 Comercial | Alfredo   | Ruiz      | Flores
	 Cliente   | Daniel    | Santana   | Loyola
	 
```


6. ¿Se podrían realizar las consultas anteriores con NATURAL LEFT JOIN o NATURAL RIGHT JOIN? Justifique su respuesta.

```sql
No es posible ya que el NATURAL obliga o valida que en ambas tablas donde se este haciendo el JOIN tenga las mismas columnas y con los mismos nombres en este caso tocaría modificar todas las tablas lo cual no es viable.
```

# Consultas resumen


1. Calcula la cantidad total que suman todos los pedidos que aparecen en la tabla pedido.

```sql
	SELECT SUM(pedido.total) AS total_pedidos FROM pedido;
```

```sql	
	   total_pedidos    
	--------------------
	 20992.829999999998
	 
```


2. Calcula la cantidad media de todos los pedidos que aparecen en la tabla pedido.

```sql
	SELECT AVG(pedido.total) AS media_pedidos FROM pedido;
```

```sql	

	   media_pedidos    
	--------------------
	 1312.0518749999999
	 
```



3. Calcula el número total de comerciales distintos que aparecen en la tabla pedido.

```sql
	SELECT  COUNT(DISTINCT id_comercial) AS total_comerciales FROM pedido;
	
```

```sql	

	 total_comerciales 
	-------------------
		         6
 	 
```



4. Calcula el número total de clientes que aparecen en la tabla cliente.

```sql
	SELECT COUNT(cliente.id) AS clientes_totales FROM cliente;	
```

```sql	
	 clientes_totales 
	------------------
		       10
		 
```


5. Calcula cuál es la mayor cantidad que aparece en la tabla pedido.

```sql
	SELECT MAX(pedido.total) AS mayor_valor FROM pedido;
```

```sql	

	 mayor_valor 
	-------------
		5760
	 
```


6. Calcula cuál es la menor cantidad que aparece en la tabla pedido.

```sql
	SELECT MIN(pedido.total) AS menor_valor FROM pedido;
```

```sql	
	 menor_valor 
	-------------
	       65.26
	 
```

7. Calcula cuál es el valor máximo de categoría para cada una de las ciudades que aparece en la tabla cliente.

```sql
	SELECT cliente.ciudad, MAX(cliente.categoria) AS valor_maximo FROM cliente
	GROUP BY cliente.ciudad;
	
```

```sql	

	 ciudad  | valor_maximo 
	---------+--------------
	 Almería |          200
	 Huelva  |          200
	 Sevilla |          300
	 Granada |          225
	 Jaén    |          300
	 Cádiz   |          100
	 
```


8. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes. Es decir, el mismo cliente puede haber realizado varios pedidos de diferentes cantidades el mismo día. Se pide que se calcule cuál es el pedido de máximo valor para cada uno de los días en los que un cliente ha realizado un pedido. Muestra el identificador del cliente, nombre, apellidos, la fecha y el valor de la cantidad.

```sql
	SELECT cliente.id,cliente.nombre,cliente.apellido1,cliente.apellido2,pedido.fecha, MAX(pedido.total) AS valor_mayor_total FROM cliente
	JOIN pedido ON cliente.id = pedido.id_cliente
	GROUP BY cliente.id,cliente.nombre,cliente.apellido1,cliente.apellido2,fecha;
```

```sql	

	 id | nombre | apellido1 | apellido2 |   fecha    | valor_mayor_total 
	----+--------+-----------+-----------+------------+-------------------
	  6 | María  | Santana   | Moreno    | 2017-02-02 |            145.82
	  8 | Pepe   | Ruiz      | Santana   | 2015-06-27 |            250.45
	  1 | Aarón  | Rivero    | Gómez     | 2019-03-11 |           2389.23
	  2 | Adela  | Salas     | Díaz      | 2015-09-10 |              5760
	  8 | Pepe   | Ruiz      | Santana   | 2016-10-10 |            2480.4
	  6 | María  | Santana   | Moreno    | 2019-01-25 |            545.75
	  3 | Adolfo | Rubio     | Flores    | 2016-08-17 |             75.29
	  2 | Adela  | Salas     | Díaz      | 2017-04-25 |            3045.6
	  8 | Pepe   | Ruiz      | Santana   | 2016-08-17 |             110.5
	  5 | Marcos | Loyola    | Méndez    | 2017-10-05 |             150.5
	  5 | Marcos | Loyola    | Méndez    | 2017-09-10 |             948.5
	  7 | Pilar  | Ruiz      |           | 2016-07-27 |            2400.6
	  1 | Aarón  | Rivero    | Gómez     | 2016-09-10 |            270.65
	  4 | Adrián | Suárez    |           | 2017-10-10 |           1983.43
	  2 | Adela  | Salas     | Díaz      | 2017-10-05 |             65.26
	 
```

9. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes, teniendo en cuenta que sólo queremos mostrar aquellos pedidos que superen la cantidad de 2000 €.

```sql
	SELECT cliente.id,cliente.nombre,cliente.apellido1,cliente.apellido2,pedido.fecha, MAX(pedido.total) AS valor_mayor_total FROM cliente
	JOIN pedido ON cliente.id = pedido.id_cliente
	GROUP BY cliente.id,cliente.nombre,cliente.apellido1,cliente.apellido2,fecha
	HAVING MAX(pedido.total) > 2000;
```

```sql	
	 id | nombre | apellido1 | apellido2 |   fecha    | valor_mayor_total 
	----+--------+-----------+-----------+------------+-------------------
	  1 | Aarón  | Rivero    | Gómez     | 2019-03-11 |           2389.23
	  2 | Adela  | Salas     | Díaz      | 2015-09-10 |              5760
	  8 | Pepe   | Ruiz      | Santana   | 2016-10-10 |            2480.4
	  2 | Adela  | Salas     | Díaz      | 2017-04-25 |            3045.6
	  7 | Pilar  | Ruiz      |           | 2016-07-27 |            2400.6	 
```


10. Calcula el máximo valor de los pedidos realizados para cada uno de los comerciales durante la fecha 2016-08-17. Muestra el identificador del comercial, nombre, apellidos y total.

```sql
	SELECT comercial.id,comercial.nombre,comercial.apellido1,comercial.apellido2,pedido.fecha, MAX(pedido.total) AS valor_mayor FROM comercial
	JOIN pedido ON pedido.id_comercial = comercial.id
	GROUP BY comercial.id,comercial.nombre,comercial.apellido1,comercial.apellido2,pedido.fecha
	HAVING pedido.fecha ='2016-08-17';
```

```sql	

	 id | nombre  | apellido1 | apellido2 |   fecha    | valor_mayor 
	----+---------+-----------+-----------+------------+-------------
	  3 | Diego   | Flores    | Salas     | 2016-08-17 |       110.5
	  7 | Antonio | Vega      | Hernández | 2016-08-17 |       75.29
  
```

11. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de clientes. Tenga en cuenta que pueden existir clientes que no han realizado ningún pedido. Estos clientes también deben aparecer en el listado indicando que el número de pedidos realizados es 0.

```sql
	SELECT cliente.id,cliente.nombre,cliente.apellido1,cliente.apellido2, COUNT(pedido.id) AS total_pedidos FROM cliente
	LEFT JOIN pedido ON cliente.id = pedido.id_cliente
	GROUP BY cliente.id,cliente.nombre,cliente.apellido1,cliente.apellido2
	
```

```sql	

	 id |  nombre   | apellido1 | apellido2 | total_pedidos 
	----+-----------+-----------+-----------+---------------
	  4 | Adrián    | Suárez    |           |             1
	 10 | Daniel    | Santana   | Loyola    |             0
	  6 | María     | Santana   | Moreno    |             2
	  2 | Adela     | Salas     | Díaz      |             3
	  9 | Guillermo | López     | Gómez     |             0
	  7 | Pilar     | Ruiz      |           |             1
	  3 | Adolfo    | Rubio     | Flores    |             1
	  1 | Aarón     | Rivero    | Gómez     |             3
	  5 | Marcos    | Loyola    | Méndez    |             2
	  8 | Pepe      | Ruiz      | Santana   |             3
	 
```

12. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de clientes durante el año 2017.

```sql
	SELECT cliente.id,cliente.nombre,cliente.apellido1,cliente.apellido2, COUNT(pedido.id) AS total_pedidos FROM cliente
	JOIN pedido ON cliente.id = pedido.id_cliente
	WHERE EXTRACT(YEAR FROM pedido.fecha) = 2017
	GROUP BY cliente.id,cliente.nombre,cliente.apellido1,cliente.apellido2
```

```sql	
	 id | nombre | apellido1 | apellido2 | total_pedidos 
	----+--------+-----------+-----------+---------------
	  2 | Adela  | Salas     | Díaz      |             2
	  4 | Adrián | Suárez    |           |             1
	  5 | Marcos | Loyola    | Méndez    |             2
	  6 | María  | Santana   | Moreno    |             1
  
```


13. Devuelve un listado que muestre el identificador de cliente, nombre, primer apellido y el valor de la máxima cantidad del pedido realizado por cada uno de los clientes. El resultado debe mostrar aquellos clientes que no han realizado ningún pedido indicando que la máxima cantidad de sus pedidos realizados es 0. Puede hacer uso de la función IFNULL.

```sql
	SELECT cliente.id AS identificador_cliente, cliente.nombre, cliente.apellido1,cliente.apellido2,COALESCE(MAX(pedido.total), 0) AS maxima_cantidad_pedido FROM cliente
	LEFT JOIN pedido ON cliente.id = pedido.id_cliente
	GROUP BY cliente.id, cliente.nombre, cliente.apellido1,cliente.apellido2;
```

```sql	
	 identificador_cliente |  nombre   | apellido1 | apellido2 | maxima_cantidad_pedido 
-----------------------+-----------+-----------+-----------+------------------------
                     4 | Adrián    | Suárez    |           |                1983.43
                    10 | Daniel    | Santana   | Loyola    |                      0
                     6 | María     | Santana   | Moreno    |                 545.75
                     2 | Adela     | Salas     | Díaz      |                   5760
                     9 | Guillermo | López     | Gómez     |                      0
                     7 | Pilar     | Ruiz      |           |                 2400.6
                     3 | Adolfo    | Rubio     | Flores    |                  75.29
                     1 | Aarón     | Rivero    | Gómez     |                2389.23
                     5 | Marcos    | Loyola    | Méndez    |                  948.5
                     8 | Pepe      | Ruiz      | Santana   |                 2480.4
	 
```


14. Devuelve cuál ha sido el pedido de máximo valor que se ha realizado cada año.

```sql
	SELECT pedido.id AS id_pedido, pedido.total, pedido.fecha FROM pedido
	JOIN (
	    SELECT EXTRACT(YEAR FROM fecha) AS anio, MAX(total) AS max_total
	    FROM pedido
	    GROUP BY EXTRACT(YEAR FROM fecha)
	) AS max_por_anio ON EXTRACT(YEAR FROM pedido.fecha) = max_por_anio.anio AND pedido.total = max_por_anio.max_total;

```

```sql	
	 id_pedido |  total  |   fecha    
	-----------+---------+------------
		 7 |    5760 | 2015-09-10
		 9 |  2480.4 | 2016-10-10
		12 |  3045.6 | 2017-04-25
		16 | 2389.23 | 2019-03-11	 
```


15. Devuelve el número total de pedidos que se han realizado cada año.

```sql
	SELECT EXTRACT(YEAR FROM fecha) AS año, COUNT(*) AS total_pedidos
	FROM pedido
	GROUP BY EXTRACT(YEAR FROM fecha);
```

```sql	

	 año  | total_pedidos 
	------+---------------
	 2017 |             6
	 2016 |             5
	 2019 |             3
	 2015 |             2

	 
```

# Con operadores básicos de comparación

1. Devuelve un listado con todos los pedidos que ha realizado Adela Salas Díaz. (Sin utilizar INNER JOIN). 

```sql
	SELECT * FROM pedido
	WHERE id_cliente = (
	    SELECT id
	    FROM cliente
	    WHERE nombre = 'Adela' AND apellido1 = 'Salas' AND apellido2 = 'Díaz'
	);

```

```sql	
	 id | total  |   fecha    | id_cliente | id_comercial 
	----+--------+------------+------------+--------------
	  3 |  65.26 | 2017-10-05 |          2 |            1
	  7 |   5760 | 2015-09-10 |          2 |            1
	 12 | 3045.6 | 2017-04-25 |          2 |            1
 
	 
```


2. Devuelve el número de pedidos en los que ha participado el comercial Daniel Sáez Vega. (Sin utilizar INNER JOIN)

```sql
	
SELECT COUNT(*) AS total_ventas_realizadas FROM pedido
WHERE id_comercial = (
    SELECT id
    FROM comercial
    WHERE nombre = 'Daniel' AND apellido1 = 'Sáez' AND apellido2 = 'Vega'
);
```

```sql	

 total_ventas_realizadas 
-------------------------
                       6
 
	 
```


3. Devuelve los datos del cliente que realizó el pedido más caro en el año 2019. (Sin utilizar INNER JOIN)

```sql
	SELECT * FROM cliente
	WHERE id = (
	    SELECT id_cliente
	    FROM pedido
	    WHERE EXTRACT(YEAR FROM fecha) = 2019
	    ORDER BY total DESC
	    LIMIT 1
	);
```

```sql
	
	 id | nombre | apellido1 | apellido2 | ciudad  | categoria 
	----+--------+-----------+-----------+---------+-----------
	  1 | Aarón  | Rivero    | Gómez     | Almería |       100
	 
```


4. Devuelve la fecha y la cantidad del pedido de menor valor realizado por el cliente Pepe Ruiz Santana.

```sql
	SELECT fecha, total FROM pedido
	WHERE id_cliente = (
	    SELECT id
	    FROM cliente
	    WHERE nombre = 'Pepe' AND apellido1 = 'Ruiz' AND apellido2 = 'Santana'
	)
	ORDER BY total ASC
	LIMIT 1;
```

```sql	
	   fecha    | total 
	------------+-------
	 2016-08-17 | 110.5
	 
```



5. Devuelve un listado con los datos de los clientes y los pedidos, de todos los clientes que han realizado un pedido durante el año 2017 con un valor mayor o igual al valor medio de los pedidos realizados durante ese mismo año.

```sql
SELECT cliente.id AS id_cliente, cliente.nombre, cliente.apellido1, cliente.apellido2,pedido.id AS id_pedido, pedido.total, pedido.fecha FROM cliente, pedido
WHERE cliente.id = pedido.id_cliente
      AND EXTRACT(YEAR FROM pedido.fecha) = 2017
      AND pedido.total >= (SELECT AVG(total) FROM pedido WHERE EXTRACT(YEAR FROM fecha) = 2017);	
```

```sql	
	 id_cliente | nombre | apellido1 | apellido2 | id_pedido |  total  |   fecha    
	------------+--------+-----------+-----------+-----------+---------+------------
		  2 | Adela  | Salas     | Díaz      |        12 |  3045.6 | 2017-04-25
		  4 | Adrián | Suárez    |           |         8 | 1983.43 | 2017-10-10
		 
```

# Subconsultas con ALL y ANY

1. Devuelve el pedido más caro que existe en la tabla pedido si hacer uso de MAX, ORDER BY ni LIMIT.

```sql

	SELECT pedido.id, pedido.total, pedido.fecha, pedido.id_cliente, pedido.id_comercial FROM pedido
	WHERE NOT EXISTS (
	    SELECT 1
	    FROM pedido AS p
	    WHERE p.total > pedido.total
	);

```

```sql	

	id | total |   fecha    | id_cliente | id_comercial 
	----+-------+------------+------------+--------------
	  7 |  5760 | 2015-09-10 |          2 |            1
		 
```

2. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando ANY o ALL). 

```sql
	SELECT id, nombre, apellido1, apellido2 FROM cliente
	WHERE id <> ALL (
	    SELECT id_cliente
	    FROM pedido
	);
```

```sql	
	 id |  nombre   | apellido1 | apellido2 
	----+-----------+-----------+-----------
	  9 | Guillermo | López     | Gómez
	 10 | Daniel    | Santana   | Loyola

		 
```

3. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando ANY o ALL).

```sql
	SELECT id, nombre, apellido1, apellido2 FROM comercial
	WHERE id <> ALL (
	    SELECT id_comercial
	    FROM pedido
	);

```

```sql	

	 id | nombre  | apellido1 | apellido2 
	----+---------+-----------+-----------
	  4 | Marta   | Herrera   | Gil
	  8 | Alfredo | Ruiz      | Flores

		 
```
# Subconsultas con IN y NOT IN

1. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando IN o NOT IN).

```sql
	SELECT id, nombre, apellido1, apellido2 FROM cliente
	WHERE id NOT IN (
	    SELECT DISTINCT id_cliente
	    FROM pedido
	);	
```

```sql	
	 id |  nombre   | apellido1 | apellido2 
	----+-----------+-----------+-----------
	  9 | Guillermo | López     | Gómez
	 10 | Daniel    | Santana   | Loyola
		 
```

2. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando IN o NOT IN).

```sql
	SELECT id, nombre, apellido1, apellido2 FROM comercial
	WHERE id NOT IN (
	    SELECT DISTINCT id_comercial
	    FROM pedido
	);

```

```sql	
	 id | nombre  | apellido1 | apellido2 
	----+---------+-----------+-----------
	  4 | Marta   | Herrera   | Gil
	  8 | Alfredo | Ruiz      | Flores

		 
```

# Subconsultas con EXISTS y NOT EXISTS

1. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando EXISTS o NOT EXISTS).

```sql
	SELECT id, nombre, apellido1, apellido2 FROM cliente
	WHERE NOT EXISTS (
	    SELECT 1
	    FROM pedido
	    WHERE pedido.id_cliente = cliente.id
	);
```

```sql	

	 id |  nombre   | apellido1 | apellido2 
	----+-----------+-----------+-----------
	 10 | Daniel    | Santana   | Loyola
	  9 | Guillermo | López     | Gómez
		 
```

2. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando EXISTS o NOT EXISTS).

```sql
SELECT id, nombre, apellido1, apellido2 FROM comercial
WHERE NOT EXISTS (
    SELECT 1
    FROM pedido
    WHERE pedido.id_comercial = comercial.id
);	
```

```sql	
	 id | nombre  | apellido1 | apellido2 
	----+---------+-----------+-----------
	  8 | Alfredo | Ruiz      | Flores
	  4 | Marta   | Herrera   | Gil

```
