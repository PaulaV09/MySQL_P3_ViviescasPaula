# 🧩 Diseño de Base de Datos

El **diseño de base de datos** es el proceso de definir la estructura que permitirá **almacenar, organizar y relacionar datos de forma eficiente**. Incluye la creación de **tablas, relaciones, índices y vistas**, asegurando integridad, rendimiento y facilidad de consulta.

------

## 🔗 JOINS: Combinación de Tablas

Los **JOINs** permiten unir datos de distintas tablas relacionadas, evitando redundancia y manteniendo integridad referencial.

### Tipos de JOIN en SQL

| Tipo de JOIN        | Descripción                                                  | Ejemplo                                                      |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **INNER JOIN**      | Devuelve filas que tienen coincidencia en ambas tablas.      | `SELECT p.nombre, c.nombre FROM pais p INNER JOIN ciudad c ON p.id = c.id_pais;` |
| **LEFT JOIN**       | Devuelve todas las filas de la tabla izquierda, y las coincidentes de la derecha (NULL si no hay). | `SELECT p.nombre, c.nombre FROM pais p LEFT JOIN ciudad c ON p.id = c.id_pais;` |
| **RIGHT JOIN**      | Devuelve todas las filas de la tabla derecha, y las coincidentes de la izquierda. | `SELECT p.nombre, c.nombre FROM ciudad c RIGHT JOIN pais p ON c.id_pais = p.id;` |
| **FULL OUTER JOIN** | Devuelve todas las filas de ambas tablas (aunque no haya coincidencia). *MySQL no lo soporta directamente; se simula con UNION.* | `LEFT JOIN ... UNION RIGHT JOIN ...`                         |

------

## 🧮 Consultas Anidadas (Subconsultas)

Las **subconsultas** son consultas dentro de otras consultas. Permiten filtrar, calcular o comparar resultados entre tablas.

### Estructura básica

```
SELECT columnas
FROM tabla1
WHERE columna OPERATOR (SELECT columnas FROM tabla2);
```

### Tipos de Subconsultas

| Tipo               | Uso principal                                | Ejemplo                                                      |
| ------------------ | -------------------------------------------- | ------------------------------------------------------------ |
| **En WHERE**       | Filtrar resultados según un valor calculado. | `SELECT Nombre FROM Pais WHERE Poblacion > (SELECT AVG(Poblacion) FROM Pais);` |
| **En FROM**        | Crear una tabla temporal.                    | `SELECT AVG(Poblacion) FROM (SELECT Poblacion FROM Ciudad WHERE id_pais=1) AS sub;` |
| **En SELECT**      | Agregar datos adicionales a las columnas.    | `SELECT Nombre, (SELECT COUNT(*) FROM Ciudad WHERE Ciudad.id_pais=Pais.id) AS NumCiudades FROM Pais;` |
| **Correlacionada** | Depende de la consulta externa.              | `SELECT Nombre FROM Ciudad c1 WHERE Poblacion > (SELECT AVG(Poblacion) FROM Ciudad c2 WHERE c2.id_pais=c1.id_pais);` |
| **En HAVING**      | Filtrar grupos agregados.                    | `SELECT id_pais, COUNT(*) FROM Ciudad GROUP BY id_pais HAVING COUNT(*) > (SELECT COUNT(*)/COUNT(DISTINCT id_pais) FROM Ciudad);` |

📘 **Ejemplo práctico:**
 Listar ciudades del país con menor población:

```
SELECT C.nombre
FROM ciudad C
INNER JOIN pais P ON P.id = C.id_pais
WHERE P.poblacion = (SELECT MIN(poblacion) FROM pais);
```

------

## ⚙️ Índices en MySQL

Los **índices** son estructuras que aceleran la búsqueda de datos, como un índice en un libro.
 Evitan “full table scans” y mejoran el rendimiento de consultas, especialmente en columnas que se usan para búsquedas o uniones.

### Sintaxis general

```
CREATE INDEX nombre_indice ON tabla (columna1, columna2, ...);
```

### Tipos de Índices

| Tipo               | Propósito                                     | Ejemplo                                                      |
| ------------------ | --------------------------------------------- | ------------------------------------------------------------ |
| **Simple**         | Buscar rápido en una sola columna.            | `CREATE INDEX idx_user_id ON users (user_id);`               |
| **Compuesto**      | Filtrar por múltiples columnas.               | `CREATE INDEX idx_order_date_status ON orders (order_date, status);` |
| **Único**          | Evita duplicados en una columna.              | `CREATE UNIQUE INDEX idx_email ON users (email);`            |
| **Texto Completo** | Búsquedas de palabras clave en textos largos. | `CREATE FULLTEXT INDEX idx_article_content ON articles (content);` |
| **Espacial**       | Datos geoespaciales (mapas, coordenadas).     | `CREATE SPATIAL INDEX idx_location ON places (geo_point);`   |

🔎 **Cuándo usar índices:**

- Columnas frecuentes en `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`.
- Columnas con valores únicos o alta cardinalidad.

⚠️ **Evitar índices en:**

- Tablas con muchas escrituras (INSERT, UPDATE, DELETE).
- Columnas con muchos valores repetidos.

------

## 👁️ Vistas (Views)

Una **vista** es una *tabla virtual* que guarda una consulta SQL. No almacena datos, solo los muestra según la definición de la vista.

### Ventajas

- ✅ Simplifican consultas complejas.
- 🔒 Mejoran la seguridad (solo muestran lo necesario).
- 🔁 Facilitan el mantenimiento.
- ⚖️ Aseguran consistencia de datos entre usuarios.

### Ejemplos de uso

| Caso                      | Descripción                 | Ejemplo                                                      |
| ------------------------- | --------------------------- | ------------------------------------------------------------ |
| **Consolidar datos**      | Combina varias tablas.      | `CREATE VIEW vista_detalles_usuario AS SELECT u.id, u.nombre, d.direccion FROM usuarios u JOIN detalles_usuario d ON u.id=d.usuario_id;` |
| **Filtrar por seguridad** | Oculta datos sensibles.     | `CREATE VIEW vista_usuarios_publicos AS SELECT id, nombre, email FROM usuarios WHERE activo=1;` |
| **Reportes**              | Simplifica informes.        | `CREATE VIEW vista_reporte_ventas AS SELECT p.nombre, SUM(v.cantidad) AS total_ventas FROM productos p JOIN ventas v ON p.id=v.producto_id GROUP BY p.nombre;` |
| **Normalización**         | Presenta datos modificados. | `CREATE VIEW vista_productos_normalizados AS SELECT id, UPPER(nombre), ROUND(precio,2) FROM productos;` |

### Gestión de Vistas

```
-- Crear una vista
CREATE VIEW vistaProductosBarato AS
SELECT C.Nombre, P.Nombre, I.idioma
FROM pais P
INNER JOIN ciudad C ON P.id = C.id_pais
INNER JOIN idioma_pais IP ON P.id = IP.id_pais
INNER JOIN idioma I ON IP.id_idioma = I.id
WHERE IP.es_oficial = TRUE;

-- Consultar la vista
SELECT * FROM vistaProductosBarato;

-- Eliminar la vista
DROP VIEW vistaProductosBarato;
```

------

## 🧠 Ejemplo de Consulta Compleja con JOINs y Vistas

### Objetivo

Generar un reporte que incluya:

- Países y sus ciudades.
- Idiomas hablados (indicando si son oficiales).
- Población total de cada país.

### Consulta Completa

```
SELECT
  p.nombre AS NombrePais,
  c.nombre AS NombreCiudad,
  i.idioma AS Idioma,
  CASE WHEN ip.es_oficial = 1 THEN 'Oficial' ELSE 'No Oficial' END AS EstadoIdioma,
  p.poblacion AS Poblacion
FROM pais p
LEFT JOIN ciudad c ON p.id = c.id_pais
LEFT JOIN idioma_pais ip ON p.id = ip.id_pais
LEFT JOIN idioma i ON ip.id_idioma = i.id;
```

### Simplificación con Vistas

Usar vistas predefinidas para reducir complejidad:

```
SELECT
  vp.NombrePais,
  vc.NombreCiudad,
  vi.Idioma,
  vi.EstadoIdioma,
  vp.Poblacion
FROM vista_paises vp
LEFT JOIN vista_ciudades_paises vc ON vp.id = vc.id_pais
LEFT JOIN vista_idiomas_oficiales vi ON vp.id = vi.id_pais;
```

📌 **Ventajas de la simplificación:**

- Menos código y mayor claridad.
- Las vistas encapsulan la lógica compleja.
- Más fácil mantenimiento y legibilidad.