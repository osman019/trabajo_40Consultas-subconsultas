
OSMAN JOHANDRY ORTIZ ROLON 



TRABAJO (CONSULTAS, CONSULTAS MULTITABLAS, SUBCONSULTAS)


MODELO FISICO(msql)

CREATE TABLE `clientes` (
  `cliente_id` int NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50) DEFAULT NULL,
  `email` varchar(50) DEFAULT NULL,
  `telefono` varchar(15) DEFAULT NULL,
  `direccion` varchar(100) DEFAULT NULL,
  `ciudad` varchar(50) DEFAULT NULL,
  `pais` varchar(50) DEFAULT NULL,
  `fecha_registro` date DEFAULT NULL,
  PRIMARY KEY (`cliente_id`),
  UNIQUE KEY `email` (`email`)
)

CREATE TABLE `detalles_pedidos` (
  `detalle_id` int NOT NULL AUTO_INCREMENT,
  `pedido_id` int DEFAULT NULL,
  `producto_id` int DEFAULT NULL,
  `cantidad` int DEFAULT NULL,
  `precio_unitario` decimal(10,2) DEFAULT NULL,
  PRIMARY KEY (`detalle_id`),
  KEY `pedido_id` (`pedido_id`),
  KEY `producto_id` (`producto_id`),
  CONSTRAINT `detalles_pedidos_ibfk_1` FOREIGN KEY (`pedido_id`) REFERENCES `pedidos` (`pedido_id`),
  CONSTRAINT `detalles_pedidos_ibfk_2` FOREIGN KEY (`producto_id`) REFERENCES `productos` (`producto_id`)
)

CREATE TABLE `empleados` (
  `empleado_id` int NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50) DEFAULT NULL,
  `puesto` varchar(50) DEFAULT NULL,
  `fecha_contratacion` date DEFAULT NULL,
  `salario` decimal(10,2) DEFAULT NULL,
  PRIMARY KEY (`empleado_id`)
)
CREATE TABLE `pedidos` (
  `pedido_id` int NOT NULL AUTO_INCREMENT,
  `cliente_id` int DEFAULT NULL,
  `empleado_id` int DEFAULT NULL,
  `fecha_pedido` date DEFAULT NULL,
  `estado` varchar(20) DEFAULT NULL,
  PRIMARY KEY (`pedido_id`),
  KEY `cliente_id` (`cliente_id`),
  KEY `empleado_id` (`empleado_id`),
  CONSTRAINT `pedidos_ibfk_1` FOREIGN KEY (`cliente_id`) REFERENCES `clientes` (`cliente_id`),
  CONSTRAINT `pedidos_ibfk_2` FOREIGN KEY (`empleado_id`) REFERENCES `empleados` (`empleado_id`)
)

CREATE TABLE `productos` (
  `producto_id` int NOT NULL AUTO_INCREMENT,
  `nombre` varchar(50) DEFAULT NULL,
  `categoria` varchar(50) DEFAULT NULL,
  `precio` decimal(10,2) DEFAULT NULL,
  `stock` int DEFAULT NULL,
  PRIMARY KEY (`producto_id`)
) 

INSERCCIONES NECESARIAS PARA ALGUNAS CONSULTAS:


INSERT INTO productos (nombre, categoria, precio, stock) 
VALUES 
('Dron', 'Electronica', 100.00, 10);
('Patineta', 'juguete', 100.00, 10);

INSERT INTO pedidos (cliente_id, empleado_id, fecha_pedido, estado) VALUES
(1, 1, '2024-01-15', 'Completado'),
(2, 1, '2024-02-20', 'Pendiente'),
(3, 2, '2024-03-10', 'Completado'),
(1, 2, '2024-04-05', 'En Proceso'),
(4, 1, '2024-05-25', 'Completado'),
(5, 3, '2024-06-15', 'Cancelado'),
(6, 2, '2024-07-22', 'Pendiente'),
(2, 3, '2024-08-30', 'Completado'),
(7, 1, '2024-09-12', 'En Proceso'),
(3, 1, '2024-10-18', 'Completado'),
(8, 2, '2024-11-05', 'Pendiente'),
(9, 3, '2024-12-11', 'Completado');

INSERT INTO clientes (nombre, email, telefono, direccion, ciudad, pais,
fecha_registro) VALUES
('osman ortiz', 'osmanortizrolon@gmail.com', '3222167943', 'Calle 103', 'tibu', 'colombia' ,
'2024-10-01'),
('Juan perez', 'juan.perez@hotmail.com', '5909-5678', 'Avenida 95', 'Barcelona',
'España', '2022-11-24'),

INSERT INTO empleados (nombre, puesto, fecha_contratacion, salario) VALUES
('stiven', 'guardia', '2024-05-10', 1000.00),
('Marta Stefany', 'aseadora', '2023-08-20', 2200.00);

INSERT INTO pedidos (cliente_id, empleado_id, fecha_pedido, estado) VALUES
(21, 1, '2024-10-01', 'Completado');

INSERT INTO detalles_pedidos (pedido_id, producto_id, cantidad, precio_unitario)
VALUES
(81, 1, 2, 999.99);




CONSULTAS BASICAS: 

1. Consulta todos los datos de la tabla clientes para ver la lista completa de clientes.

SELECT * FROM clientes;

2. Muestra los nombres y correos electrónicos de todos los clientes que residen en la ciudad de Madrid.

SELECT nombre, correo
FROM clientes
WHERE ciudad = 'Madrid';


3. Obtén una lista de productos con un precio mayor a $100, mostrando solo el nombre y el precio.

SELECT nombre, precio
FROM productos
WHERE precio > 100;


4. Encuentra todos los empleados que tienen un salario superior a $2500, mostrando su nombre, puesto y salario.

SELECT nombre, puesto, salario
FROM empleados
WHERE salario > 2500;


5. Lista los nombres de los productos en la categoría "Electrónica", ordenados alfabéticamente.

SELECT nombre
FROM productos
WHERE categoria = 'Electrónica'
ORDER BY nombre ASC;



6. Muestra los detalles de los pedidos que están en estado "Pendiente", incluyendo el ID del pedido, el ID del cliente y la fecha del pedido.

SELECT pedido_id, cliente_id, fecha
FROM pedidos
WHERE estado = 'Pendiente';



7. Encuentra el nombre y el precio del producto más caro en la base de datos.

SELECT nombre, precio
FROM productos
ORDER BY precio DESC
LIMIT 1;



8. Obtén el total de pedidos realizados por cada cliente, mostrando el ID del cliente y el total de pedidos.

SELECT cliente_id, COUNT(*) AS total_pedidos
FROM pedidos
GROUP BY cliente_id;


9. Calcula el promedio de salario de todos los empleados en la empresa.

SELECT AVG(salario) AS promedio_salario
FROM empleados;



10. Encuentra el número de productos en cada categoría, mostrando la categoría y el número de productos.

SELECT categoria, COUNT(*) AS numero_productos
FROM productos
GROUP BY categoria;

CONSULTA MULTITABLA:

11. Encuentra los nombres de los clientes y los detalles de sus pedidos.

SELECT c.nombre AS nombre_cliente, p.*
FROM clientes c
JOIN pedidos p ON c.cliente_id = p.cliente_id;


12. Lista todos los productos pedidos junto con el precio unitario de cada pedido.

SELECT dp.producto_id, dp.precio_unitario
FROM detalles_pedidos dp;


13. Encuentra los nombres de los clientes y los nombres de los empleados que gestionaron sus pedidos.

SELECT c.nombre AS nombre_cliente, e.nombre AS nombre_empleado
FROM pedidos p
JOIN clientes c ON p.cliente_id = c.cliente_id
JOIN empleados e ON p.empleado_id = e.empleado_id;



14. Muestra todos los pedidos y, si existen, los productos en cada pedido, incluyendo los pedidos sin productos usando LEFT JOIN.

SELECT p.pedido_id, dp.producto_id, dp.cantidad
FROM pedidos p
LEFT JOIN detalles_pedidos dp ON p.pedido_id = dp.pedido_id;



15. Encuentra los productos y, si existen, los detalles de pedidos en los que no se ha incluido el producto usando RIGHT JOIN.

SELECT p.producto_id, dp.pedido_id, dp.cantidad
FROM productos p
RIGHT JOIN detalles_pedidos dp ON p.producto_id = dp.producto_id
WHERE dp.pedido_id IS NULL;




16. Lista todos los empleados junto con los pedidos que han gestionado, si existen, usando LEFT JOIN para ver los empleados sin pedidos.

SELECT e.empleado_id, e.nombre AS nombre_empleado, p.pedido_id
FROM empleados e
LEFT JOIN pedidos p ON e.empleado_id = p.empleado_id;


17. Encuentra los empleados que no han gestionado ningún pedido usando un LEFT JOIN combinado con WHERE.

SELECT e.empleado_id, e.nombre AS nombre_empleado
FROM empleados e
LEFT JOIN pedidos p ON e.empleado_id = p.empleado_id
WHERE p.pedido_id IS NULL;


18. Calcula el total gastado en cada pedido, mostrando el ID del pedido y el total, usando JOIN.

SELECT dp.pedido_id, SUM(dp.cantidad * dp.precio_unitario) AS total_gastado
FROM detalles_pedidos dp
GROUP BY dp.pedido_id;



19. Realiza un CROSS JOIN entre clientes y productos para mostrar todas las combinaciones posibles de clientes y productos.

SELECT c.nombre AS nombre_cliente, p.nombre AS nombre_producto
FROM clientes c
CROSS JOIN productos p;



20. Encuentra los nombres de los clientes y los productos que han comprado, si existen, incluyendo los clientes que no han realizado pedidos usando LEFT JOIN.

SELECT c.nombre AS nombre_cliente, p.nombre AS nombre_producto
FROM clientes c
LEFT JOIN pedidos d ON c.cliente_id = d.cliente_id
LEFT JOIN detalles_pedidos dp ON d.pedido_id = dp.pedido_id
LEFT JOIN productos p ON dp.producto_id = p.producto_id;


SUBCONSULTAS:

21. Encuentra los nombres de los clientes que han realizado al menos un pedido de más de $500.

SELECT DISTINCT clientes.nombre
FROM clientes
JOIN pedidos ON clientes.cliente_id = pedidos.cliente_id
JOIN detalles_pedidos ON pedidos.pedido_id = detalles_pedidos.pedido_id
WHERE (detalles_pedidos.cantidad * detalles_pedidos.precio_unitario) > 500;




22. Muestra los productos que nunca han sido pedidos.

SELECT productos.nombre
FROM productos
LEFT JOIN detalles_pedidos ON productos.producto_id = detalles_pedidos.producto_id
WHERE detalles_pedidos.producto_id IS NULL;




23. Lista los empleados que han gestionado pedidos en los últimos 6 meses.

SELECT DISTINCT empleados.nombre
FROM empleados
JOIN pedidos ON empleados.empleado_id = pedidos.empleado_id
WHERE pedidos.fecha_pedido >= DATE_SUB(CURDATE(), INTERVAL 6 MONTH);



24. Encuentra el pedido con el total de ventas más alto.

SELECT p.pedido_id, SUM(dp.cantidad * dp.precio_unitario) AS total_ventas
FROM pedidos p
JOIN detalles_pedidos dp ON p.pedido_id = dp.pedido_id
GROUP BY p.pedido_id
ORDER BY total_ventas DESC
LIMIT 1;


25. Muestra los nombres de los clientes que han realizado más pedidos que el promedio de pedidos de todos los clientes.

SELECT c.nombre
FROM clientes c
JOIN (
    SELECT cliente_id, COUNT(pedido_id) AS pedido_count
    FROM pedidos
    GROUP BY cliente_id
) AS conteo_pedidos ON c.cliente_id = conteo_pedidos.cliente_id
WHERE conteo_pedidos.pedido_count > (
    SELECT AVG(pedido_count)
    FROM (
        SELECT COUNT(pedido_id) AS pedido_count
        FROM pedidos
        GROUP BY cliente_id
    ) AS promedio
);


26. Obtén los productos cuyo precio es superior al precio promedio de todos los productos.

SELECT nombre, precio
FROM productos
WHERE precio > (SELECT AVG(precio) FROM productos);


27. Lista los clientes que han gastado más de $1,000 en total.

SELECT c.nombre, SUM(dp.cantidad * dp.precio_unitario) AS total_gastado
FROM clientes c
JOIN pedidos p ON c.cliente_id = p.cliente_id
JOIN detalles_pedidos dp ON p.pedido_id = dp.pedido_id
GROUP BY c.cliente_id
HAVING total_gastado > 1000;


28. Encuentra los empleados que ganan un salario mayor al promedio de la empresa.

SELECT nombre, salario
FROM empleados
WHERE salario > (SELECT AVG(salario) FROM empleados);


29. Obtén los productos que generaron ingresos mayores al ingreso promedio por producto.

SELECT p.nombre, SUM(dp.cantidad * dp.precio_unitario) AS ingreso_total
FROM productos p
JOIN detalles_pedidos dp ON p.producto_id = dp.producto_id
GROUP BY p.producto_id
HAVING ingreso_total > (
    SELECT AVG(ingreso)
    FROM (
        SELECT SUM(dp2.cantidad * dp2.precio_unitario) AS ingreso
        FROM detalles_pedidos dp2
        GROUP BY dp2.producto_id
    ) AS ingresos
);



30. Encuentra el nombre del cliente que realizó el pedido más reciente.

SELECT c.nombre
FROM clientes c
JOIN pedidos p ON c.cliente_id = p.cliente_id
ORDER BY p.fecha_pedido DESC
LIMIT 1;




31. Muestra los productos pedidos al menos una vez en los últimos 3 meses.

SELECT DISTINCT pr.nombre
FROM productos pr
JOIN detalles_pedidos dp ON pr.producto_id = dp.producto_id
JOIN pedidos p ON dp.pedido_id = p.pedido_id
WHERE p.fecha_pedido >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH);



32. Lista los empleados que no han gestionado ningún pedido.

SELECT nombre
FROM empleados
WHERE empleado_id NOT IN (SELECT empleado_id FROM pedidos);



33. Encuentra los clientes que han comprado más de tres tipos distintos de productos.

SELECT c.nombre
FROM clientes AS c
JOIN pedidos AS p ON c.cliente_id = p.cliente_id
JOIN detalles_pedidos AS dp ON p.pedido_id = dp.pedido_id
GROUP BY c.cliente_id
HAVING COUNT(DISTINCT dp.producto_id) > 3;



34. Muestra el nombre del producto más caro que se ha pedido al menos cinco veces.

SELECT p.nombre
FROM productos AS p
JOIN detalles_pedidos AS dp ON p.producto_id = dp.producto_id
GROUP BY p.producto_id
HAVING COUNT(dp.pedido_id) >= 5
ORDER BY p.precio DESC
LIMIT 1;


35. Lista los clientes cuyo primer pedido fue un año después de su registro.

SELECT c.nombre
FROM clientes AS c
JOIN pedidos AS p ON c.cliente_id = p.cliente_id
WHERE p.fecha_pedido = (
    SELECT MIN(fecha_pedido)
    FROM pedidos
    WHERE cliente_id = c.cliente_id
)
AND p.fecha_pedido >= DATE_ADD(c.fecha_registro, INTERVAL 1 YEAR);



36. Encuentra los nombres de los productos que tienen un stock inferior al promedio del stock de todos los productos.

SELECT nombre
FROM productos
WHERE stock < (SELECT AVG(stock) FROM productos);



37. Lista los clientes que han realizado menos de tres pedidos.

SELECT c.nombre
FROM clientes c
LEFT JOIN pedidos p ON c.cliente_id = p.cliente_id
GROUP BY c.cliente_id
HAVING COUNT(p.pedido_id) < 3;



38. Encuentra los nombres de los productos que fueron pedidos por los clientes que se registraron en el último año.

SELECT DISTINCT p.nombre
FROM productos p
JOIN detalles_pedidos dp ON p.producto_id = dp.producto_id
JOIN pedidos ped ON dp.pedido_id = ped.pedido_id
JOIN clientes c ON ped.cliente_id = c.cliente_id
WHERE c.fecha_registro >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR);



39. Obtén el nombre del empleado que gestionó el mayor número de pedidos.

SELECT e.nombre
FROM empleados e
JOIN pedidos p ON e.empleado_id = p.empleado_id
GROUP BY e.empleado_id
ORDER BY COUNT(p.pedido_id) DESC
LIMIT 1;


40. Lista los productos que han sido comprados en cantidades mayores que el promedio de cantidad de compra de todos los productos.

SELECT p.nombre, SUM(dp.cantidad) AS total_comprado
FROM productos p
JOIN detalles_pedidos dp ON p.producto_id = dp.producto_id
GROUP BY p.producto_id
HAVING total_comprado > (SELECT AVG(cantidad) FROM detalles_pedidos);