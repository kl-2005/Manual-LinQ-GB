# IQueryable<T>

`IQueryable<T>` es una interfaz que extiende `IEnumerable<T>` y permite construir consultas que pueden ser traducidas y ejecutadas por un proveedor externo, como una base de datos.

Es uno de los componentes fundamentales de **Entity Framework Core**, ya que permite que las consultas LINQ escritas en C# sean convertidas a SQL.

Pertenece al namespace:

```csharp
System.Linq
```

---

# 1. ¿Qué es IQueryable?

Mientras que `IEnumerable<T>` trabaja con datos que ya están cargados en memoria RAM, `IQueryable<T>` representa una consulta que aún no ha sido ejecutada.

Por ejemplo:

```csharp
var clientes =
    contexto.Clientes;
```

Tipo real:

```csharp
IQueryable<Cliente>
```

En este punto:

```text
Todavía no se ejecuta ninguna consulta SQL.
```

Solo se construye una representación lógica de la consulta.

---

# 2. ¿Por Qué Existe?

Supongamos que tenemos una tabla con un millón de registros.

### Opción Incorrecta

```csharp
var clientes =
    contexto.Clientes
            .ToList();

var activos =
    clientes.Where(c => c.Activo);
```

Proceso:

```text
Base de Datos
      ↓
1.000.000 registros
      ↓
Memoria RAM
      ↓
Filtro Activo
```

Problema:

```text
Se transfieren todos los registros.
```

---

### Opción Correcta

```csharp
var activos =
    contexto.Clientes
            .Where(c => c.Activo);
```

Proceso:

```text
Base de Datos
      ↓
WHERE Activo = 1
      ↓
Solo registros necesarios
```

Beneficio:

```text
Menor consumo de memoria.
Menor tráfico de red.
Mayor rendimiento.
```

---

# 3. Herencia de Interfaces

La definición simplificada es:

```csharp
public interface IQueryable<T>
    : IEnumerable<T>,
      IQueryable,
      IEnumerable
{
}
```

Arquitectura:

```text
IEnumerable
      │
      ▼
IEnumerable<T>
      │
      ▼
IQueryable<T>
```

Por eso todo `IQueryable` también puede ser recorrido con:

```csharp
foreach
```

---

# 4. Componentes Internos

La interfaz contiene tres propiedades importantes:

```csharp
public interface IQueryable
{
    Expression Expression { get; }

    Type ElementType { get; }

    IQueryProvider Provider { get; }
}
```

---

## Expression

Contiene el árbol de expresiones.

Ejemplo:

```csharp
contexto.Clientes
        .Where(c => c.Activo)
```

Genera algo similar a:

```text
Where
 │
 └── c.Activo
```

---

## Provider

Es el encargado de interpretar el árbol.

Por ejemplo:

```text
Entity Framework Core
```

analiza la expresión y genera SQL.

---

## ElementType

Representa el tipo de entidad.

```csharp
Cliente
```

---

# 5. IQueryable y Expression Trees

La gran diferencia con `IEnumerable` es que utiliza:

```csharp
Expression<Func<T,bool>>
```

en lugar de:

```csharp
Func<T,bool>
```

---

## IEnumerable

```csharp
Func<Cliente,bool>
```

Representación:

```text
Código compilado
```

---

## IQueryable

```csharp
Expression<Func<Cliente,bool>>
```

Representación:

```text
Árbol de Expresiones
```

---

# 6. Flujo Completo en Entity Framework

Consulta:

```csharp
var consulta =
    contexto.Clientes
            .Where(c => c.Activo)
            .OrderBy(c => c.Nombre);
```

---

### Paso 1

C# genera:

```text
Expression Tree
```

---

### Paso 2

EF Core analiza:

```text
Where
OrderBy
Activo
Nombre
```

---

### Paso 3

EF Core traduce a SQL:

```sql
SELECT *
FROM Clientes
WHERE Activo = 1
ORDER BY Nombre;
```

---

### Paso 4

SQL Server ejecuta.

---

### Paso 5

Los resultados vuelven como objetos C#.

---

# 7. Ejecución Diferida

Al igual que `IEnumerable`, `IQueryable` utiliza Deferred Execution.

Ejemplo:

```csharp
var consulta =
    contexto.Clientes
            .Where(c => c.Activo);
```

En este momento:

```text
No existe consulta SQL ejecutada.
```

---

La ejecución ocurre cuando:

```csharp
consulta.ToList();
```

o

```csharp
consulta.First();
```

o

```csharp
foreach(...)
```

---

# 8. Ejemplo Visual

Código:

```csharp
var consulta =
    contexto.Clientes
            .Where(c => c.Activo)
            .OrderBy(c => c.Nombre)
            .Take(10);
```

Árbol:

```text
Take(10)
    │
OrderBy(Nombre)
    │
Where(Activo)
    │
Clientes
```

SQL generado:

```sql
SELECT TOP(10) *
FROM Clientes
WHERE Activo = 1
ORDER BY Nombre;
```

---

# 9. IQueryable vs IEnumerable

| Característica           | IEnumerable         | IQueryable    |
| ------------------------ | ------------------- | ------------- |
| Procesamiento            | Memoria RAM         | Base de Datos |
| Namespace                | Collections.Generic | Linq          |
| Utiliza Delegados        | Sí                  | No            |
| Utiliza Expression Trees | No                  | Sí            |
| Traducción SQL           | No                  | Sí            |
| EF Core                  | Limitado            | Principal     |
| Rendimiento BD           | Menor               | Mayor         |

---

# 10. Conversión Entre Ambos

## IQueryable → IEnumerable

```csharp
IQueryable<Cliente> consulta =
    contexto.Clientes;

IEnumerable<Cliente> datos =
    consulta.AsEnumerable();
```

A partir de aquí:

```text
El resto se ejecutará en memoria.
```

---

## IQueryable → List

```csharp
var lista =
    consulta.ToList();
```

Esto ejecuta la consulta inmediatamente.

---

# 11. Error Común

Incorrecto:

```csharp
var clientes =
    contexto.Clientes
            .ToList()
            .Where(c => c.Activo);
```

Proceso:

```text
SQL:
SELECT * FROM Clientes
```

Luego:

```text
Filtro en RAM
```

---

Correcto:

```csharp
var clientes =
    contexto.Clientes
            .Where(c => c.Activo)
            .ToList();
```

Proceso:

```sql
SELECT *
FROM Clientes
WHERE Activo = 1;
```

---

# 12. Caso Empresarial

Supongamos un sistema de ventas.

Entidad:

```csharp
public class Producto
{
    public int Id { get; set; }

    public string Nombre { get; set; }

    public decimal Precio { get; set; }

    public bool Activo { get; set; }
}
```

Consulta:

```csharp
var productos =
    contexto.Productos
            .Where(p => p.Activo)
            .Where(p => p.Precio > 100)
            .OrderBy(p => p.Nombre)
            .Take(20);
```

SQL aproximado:

```sql
SELECT TOP 20 *
FROM Productos
WHERE Activo = 1
AND Precio > 100
ORDER BY Nombre;
```

Todo el filtrado ocurre en el servidor.

---

# 13. Ventajas

## Alto Rendimiento

Solo se recuperan datos necesarios.

---

## Menor Consumo de Memoria

Los registros innecesarios nunca llegan a la aplicación.

---

## Integración con EF Core

Permite generar SQL automáticamente.

---

## Consultas Dinámicas

Puede combinarse con Expression Trees.

---

# 14. Desventajas

## Dependencia del Proveedor

La traducción depende de:

```text
Entity Framework
LINQ to SQL
MongoDB Provider
Cosmos DB Provider
```

---

## No Todo Puede Traducirse

Ejemplo:

```csharp
.Where(c => MiMetodoPersonalizado(c))
```

EF Core puede no saber traducirlo a SQL.

---

# 15. Arquitectura Completa

```text
LINQ Query
      │
      ▼
Expression Tree
      │
      ▼
IQueryable
      │
      ▼
IQueryProvider
      │
      ▼
SQL
      │
      ▼
Base de Datos
      │
      ▼
Resultados
      │
      ▼
Objetos C#
```

---

# 16. Resumen

`IQueryable<T>` es una interfaz especializada para construir consultas que pueden ser interpretadas por proveedores externos.

Características principales:

* Extiende `IEnumerable<T>`.
* Utiliza Expression Trees.
* Permite traducción a SQL.
* Es la base de Entity Framework Core.
* Implementa ejecución diferida.
* Mejora el rendimiento de acceso a datos.
* Reduce el consumo de memoria.
* Permite consultas dinámicas avanzadas.

En aplicaciones empresariales modernas, prácticamente todas las consultas realizadas mediante Entity Framework Core comienzan como un `IQueryable<T>` y terminan convirtiéndose en SQL ejecutado por la base de datos.
