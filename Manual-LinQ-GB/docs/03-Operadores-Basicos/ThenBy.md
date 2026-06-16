# ThenBy

## 1. Introducción

`ThenBy()` es un operador de LINQ que permite realizar **ordenamientos secundarios** sobre una secuencia previamente ordenada mediante `OrderBy()` o `OrderByDescending()`.

Se utiliza cuando varios elementos tienen el mismo valor en el criterio principal y se necesita un segundo criterio para determinar el orden final.

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. ¿Por Qué Existe ThenBy?

Supongamos la siguiente lista:

| Nombre  | Ciudad |
| ------- | ------ |
| Gabriel | Quito  |
| Ana     | Ambato |
| Carlos  | Quito  |
| Pedro   | Ambato |

Si ordenamos únicamente por Ciudad:

```csharp
.OrderBy(c => c.Ciudad)
```

Obtendremos:

```text
Ambato
Ambato
Quito
Quito
```

Pero dentro de cada ciudad el orden no está definido.

Para ordenar también por Nombre:

```csharp
.OrderBy(c => c.Ciudad)
.ThenBy(c => c.Nombre)
```

Resultado:

```text
Ambato - Ana
Ambato - Pedro
Quito - Carlos
Quito - Gabriel
```

---

# 3. Sintaxis

```csharp
ThenBy(Func<TSource,TKey> keySelector)
```

Donde:

* `TSource` = Tipo de la colección.
* `TKey` = Campo secundario de ordenamiento.

---

# 4. Arquitectura Interna

```text
Colección
     │
     ▼
OrderBy()
     │
     ▼
Agrupación por Clave Principal
     │
     ▼
ThenBy()
     │
     ▼
Orden Secundario
     │
     ▼
Resultado Final
```

---

# 5. Ejemplo Básico

Entidad:

```csharp
public class Persona
{
    public string Nombre { get; set; }

    public string Ciudad { get; set; }
}
```

Consulta:

```csharp
var resultado =
    personas
        .OrderBy(p => p.Ciudad)
        .ThenBy(p => p.Nombre);
```

Resultado:

```text
Ambato - Ana
Ambato - Pedro
Quito - Carlos
Quito - Gabriel
```

---

# 6. Múltiples Niveles de Ordenamiento

LINQ permite encadenar varios ThenBy.

```csharp
var resultado =
    empleados
        .OrderBy(e => e.Departamento)
        .ThenBy(e => e.Cargo)
        .ThenBy(e => e.Nombre);
```

Proceso:

```text
Departamento
      ↓
Cargo
      ↓
Nombre
```

---

# 7. ThenBy con Valores Numéricos

Entidad:

```csharp
public class Producto
{
    public string Categoria { get; set; }

    public decimal Precio { get; set; }
}
```

Consulta:

```csharp
var productosOrdenados =
    productos
        .OrderBy(p => p.Categoria)
        .ThenBy(p => p.Precio);
```

Resultado:

```text
Electrónicos - 100
Electrónicos - 200
Electrónicos - 300

Hogar - 50
Hogar - 75
```

---

# 8. ThenByDescending

Para orden descendente secundario:

```csharp
var resultado =
    empleados
        .OrderBy(e => e.Departamento)
        .ThenByDescending(e => e.Salario);
```

Proceso:

```text
Departamento ASC
Salario DESC
```

---

# 9. Deferred Execution

ThenBy también utiliza ejecución diferida.

```csharp
var consulta =
    clientes
        .OrderBy(c => c.Ciudad)
        .ThenBy(c => c.Nombre);
```

En este punto:

```text
Todavía no se ejecuta
```

La ejecución ocurre cuando:

```csharp
consulta.ToList();
```

o

```csharp
foreach(var item in consulta)
{
}
```

---

# 10. ThenBy en IEnumerable

```csharp
IEnumerable<Cliente> resultado =
    clientes
        .OrderBy(c => c.Ciudad)
        .ThenBy(c => c.Nombre);
```

El ordenamiento se realiza en memoria RAM.

---

# 11. ThenBy en IQueryable

```csharp
var consulta =
    contexto.Clientes
            .OrderBy(c => c.Ciudad)
            .ThenBy(c => c.Nombre);
```

SQL generado:

```sql
SELECT *
FROM Clientes
ORDER BY Ciudad ASC,
         Nombre ASC;
```

---

# 12. Combinación con Where

```csharp
var resultado =
    clientes
        .Where(c => c.Activo)
        .OrderBy(c => c.Ciudad)
        .ThenBy(c => c.Nombre);
```

Proceso:

```text
Where
   ↓
OrderBy
   ↓
ThenBy
   ↓
Resultado
```

---

# 13. Combinación con Select

```csharp
var resultado =
    clientes
        .OrderBy(c => c.Ciudad)
        .ThenBy(c => c.Nombre)
        .Select(c => new
        {
            c.Nombre,
            c.Ciudad
        });
```

---

# 14. Caso Empresarial

Entidad:

```csharp
public class Venta
{
    public string Vendedor { get; set; }

    public string Ciudad { get; set; }

    public decimal Total { get; set; }
}
```

Consulta:

```csharp
var reporte =
    contexto.Ventas
            .OrderBy(v => v.Ciudad)
            .ThenBy(v => v.Vendedor);
```

SQL aproximado:

```sql
SELECT *
FROM Ventas
ORDER BY Ciudad ASC,
         Vendedor ASC;
```

---

# 15. Ventajas

## Ordenamiento Jerárquico

Permite ordenar por múltiples campos.

---

## Consultas Más Precisas

Produce resultados más organizados.

---

## Compatible con SQL

Entity Framework traduce fácilmente la consulta.

---

## Fácil Lectura

Expresa claramente los niveles de ordenamiento.

---

# 16. Errores Comunes

Incorrecto:

```csharp
clientes
    .OrderBy(c => c.Ciudad)
    .OrderBy(c => c.Nombre);
```

Problema:

```text
El segundo OrderBy reemplaza al primero.
```

---

Correcto:

```csharp
clientes
    .OrderBy(c => c.Ciudad)
    .ThenBy(c => c.Nombre);
```

---

# 17. Diferencia Entre OrderBy y ThenBy

| Operador | Función          |
| -------- | ---------------- |
| OrderBy  | Orden principal  |
| ThenBy   | Orden secundario |

Ejemplo:

```csharp
.OrderBy(c => c.Ciudad)
```

↓

```text
Primer criterio
```

Ejemplo:

```csharp
.ThenBy(c => c.Nombre)
```

↓

```text
Segundo criterio
```

---

# 18. Flujo Completo

```text
Colección
      │
      ▼
OrderBy()
      │
      ▼
Primer Ordenamiento
      │
      ▼
ThenBy()
      │
      ▼
Segundo Ordenamiento
      │
      ▼
Resultado Final
```

---

# 19. Ejemplo Completo

```csharp
var empleados =
    listaEmpleados
        .OrderBy(e => e.Departamento)
        .ThenBy(e => e.Cargo)
        .ThenBy(e => e.Nombre)
        .ToList();
```

SQL equivalente:

```sql
ORDER BY
Departamento ASC,
Cargo ASC,
Nombre ASC
```

---

# 20. Resumen

`ThenBy()` permite aplicar criterios de ordenamiento adicionales a una secuencia ya ordenada.

Características principales:

* Requiere un OrderBy previo.
* Permite múltiples niveles de ordenamiento.
* Implementa Deferred Execution.
* Funciona con IEnumerable e IQueryable.
* Se traduce eficientemente a SQL mediante Entity Framework Core.
* Mejora la organización de los datos.
* Es ampliamente utilizado en reportes y sistemas empresariales.

En aplicaciones reales, `ThenBy()` es esencial para ordenar datos por múltiples columnas, tal como ocurre en consultas SQL con varias cláusulas `ORDER BY`.
