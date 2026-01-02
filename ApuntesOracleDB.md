# Oracle DB: Guía de Referencia Rápida

## 1. Comandos útiles (Metadatos y Sistema)

Consultas para obtener información sobre la base de datos y el entorno.

**Listar todas las tablas (incluso las del sistema)**
```sql
SELECT table_name FROM all_tables;
```

**Listar solo las tablas del usuario actual**
```sql
SELECT table_name FROM user_tables;
```

**Describir la estructura de una tabla (columnas y tipos)**
```sql
DESC nombre_tabla;
```

**Obtener la fecha y hora actual del sistema**
```sql
SELECT sysdate FROM dual;
```

**Obtener el usuario actual**
```sql
SELECT user FROM dual;
```

---

## 2. Consultas Básicas (DML)

### SELECT: Recuperación de datos

**Seleccionar todos los registros**
```sql
SELECT * FROM EMPLEADOS;
```

**Proyección y selección (Columnas específicas con condición)**
```sql
SELECT nombre, salario 
FROM EMPLEADOS 
WHERE salario > 50000;
```

**Filtrar por conjunto de datos (IN)**
```sql
SELECT nombre, salario 
FROM EMPLEADOS 
WHERE departamento IN ('Ventas', 'Marketing');
```

**Filtrar por rangos (BETWEEN)**
```sql
SELECT nombre, salario 
FROM EMPLEADOS 
WHERE salario BETWEEN 40000 AND 80000;
```

**Filtrar por patrón de texto (LIKE)**
* `%`: Cualquier cadena de caracteres.
* `_`: Un solo carácter.
```sql
SELECT nombre, salario 
FROM EMPLEADOS 
WHERE nombre LIKE 'J%';
```

**Operadores lógicos (AND, OR)**
```sql
SELECT nombre, salario 
FROM EMPLEADOS 
WHERE salario > 50000 AND departamento = 'Ventas';
```

**Manejo de fechas (TO_CHAR y TO_DATE)**
```sql
SELECT nombre, TO_CHAR(fecha_contratacion, 'YYYY-MM-DD') 
FROM EMPLEADOS 
WHERE fecha_contratacion > TO_DATE('2020-01-01', 'YYYY-MM-DD');
```

**Eliminar duplicados en la salida**
```sql
SELECT DISTINCT departamento FROM EMPLEADOS;
```

### Funciones de Agregación

**Calcular la media (AVG)**
```sql
SELECT AVG(salario) FROM EMPLEADOS;
```

**Calcular la suma (SUM)**
```sql
SELECT SUM(salario) FROM EMPLEADOS;
```

**Obtener el máximo (MAX)**
```sql
SELECT MAX(salario) FROM EMPLEADOS;
```

**Obtener el mínimo (MIN)**
```sql
SELECT MIN(salario) FROM EMPLEADOS;
```

**Contar total de registros**
```sql
SELECT COUNT(*) FROM EMPLEADOS;
```

**Contar valores distintos**
```sql
SELECT COUNT(DISTINCT departamento) FROM EMPLEADOS;
```

---

### Modificación de Datos

**INSERT: Insertar un nuevo registro**
```sql
INSERT INTO EMPLEADOS (nombre, salario, departamento) 
VALUES ('Juan Perez', 60000, 'Ventas');
```

**UPDATE: Actualizar registros existentes**
```sql
UPDATE EMPLEADOS 
SET salario = salario * 1.1 
WHERE departamento = 'Ventas';
```

**DELETE: Eliminar registros**
```sql
DELETE FROM EMPLEADOS 
WHERE nombre = 'Juan Perez';
```

---

## 3. Subconsultas

**Subconsulta escalar en WHERE**
Selecciona empleados que ganan más que la media.
```sql
SELECT nombre 
FROM EMPLEADOS 
WHERE salario > (SELECT AVG(salario) FROM EMPLEADOS);
```

**Subconsulta con EXISTS (Correlacionada)**
Filtrar empleados que tienen al menos una hija (basado en tabla relacionada).
```sql
SELECT nombre 
FROM empleados 
WHERE EXISTS (SELECT * FROM familiares WHERE nss = empleados.nss AND relacion = 'hija');
```

---

## 4. Comandos de SQL*Plus (Entorno)

Estos comandos son específicos de la herramienta cliente, no del lenguaje SQL estándar.

**Crear o editar un fichero de comandos**
Abre el editor definido (ej. vi o notepad).
```sql
edit nombre_fichero.sql
```

**Ejecutar un fichero de comandos**
```sql
@nombre_fichero.sql
```

**Grabar historial (Spool)**
Guarda la salida de la consola en un archivo de texto.
```sql
spool nombre_fichero  -- Iniciar grabación
-- ... tus consultas aquí ...
spool off             -- Detener y guardar
```