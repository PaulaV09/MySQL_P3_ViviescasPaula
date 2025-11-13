# 📘 Normalización en Bases de Datos

## 🧩 1. ¿Qué es Normalizar?

**Normalizar** es el proceso de organizar los datos en una base de datos relacional para **minimizar la redundancia** y **mejorar la integridad**.
 Busca que los resultados sean **consistentes, precisos y eficientes**.

### 🔹 Ventajas

- **Evita la redundancia** de datos.
- **Mejora la integridad** y coherencia de la información.
- **Optimiza las consultas.**
- **Facilita el mantenimiento** y actualizaciones.

------

## ⚙️ 1.1 Pasos de la Normalización

El proceso de normalización sigue **formas normales (NF)**:
 1NF → 2NF → 3NF → BCNF (Boyce-Codd).

Cada una aplica reglas más estrictas para reducir redundancia y dependencia entre datos.

------

## 🧱 1NF – Primera Forma Normal

**Objetivo:** Eliminar repeticiones y asegurar que cada columna contenga valores **atómicos**.

**Regla:**
 Cada campo debe tener un solo valor indivisible.

**Ejemplo:**

```
CREATE TABLE Estudiante (
  ID int,
  Nombre varchar(100),
  Telefono varchar(15)
);
```

**Problema común:** columnas con listas o valores múltiples (no atómicos).
 **Solución:** dividir los datos en filas separadas.

------

## 🧱 2NF – Segunda Forma Normal

**Objetivo:** Eliminar **dependencias parciales**.
 Aplica cuando una tabla tiene **clave primaria compuesta**.

**Regla:**
 Estar en 1NF y que cada atributo no clave dependa completamente de toda la clave primaria.

**Ejemplo:**

```
CREATE TABLE Curso (
  CursoID int,
  EstudianteID int,
  NombreCurso varchar(100)
);
```

**Caso práctico:**
 Dividir una tabla de cursos y estudiantes en varias tablas:

- `EstudianteCurso`
- `Curso`
- `Estudiante`

✅ Se eliminan dependencias funcionales parciales y se mejora la integridad.

------

## 🧱 3NF – Tercera Forma Normal

**Objetivo:** Eliminar **dependencias transitivas**.
 (Es decir, un atributo no clave no debe depender de otro atributo no clave).

**Regla:**
 Estar en 2NF y que no existan dependencias transitivas.

**Ejemplo:**

```
CREATE TABLE Profesor (
  ProfesorID int,
  Nombre varchar(100),
  DepartamentoID int,
  NombreDepartamento varchar(100)
);
```

**Solución:** separar el departamento:

```
CREATE TABLE Departamento (
  DepartamentoID int PRIMARY KEY,
  NombreDepartamento varchar(100)
);
```

------

## 🧱 BCNF – Forma Normal de Boyce-Codd (a veces llamada 4NF)

**Objetivo:** Eliminar **dependencias multivaluadas** o **entre claves candidatas**.
 Cada determinante debe ser una **clave candidata**.

**Regla:**
 Estar en 3NF y que todo determinante sea clave candidata.

**Ejemplo:**

```
CREATE TABLE Asignacion (
  ProfesorID int,
  CursoID int,
  PRIMARY KEY (ProfesorID, CursoID)
);
```

**Si un atributo depende solo de parte de la clave**, se crea una tabla aparte.

------

## 🧩 2. Casos Comunes y Soluciones

### 🔸 Dependencia Parcial (2NF)

**Problema:** un atributo depende solo de una parte de la clave compuesta.
 **Solución:** dividir la tabla.

```
CREATE TABLE CursoEstudiante (
  CursoID int,
  EstudianteID int,
  NombreCurso varchar(100),
  NombreEstudiante varchar(100),
  PRIMARY KEY (CursoID, EstudianteID)
);
-- Solución
CREATE TABLE Curso (
  CursoID int PRIMARY KEY,
  NombreCurso varchar(100)
);
CREATE TABLE Estudiante (
  EstudianteID int PRIMARY KEY,
  NombreEstudiante varchar(100)
);
```

------

### 🔸 Dependencia Transitiva (3NF)

**Problema:** un atributo no clave depende de otro atributo no clave.
 **Solución:** moverlo a otra tabla.

```
CREATE TABLE Profesor (
  ProfesorID int,
  Nombre varchar(100),
  DepartamentoID int,
  NombreDepartamento varchar(100)
);
-- Solución
CREATE TABLE Departamento (
  DepartamentoID int PRIMARY KEY,
  NombreDepartamento varchar(100)
);
```

------

### 🔸 Claves Candidatas Dependientes (BCNF)

**Problema:** dependencias entre claves candidatas.
 **Solución:** reestructurar para que cada determinante sea clave candidata.

```
CREATE TABLE Asignacion (
  ProfesorID int,
  CursoID int,
  Horario varchar(50),
  PRIMARY KEY (ProfesorID, CursoID)
);
-- Si Horario depende solo de CursoID
CREATE TABLE HorarioCurso (
  CursoID int PRIMARY KEY,
  Horario varchar(50)
);
```

------

## 🧠 3. Ejemplo Completo: Universidad

### Tabla original

```
CREATE TABLE Universidad (
  EstudianteID int,
  NombreEstudiante varchar(100),
  CursoID int,
  NombreCurso varchar(100),
  ProfesorID int,
  NombreProfesor varchar(100),
  Departamento varchar(100)
);
```

### Problemas

- Redundancia
- Dependencia parcial
- Dependencia transitiva

### Normalización paso a paso

**1NF:** asegurar valores atómicos.
 **2NF:** separar en tablas base:

```
CREATE TABLE Estudiantes (...);
CREATE TABLE Cursos (...);
CREATE TABLE Profesores (...);
```

**3NF:** eliminar dependencias transitivas (crear tabla `Departamentos`).
 **BCNF:** verificar que cada determinante sea clave candidata.

✅ Resultado final:
 `Estudiantes`, `Cursos`, `Profesores`, `Departamentos`, `Asignaciones`.

------

## 🏥 3.1 Caso Avanzado: Sistema Hospitalario

### Tabla original

```
CREATE TABLE RegistroHospital (
  PacienteID int,
  NombrePaciente varchar(100),
  FechaNacimiento date,
  MedicoID int,
  NombreMedico varchar(100),
  Especialidad varchar(100),
  FechaVisita datetime,
  DescripcionTratamiento varchar(255),
  Medicamento varchar(100),
  Dosis varchar(50)
);
```

### Problemas

- Redundancia
- Dependencias parciales y transitivas

### Solución paso a paso

**2NF:**

```
CREATE TABLE Pacientes (...);
CREATE TABLE Medicos (...);
CREATE TABLE Visitas (...);
```

**3NF:**

```
CREATE TABLE Tratamientos (...);
CREATE TABLE Prescripciones (...);
```

✅ Resultado:
 `Pacientes`, `Medicos`, `Visitas`, `Tratamientos`, `Prescripciones`.

**Beneficios:**
 Menos redundancia, más integridad y facilidad de mantenimiento.

------

## 🧾 Conclusión

- La **normalización** mejora la eficiencia, integridad y consistencia de las bases de datos.
- Las formas normales (1NF → BCNF) eliminan **redundancia, dependencias parciales y transitivas**.
- Una base de datos bien normalizada es más fácil de **mantener, consultar y escalar.**