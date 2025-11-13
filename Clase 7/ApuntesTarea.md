# 📘 Apuntes: Procedimientos Almacenados en Bases de Datos

## 🧩 1. Introducción

Los **procedimientos almacenados** son conjuntos de instrucciones SQL que se guardan en el **servidor de la base de datos** y se pueden ejecutar usando un nombre específico.

📖 **Analogía:** son como una receta guardada; en lugar de repetir todos los pasos, simplemente llamas a la receta por su nombre.

### ✅ Ventajas iniciales

- **Eficiencia:** se ejecutan más rápido al estar precompilados.
- **Optimización:** el servidor no recompila las instrucciones cada vez.
- **Seguridad:** evita la ejecución directa de SQL y posibles inyecciones.

------

## ⚙️ 1.1 Definición Detallada

Un **procedimiento almacenado** es una función definida por el usuario que:

- Contiene instrucciones SQL y lógica de programación.
- Puede ser invocado por aplicaciones, disparadores u otros procedimientos.

### 🔑 Características

- **Precompilados:** mejoran el rendimiento.
- **Parametrizables:** admiten parámetros `IN`, `OUT`, `INOUT`.
- **Encapsulan** lógica de negocio.
- **Transaccionales:** admiten `COMMIT` y `ROLLBACK`.
- **Seguros:** controlan el acceso y previenen inyección SQL.

### 🧠 Aplicaciones

- **Optimización:** ejecutan tareas complejas más rápido.
- **Automatización:** reaccionan ante eventos (INSERT, UPDATE…).
- **Integridad:** garantizan consistencia en los datos.
- **Mantenimiento:** centralizan la lógica del negocio.
- **Reportes:** pueden generar informes directamente.

------

## 💪 1.1.3 Ventajas Principales

| Ventaja            | Descripción                                  | Ejemplo                                                      |
| ------------------ | -------------------------------------------- | ------------------------------------------------------------ |
| **Modularidad**    | Se divide la lógica en partes reutilizables. | Procedimiento para calcular total, verificar inventario, procesar pago. |
| **Rendimiento**    | Menos tráfico de red y ejecución más rápida. | Consultas ejecutadas directamente en el servidor.            |
| **Seguridad**      | Protege contra inyecciones SQL.              | Procedimiento de transferencia bancaria.                     |
| **Mantenibilidad** | Los cambios se hacen una sola vez.           | Cambio en cálculo de impuestos dentro de un ERP.             |

------

## 🧱 1.2 Estructura Básica

```
DELIMITER //
CREATE PROCEDURE nombre_procedimiento ([parámetros])
BEGIN
    -- Instrucciones SQL
END //
DELIMITER ;
```

### 🧩 Tipos de Parámetros

- **IN:** recibe un valor.
- **OUT:** devuelve un valor.
- **INOUT:** recibe y modifica un valor.

**Ejemplo:**

```
CREATE PROCEDURE CalcularTotal(IN precio DECIMAL(10,2), IN cantidad INT, OUT total DECIMAL(10,2))
BEGIN
    SET total = precio * cantidad;
END;
```

------

## 🧰 1.3 Configuración en MySQL

### 🔹 Crear Base y Tablas

```
CREATE DATABASE base_ejemplo;
USE base_ejemplo;

CREATE TABLE productos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(20),
  estado VARCHAR(20) DEFAULT 'disponible',
  precio FLOAT DEFAULT 0.0
);
```

### 🔹 Procedimiento con Parámetro IN

```
CREATE PROCEDURE obtenerProductosPorEstado(IN nombre_estado VARCHAR(255))
BEGIN
  SELECT * FROM productos WHERE estado = nombre_estado;
END;
```

### 🔹 Procedimiento con Parámetro OUT

```
CREATE PROCEDURE contarProductosPorEstado(IN nombre_estado VARCHAR(25), OUT numero INT)
BEGIN
  SELECT COUNT(id) INTO numero FROM productos WHERE estado = nombre_estado;
END;
```

### 🔹 Procedimiento con INOUT

```
CREATE PROCEDURE venderProducto(INOUT beneficio INT, IN id_producto INT)
BEGIN
  DECLARE precio_producto FLOAT;
  SELECT precio INTO precio_producto FROM productos WHERE id = id_producto;
  SET beneficio = beneficio + precio_producto;
END;
```

### 🔹 Eliminar un Procedimiento

```
DROP PROCEDURE nombre_procedimiento;
```

------

## 🌎 1.4 Ejemplos Prácticos

### 🏙️ Listar Ciudades de un País

```
CREATE PROCEDURE ListarCiudadesDePais(IN nombrePais VARCHAR(100))
BEGIN
  SELECT ciudad.nombre
  FROM ciudad
  INNER JOIN pais ON ciudad.id_pais = pais.id
  WHERE pais.nombre = nombrePais;
END;
```

### 📊 Contar Ciudades de un País

```
CREATE PROCEDURE ContarCiudadesDePais(IN nombrePais VARCHAR(100), OUT totalCiudades INT)
BEGIN
  SELECT COUNT(*) INTO totalCiudades
  FROM ciudad
  INNER JOIN pais ON ciudad.id_pais = pais.id
  WHERE pais.nombre = nombrePais;
END;
```

------

## 🔁 1.5 Estructuras de Control

### 🧮 IF - THEN - ELSE

```
IF condicion THEN
   -- acciones
ELSE
   -- acciones
END IF;
```

**Ejemplo:** Clasificar usuarios como “Mayor” o “Menor”.

```
CREATE PROCEDURE ClasificarUsuario(IN userID INT)
BEGIN
  DECLARE edad INT;
  SELECT edad INTO edad FROM usuarios WHERE id = userID;
  IF edad >= 18 THEN
    UPDATE usuarios SET categoria = 'Mayor' WHERE id = userID;
  ELSE
    UPDATE usuarios SET categoria = 'Menor' WHERE id = userID;
  END IF;
END;
```

------

### 🔁 LOOP

```
LOOP
   -- acciones
   IF condicion_salida THEN
      LEAVE loop_label;
   END IF;
END LOOP loop_label;
```

------

### 🔂 REPEAT

```
REPEAT
   -- acciones
UNTIL condicion
END REPEAT;
```

------

### 🔄 WHILE

```
WHILE condicion DO
   -- acciones
END WHILE;
```

------

### 🎯 CASE

```
CASE expresion
   WHEN valor1 THEN -- acción
   WHEN valor2 THEN -- acción
   ELSE -- acción
END CASE;
```

**Ejemplo: Categorizar empleados por salario**

```
CREATE PROCEDURE AsignarCategoriaSalario()
BEGIN
  CASE
    WHEN salario <= 3000 THEN UPDATE empleados SET categoria = 'Entrada';
    WHEN salario <= 7000 THEN UPDATE empleados SET categoria = 'Media';
    ELSE UPDATE empleados SET categoria = 'Alta';
  END CASE;
END;
```

------

## 🧪 1.6 Ejemplos de Uso

### IF-THEN-ELSE

```
CREATE PROCEDURE VerificarSaldo(IN saldo DECIMAL(10,2), OUT mensaje VARCHAR(100))
BEGIN
  IF saldo >= 0 THEN
      SET mensaje = 'Saldo positivo';
  ELSE
      SET mensaje = 'Saldo negativo';
  END IF;
END;
```

### CASE

```
CREATE PROCEDURE AsignarDescuento(IN nivelUsuario VARCHAR(50), OUT descuento INT)
BEGIN
  CASE nivelUsuario
    WHEN 'Principiante' THEN SET descuento = 5;
    WHEN 'Intermedio' THEN SET descuento = 10;
    WHEN 'Experto' THEN SET descuento = 15;
  END CASE;
END;
```

------

## 🧩 Conclusión

Los **procedimientos almacenados**:

- Mejoran el **rendimiento**.
- Aumentan la **seguridad**.
- Facilitan el **mantenimiento** y la **modularidad**.
- Son esenciales para sistemas donde múltiples usuarios o aplicaciones interactúan con la base de datos.