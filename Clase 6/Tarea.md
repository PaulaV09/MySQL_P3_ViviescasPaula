# 🧩 Consultas SQL – Niveles 1, 2 y 3

------

## 🔹 NIVEL 1

### 1️⃣ ID y nombre de los 10 primeros parques por orden alfabético

```
SELECT id, nombre 
FROM parque_natural 
ORDER BY nombre ASC 
LIMIT 10;
```

### 2️⃣ ID, región y departamentos de la tabla departamento

```
SELECT id, region, departamentos 
FROM departamento;
```

### 3️⃣ Vehículos cuyo tipo sea exactamente '4x4'

```
SELECT * 
FROM vehiculo 
WHERE tipo = '4x4';
```

### 4️⃣ Alojamientos con capacidad ≥ 4, ordenados por precio por noche descendente

```
SELECT * 
FROM alojamiento 
WHERE capacidad >= 4 
ORDER BY precio_x_noche DESC;
```

### 5️⃣ Parques declarados a partir del 2015-01-01

```
SELECT id, nombre, dia_declarado 
FROM parque_natural 
WHERE dia_declarado >= '2015-01-01';
```

### 6️⃣ Los 5 visitantes con cédula más alta

```
SELECT id, nombre, cedula 
FROM visitante 
ORDER BY cedula DESC 
LIMIT 5;
```

### 7️⃣ Áreas con extensión entre 15 y 25

```
SELECT id, nombre, extension 
FROM area2 
WHERE extension BETWEEN 15 AND 25;
```

### 8️⃣ Personal con sueldo entre 1’800.000 y 2’200.000

```
SELECT id, nombre, sueldo 
FROM personal_parque 
WHERE sueldo BETWEEN 1800000 AND 2200000;
```

### 9️⃣ Visitantes cuya profesión sea 'Ingeniero' o 'Docente'

```
SELECT * 
FROM visitante 
WHERE profesion IN ('Ingeniero', 'Docente');
```

### 🔟 Alojamiento con el mayor precio por noche

```
SELECT id, categoria, precio_x_noche 
FROM alojamiento 
ORDER BY precio_x_noche DESC 
LIMIT 1;
```

------

## 🔹 NIVEL 2

### 1️⃣ ID y nombre de los 10 primeros parques por orden alfabético

```
SELECT id, nombre 
FROM parque_natural 
ORDER BY nombre ASC 
LIMIT 10;
```

### 2️⃣ ID, región y departamentos de la tabla departamento

```
SELECT id, region, departamentos 
FROM departamento;
```

### 3️⃣ Vehículos cuyo tipo sea exactamente '4x4'

```
SELECT * 
FROM vehiculo 
WHERE tipo = '4x4';
```

### 4️⃣ Alojamientos con capacidad ≥ 4, ordenados por precio por noche descendente

```
SELECT * 
FROM alojamiento 
WHERE capacidad >= 4 
ORDER BY precio_x_noche DESC;
```

### 5️⃣ Parques declarados a partir del 2015-01-01

```
SELECT id, nombre, dia_declarado 
FROM parque_natural 
WHERE dia_declarado >= '2015-01-01';
```

### 6️⃣ Los 5 visitantes con cédula más alta

```
SELECT id, nombre, cedula 
FROM visitante 
ORDER BY cedula DESC 
LIMIT 5;
```

### 7️⃣ Áreas con extensión entre 15 y 25

```
SELECT id, nombre, extension 
FROM area2 
WHERE extension BETWEEN 15 AND 25;
```

### 8️⃣ Personal con sueldo entre 1’800.000 y 2’200.000

```
SELECT id, nombre, sueldo 
FROM personal_parque 
WHERE sueldo BETWEEN 1800000 AND 2200000;
```

### 9️⃣ Visitantes cuya profesión sea 'Ingeniero' o 'Docente'

```
SELECT * 
FROM visitante 
WHERE profesion IN ('Ingeniero', 'Docente');
```

### 🔟 Alojamiento con el mayor precio por noche

```
SELECT id, categoria, precio_x_noche 
FROM alojamiento 
ORDER BY precio_x_noche DESC 
LIMIT 1;
```

------

## 🔹 NIVEL 3

### 1️⃣ Visitantes con categoría de alojamiento y nombre del parque

```
SELECT v.nombre AS visitante, 
       a.categoria AS categoria_alojamiento, 
       p.nombre AS parque
FROM visitante v
INNER JOIN alojamiento a ON v.alojamiento_id = a.id
INNER JOIN parque_natural p ON a.parque_natural_id = p.id;
```

### 2️⃣ Entradas con el nombre del parque

```
SELECT e.nombre AS entrada, e.ubicacion, p.nombre AS parque
FROM entrada e
INNER JOIN parque_natural p ON e.id_parque = p.id;
```

### 3️⃣ Personal de vigilancia con empleado, área y vehículo

```
SELECT pp.nombre AS empleado, 
       a.nombre AS area, 
       v.marca AS vehiculo
FROM personal_de_vigilancia pv
INNER JOIN personal_parque pp ON pv.personal_parque_id = pp.id
INNER JOIN area2 a ON pv.area2_id = a.id
INNER JOIN vehiculo v ON pv.vehiculo_id = v.id;
```

### 4️⃣ Especies con su investigador (funciones) y parque

```
SELECT e.denominacion_cientifica AS especie, 
       pi.funciones AS investigador, 
       p.nombre AS parque
FROM especie e
INNER JOIN personal_investigador pi ON e.personal_investigador_id = pi.id
INNER JOIN area2 a ON e.area2_id = a.id
INNER JOIN parque_natural p ON a.parque_id = p.id;
```

### 5️⃣ Entidades y sus departamentos (LEFT JOIN)

```
SELECT en.nombre AS entidad, 
       d.departamentos AS departamento
FROM entidad en
LEFT JOIN departamento d ON en.departamento_id = d.id;
```

### 6️⃣ Personal investigador con título y fecha

```
SELECT pi.id AS id_investigador, 
       pi.funciones, 
       t.titulo, 
       t.fecha
FROM personal_investigador pi
INNER JOIN titulacion t ON pi.titulacion_id = t.id;
```