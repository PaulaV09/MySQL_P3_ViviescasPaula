# 📘 Apuntes de Clase – MySQL: Creación de Tablas y Relaciones

## 🧩 1. Creación de Tablas a Partir de Consultas

### 🔹 Concepto

Permite generar una nueva tabla basada en los resultados de una consulta `SELECT`.
 Sintaxis básica:

```sql
CREATE TABLE nueva_tabla AS
SELECT columna1, columna2, ...
FROM tabla_origen
WHERE condicion;
```

### 🔹 Usos Comunes

- **Análisis de datos:** crear subconjuntos específicos.
- **Backup temporal:** copiar datos para pruebas o mantenimiento.
- **Transformación de datos:** guardar resultados intermedios.

### 🔹 Consideraciones

- No hereda **llaves primarias**, **índices** ni **restricciones** automáticamente.
- Es **eficiente** para generar nuevas vistas de datos.
- La tabla creada **no se actualiza** si cambian los datos originales.

------

### 🧠 Ejemplos

```sql
CREATE TABLE empleados_departamento_x AS
SELECT nombre, salario
FROM empleados
WHERE departamento = 'Departamento X' AND salario > 50000;
CREATE TABLE tempPais AS
SELECT nombre, poblacion
FROM pais
WHERE poblacion <= 100000000;
```

------

## 🔍 1.2 Revisar la Estructura de una Tabla

### Comandos Principales

| Comando                                                      | Descripción                                    |
| ------------------------------------------------------------ | ---------------------------------------------- |
| `DESCRIBE nombre_tabla;`                                     | Muestra columnas, tipos de datos, claves, etc. |
| `SHOW COLUMNS FROM nombre_tabla;`                            | Similar a `DESCRIBE`.                          |
| `SHOW CREATE TABLE nombre_tabla;`                            | Muestra el SQL con el que fue creada la tabla. |
| `SHOW TABLE STATUS LIKE 'nombre_tabla';`                     | Información general de la tabla.               |
| `SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'nombre_tabla';` | Consulta avanzada sobre estructura.            |

📘 Ejemplo:

```sql
DESCRIBE tempPais;
```

------

## 🧱 2. Relaciones entre Tablas y Llaves Foráneas

### 🔹 Concepto

Permiten **vincular datos** entre tablas para mantener **integridad referencial**.
 Tipos principales:

- **Uno a uno (1:1)**
- **Uno a muchos (1:N)**
- **Muchos a muchos (N:M)**

------

### 🔸 Relación 1 a 1

Cada registro de una tabla se asocia con **uno y solo uno** de otra.
 Se usa por organización o seguridad.

------

### 🔸 Relación 1 a Muchos

Un registro en una tabla puede estar relacionado con **varios** en otra.

```sql
CREATE TABLE Libro (
    ID INT PRIMARY KEY,
    Titulo VARCHAR(100),
    Autor VARCHAR(100)
);

CREATE TABLE Prestamo (
    ID INT PRIMARY KEY,
    ID_Libro INT,
    FechaPrestamo DATE,
    FechaDevolucion DATE,
    FOREIGN KEY (ID_Libro) REFERENCES Libro(ID)
);
```

👉 Un libro puede tener muchos préstamos.

------

### 🔸 Relación Muchos a Muchos

Se usa una **tabla intermedia** (de unión) que contiene claves foráneas de ambas tablas.

```sql
CREATE TABLE Estudiante (
    ID INT PRIMARY KEY,
    Nombre VARCHAR(100)
);

CREATE TABLE Curso (
    ID INT PRIMARY KEY,
    Nombre VARCHAR(100)
);

CREATE TABLE Inscripcion (
    ID_Estudiante INT,
    ID_Curso INT,
    FechaInscripcion DATE,
    PRIMARY KEY (ID_Estudiante, ID_Curso),
    FOREIGN KEY (ID_Estudiante) REFERENCES Estudiante(ID),
    FOREIGN KEY (ID_Curso) REFERENCES Curso(ID)
);
```

------

## 🔗 2.3 Crear Llaves Foráneas

### 🔹 Durante la Creación

```sql
CREATE TABLE ciudad (
    id INT PRIMARY KEY,
    nombre VARCHAR(20),
    id_pais INT,
    FOREIGN KEY (id_pais) REFERENCES pais (id)
);
```

### 🔹 Después de Crear la Tabla

```sql
ALTER TABLE NombreTabla
ADD FOREIGN KEY (ColumnaForanea) REFERENCES OtraTabla(ColumnaReferenciada);
```

### 🔹 Ventajas

- Mantiene **integridad referencial**
- Facilita **JOINS**
- Permite **borrado o actualización en cascada**

------

## 🏷️ 2.4 Trabajo con Campos y Alias

### 🔹 Campos

Son las **columnas** de una tabla.
 Ejemplos:

```sql
SELECT nombre, edad FROM usuarios;
SELECT precio * cantidad AS total FROM ventas;
```

### 🔹 Alias

Permiten renombrar tablas o columnas para simplificar consultas.

#### 🔸 Alias para Columnas

```sql
SELECT fecha_de_nacimiento AS fecha_nac FROM empleados;
```

#### 🔸 Alias para Tablas

```sql
SELECT e.nombre, d.nombre_departamento
FROM empleados e
JOIN departamentos d ON e.id_departamento = d.id;
```

#### 🔸 Alias en Subconsultas

```sql
SELECT u.nombre
FROM (SELECT * FROM usuarios WHERE edad > 30) AS u;
```

#### 🔸 Alias en Funciones de Agregación

```sql
SELECT AVG(salario) AS salario_promedio FROM empleados;
```

------

## ⚙️ 2.5 Funciones y Comandos en Campos (MySQL)

### 📋 Funciones Comunes

| Función               | Descripción                             | Ejemplo                          |
| --------------------- | --------------------------------------- | -------------------------------- |
| `CONCAT()`            | Une cadenas de texto                    | `CONCAT(nombre, ' ', apellido)`  |
| `UPPER()` / `LOWER()` | Convierte texto a mayúsculas/minúsculas | `UPPER(nombre)`                  |
| `LENGTH()`            | Longitud de una cadena                  | `LENGTH(nombre)`                 |
| `SUBSTRING()`         | Extrae parte de una cadena              | `SUBSTRING(nombre,1,3)`          |
| `TRIM()`              | Elimina espacios                        | `TRIM(nombre)`                   |
| `ROUND()`             | Redondea números                        | `ROUND(salario,2)`               |
| `DATE_FORMAT()`       | Da formato a fechas                     | `DATE_FORMAT(fecha, '%d-%m-%Y')` |
| `NOW()`               | Devuelve fecha y hora actual            | `NOW()`                          |
| `IFNULL()`            | Valor alternativo si es nulo            | `IFNULL(departamento, 'N/A')`    |

### 🔹 Ejemplo General

```sql
SELECT
    CONCAT(nombre, ' ', apellido) AS nombre_completo,
    ROUND(salario, 2) AS salario_redondeado,
    DATE_FORMAT(fecha_nacimiento, '%d-%m-%Y') AS fecha_formateada,
    IFNULL(departamento, 'No asignado') AS depto
FROM empleados;
```