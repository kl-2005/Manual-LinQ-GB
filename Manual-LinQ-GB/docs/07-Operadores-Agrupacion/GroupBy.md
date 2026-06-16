# GroupBy

## 1. Introducción

`GroupBy()` es un operador de agrupación de LINQ que permite organizar elementos de una secuencia en grupos según una clave común.

Es equivalente al:

```sql
GROUP BY
```

de SQL.

Pertenece al namespace:

```csharp
System.Linq
```

Es uno de los operadores más importantes para análisis de datos, reportes y estadísticas.

---

# 2. ¿Qué Hace GroupBy?

Agrupa elementos que comparten una característica común.

Ejemplo:

```text
Ventas
│
├─ Quito
├─ Quito
├─ Guayaquil
├─ Cuenca
├─ Guayaquil
```

Resultado:

```text
Quito
 ├─ Venta 1
 └─ Venta 2

Guayaquil
 ├─ Venta 3
 └─ Venta 5

Cuenca
 └─ Venta 4
```

---

# 3. Sintaxis

```csharp
GroupBy(
    keySelector
)
```

---

# 4. Ejemplo Básico

```csharp
List<string> ciudades =
[
    "Quito",
    "Quito",
    "Cuenca",
    "Guayaquil",
    "Cuenca"
];

var grupos =
    ciudades.GroupBy(
        c => c
    );
```

---

# 5. Recorrer los Grupos

```csharp
foreach(var grupo in grupos)
{
    Console.WriteLine(
        grupo.Key
    );

    foreach(var ciudad in grupo)
    {
        Console.WriteLine(ciudad);
    }
}
```

Resultado:

```text
Quito
Quito
Quito

Cuenca
Cuenca
Cuenca

Guayaquil
Guayaquil
```

---

# 6. Arquitectura Interna

```text
Colección
      │
      ▼
  GroupBy()
      │
      ▼
Generar Claves
      │
      ▼
Crear Grupos
      │
      ▼
Resultado
```

---

# 7. GroupBy con Objetos

Clase:

```csharp
public class Empleado
{
    public string Departamento { get; set; }
    public string Nombre { get; set; }
}
```

Consulta:

```csharp
var grupos =
    empleados.GroupBy(
        e => e.Departamento
    );
```

---

# 8. Resultado de GroupBy

Cada grupo implementa:

```csharp
IGrouping<TKey,TElement>
```

Donde:

```text
TKey
```

es la clave.

y

```text
TElement
```

es el elemento agrupado.

---

# 9. Obtener la Clave

```csharp
foreach(var grupo in grupos)
{
    Console.WriteLine(
        grupo.Key
    );
}
```

Resultado:

```text
Ventas
Sistemas
Contabilidad
```

---

# 10. Contar Elementos por Grupo

```csharp
var resultado =
    empleados.GroupBy(
        e => e.Departamento
    )
    .Select(
        g => new
        {
            Departamento = g.Key,
            Total = g.Count()
        }
    );
```

---

# 11. Sumar por Grupo

```csharp
var ventasPorCiudad =
    ventas.GroupBy(
        v => v.Ciudad
    )
    .Select(
        g => new
        {
            Ciudad = g.Key,
            Total = g.Sum(
                x => x.Total
            )
        }
    );
```

---

# 12. Promedio por Grupo

```csharp
var promedios =
    estudiantes.GroupBy(
        e => e.Curso
    )
    .Select(
        g => new
        {
            Curso = g.Key,
            Promedio = g.Average(
                x => x.Nota
            )
        }
    );
```

---

# 13. Entity Framework

Consulta:

```csharp
var resultado =
    contexto.Ventas
        .GroupBy(
            v => v.Ciudad
        )
        .Select(
            g => new
            {
                Ciudad = g.Key,
                Total = g.Count()
            }
        );
```

SQL aproximado:

```sql
SELECT Ciudad,
       COUNT(*) AS Total
FROM Ventas
GROUP BY Ciudad
```

---

# 14. Caso Empresarial

Ventas por ciudad:

```csharp
var ventas =
    contexto.Ventas
        .GroupBy(
            v => v.Ciudad
        );
```

Clientes por provincia:

```csharp
var clientes =
    contexto.Clientes
        .GroupBy(
            c => c.Provincia
        );
```

Productos por categoría:

```csharp
var productos =
    contexto.Productos
        .GroupBy(
            p => p.Categoria
        );
```

---

# 15. Agrupación Múltiple

```csharp
var resultado =
    ventas.GroupBy(
        v => new
        {
            v.Ciudad,
            v.Anio
        }
    );
```

Resultado:

```text
Quito - 2025
Quito - 2026
Guayaquil - 2025
Guayaquil - 2026
```

---

# 16. Diferencia entre GroupBy y OrderBy

## OrderBy

Ordena.

```csharp
OrderBy()
```

---

## GroupBy

Agrupa.

```csharp
GroupBy()
```

---

# 17. Rendimiento

- Muy eficiente para reportes.
- Compatible con SQL.
- Utilizado ampliamente en BI.
- Puede generar consultas complejas en bases de datos grandes.

---

# 18. Ejemplos Reales

Ventas por año:

```csharp
ventas.GroupBy(
    v => v.Anio
);
```

Facturas por mes:

```csharp
facturas.GroupBy(
    f => f.Mes
);
```

Estudiantes por carrera:

```csharp
estudiantes.GroupBy(
    e => e.Carrera
);
```

---

# 19. Resumen

`GroupBy()` agrupa elementos según una clave común.

Características:

- Equivalente a GROUP BY de SQL.
- Devuelve colecciones agrupadas.
- Compatible con Entity Framework.
- Muy utilizado en reportes.
- Permite conteos, sumas, promedios y estadísticas.
- Fundamental para análisis de datos.
