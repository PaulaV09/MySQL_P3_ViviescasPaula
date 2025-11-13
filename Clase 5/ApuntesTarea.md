# 📘 Apuntes de Clase – Claves, Restricciones y Modelo Entidad-Relación (E-R)

------

## 🔑 1. ¿Qué son las Claves en MySQL?

### 🧠 Concepto

Las **claves** son elementos esenciales en bases de datos relacionales (como MySQL) que garantizan:

- **Integridad de datos**
- **Rendimiento en consultas**
- **Relaciones entre tablas**
- **Prevención de duplicados**

> 🧩 Su origen se remonta al modelo relacional de **E.F. Codd (1970)**, quien propuso la organización de los datos mediante relaciones entre tablas.

------

### 🧱 1.1 Tipos de Claves

#### 🔸 Clave Primaria (`PRIMARY KEY`)

- Identifica de forma **única** cada registro en una tabla.
- No permite valores **nulos** ni duplicados.
- Cada tabla solo puede tener **una clave primaria**.

📘 **Ejemplo:**

```
CREATE TABLE estudiantes (
    id INT PRIMARY KEY,
    nombre VARCHAR(50)
);
```

------

#### 🔸 Clave Foránea (`FOREIGN KEY`)

- Crea una **relación entre tablas**.
- Asegura que los valores de una columna coincidan con los de la **clave primaria** en otra tabla.

📘 **Ejemplo:**

```
CREATE TABLE cursos (
    id INT PRIMARY KEY,
    nombre VARCHAR(50)
);

CREATE TABLE inscripciones (
    estudiante_id INT,
    curso_id INT,
    FOREIGN KEY (estudiante_id) REFERENCES estudiantes(id),
    FOREIGN KEY (curso_id) REFERENCES cursos(id)
);
```

------

## ⚙️ 2. Restricciones en MySQL

Las **restricciones** limitan los valores que pueden almacenarse en una columna, garantizando la **coherencia** y **validez** de los datos.

------

### 🔸 2.1 `UNIQUE`

Asegura que los valores de una columna o conjunto de columnas sean **únicos**.

```
CREATE TABLE empleados (
    id INT PRIMARY KEY,
    codigo_empleado INT UNIQUE,
    nombre VARCHAR(50)
);
```

------

### 🔸 2.2 `DEFAULT`

Define un **valor por defecto** si no se proporciona uno en la inserción.

```
CREATE TABLE pedidos (
    id INT PRIMARY KEY,
    fecha_pedido DATE DEFAULT (CURRENT_DATE),
    total DECIMAL(10, 2) DEFAULT 0.00
);
```

------

### 🔸 2.3 `CHECK`

Valida una **condición lógica** antes de insertar un valor.

```
CREATE TABLE productos (
    id INT PRIMARY KEY,
    nombre VARCHAR(50),
    cantidad INT CHECK (cantidad >= 0)
);
```

------

### 🔸 2.4 `NOT NULL`

Indica que una columna **no puede contener valores nulos**.

```
CREATE TABLE clientes (
    id INT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL
);
```

------

### 🔸 2.5 `AUTO_INCREMENT`

Genera valores **automáticos y únicos** para la clave primaria.

```
CREATE TABLE empleados (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50)
);
```

------

## 🧮 3. Modelo Entidad–Relación (E-R)

### 🧠 Concepto

El **modelo E-R** representa la estructura **conceptual** de una base de datos mediante:

- **Entidades** → Objetos del mundo real (por ejemplo, “Libro”).
- **Atributos** → Características de las entidades (por ejemplo, “Título”).
- **Relaciones** → Asociaciones entre entidades (por ejemplo, “Un libro pertenece a un autor”).

El **modelo relacional** traduce ese diseño conceptual a tablas, columnas y relaciones con claves.

------

### 🔍 Diferencias entre Modelo E-R y Modelo Relacional

| Aspecto                   | Modelo E-R                        | Modelo Relacional           |
| ------------------------- | --------------------------------- | --------------------------- |
| **Enfoque**               | Conceptual / lógico               | Físico / implementación     |
| **Representación**        | Entidades, atributos y relaciones | Tablas, filas y columnas    |
| **Nivel de abstracción**  | Más alto                          | Más concreto                |
| **Herramientas visuales** | Diagramas E-R                     | Tablas SQL                  |
| **Relaciones**            | Conceptuales                      | Claves primarias y foráneas |
| **Lenguaje**              | No usa SQL                        | Usa SQL                     |
| **Objetivo**              | Diseño conceptual                 | Implementación física       |

------

## 🧩 3.1 Componentes del Modelo E-R

| Componente            | Descripción                      | Ejemplo                                                      |
| --------------------- | -------------------------------- | ------------------------------------------------------------ |
| **Entidad**           | Objeto del mundo real            | Libro, Autor, Cliente                                        |
| **Atributo**          | Propiedades de la entidad        | Título, Año, Nombre                                          |
| **Relación**          | Asociación entre entidades       | Cliente — Préstamo — Libro                                   |
| **Cardinalidad**      | Cuántas instancias se relacionan | 1:1, 1:N, N:M                                                |
| **Tipos de relación** | Según la cardinalidad            | Paciente–Historia (1:1), País–Ciudad (1:N), Producto–Proveedor (N:M) |

------

## 🧭 3.2 Notación Gráfica en Diagramas E-R

| Elemento         | Representación                  |
| ---------------- | ------------------------------- |
| **Entidad**      | 🔲 Rectángulo                    |
| **Atributo**     | ⭕ Óvalo                         |
| **Relación**     | 🔺 Rombos o líneas               |
| **Cardinalidad** | 1:1, 1:N, N:M cerca de la línea |

------

## 🧱 3.3 Pasos para Crear un Diagrama E-R

1. **Identificar Entidades**
    Ejemplo: Libros, Autores, Usuarios, Préstamos.
2. **Definir Atributos**
    Ejemplo: Título, ISBN, Año, Editorial.
3. **Establecer Relaciones**
    Ejemplo: Un Usuario puede hacer varios Préstamos (1:N).
4. **Definir Claves Primarias**
    Identifican de forma única cada entidad.
5. **Agregar Restricciones y Atributos Derivados**
    (como disponibilidad o cantidad).
6. **Dibujar el Diagrama**
    Usa herramientas como *draw.io*, *Lucidchart*, *Visio*.
7. **Revisar y Ajustar**
    Verifica que todas las relaciones estén correctas.
8. **Documentar**
    Explica entidades, atributos y relaciones.
9. **Implementar en la Base de Datos**
    Traducir entidades y relaciones a tablas SQL.
10. **Mantener y Actualizar**
     Ajustar el modelo conforme evolucione el sistema.

------

## 📚 3.4 Caso de Estudio: Biblioteca “BiblioNet”

### 🏷️ Entidades Principales

- **Libros**
- **Autores**
- **Usuarios**
- **Préstamos**
- **Reseñas**
- **Categorías**

### 🔹 Atributos (ejemplos)

- Libros: ISBN, título, año, editorial
- Autores: nombre, nacionalidad, fecha de nacimiento
- Usuarios: nombre, correo, contraseña
- Préstamos: fecha_inicio, fecha_devolución, estado
- Reseñas: texto, calificación, fecha

### 🔹 Relaciones

- Un libro tiene varios autores (N:M)
- Un usuario puede dejar varias reseñas (1:N)
- Un usuario puede hacer varios préstamos (1:N)
- Un libro puede pertenecer a varias categorías (N:M)

------

### 💾 Modelo Relacional Derivado

| Tabla      | Clave Primaria | Claves Foráneas  |
| ---------- | -------------- | ---------------- |
| Libros     | ISBN           | ID_autor         |
| Autores    | ID_autor       | —                |
| Usuarios   | ID_usuario     | —                |
| Préstamos  | ID_prestamo    | ID_usuario, ISBN |
| Reseñas    | ID_reseña      | ID_usuario, ISBN |
| Categorías | ID_categoria   | —                |