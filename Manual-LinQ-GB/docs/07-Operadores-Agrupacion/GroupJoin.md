# GroupJoin

## 1. Introducción

`GroupJoin()` es un operador de LINQ que permite relacionar dos colecciones y agrupar los elementos coincidentes.

Es una combinación entre:

```csharp
Join()
```

y

```csharp
GroupBy()
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. ¿Qué Hace GroupJoin?

Relaciona:

```text
Colección Padre
```

con

```text
Colección Hija
```

agrupando los elementos relacionados.

---

# 3. Ejemplo Conceptual

Departamentos:

```text
Ventas
Sistemas
Contabilidad
```

Empleados:

```text
Juan → Ventas
Ana → Ventas
Luis → Sistemas
```

Resultado:

```text
Ventas
 ├─ Juan
 └─ Ana

Sistemas
 └─ Luis

Contabilidad
 └─ Sin empleados
```

---

# 4. Sintaxis

```csharp
GroupJoin(
    inner,
    outerKeySelector,
    innerKeySelector,
    resultSelector
)
```

---

# 5. Clases de Ejemplo

```csharp
public class Departamento
{
    public int Id { get; set; }
    public string Nombre { get; set; }
}

public class Empleado
{
    public string Nombre { get; set; }
    public int DepartamentoId { get; set; }
}
```

---

# 6. Ejemplo Básico

```csharp
var resultado =
    departamentos.GroupJoin(
        empleados,
        d => d.Id,
        e => e.DepartamentoId,
        (departamento, empleadosGrupo)
            => new
            {
                Departamento =
                    departamento.Nombre,
                Empleados =
                    empleadosGrupo
            }
    );
```

---

# 7. Arquitectura Interna

```text
Departamentos
      │
      ▼
 GroupJoin
      │
      ▼
Relacionar Claves
      │
      ▼
Crear Grupos
      │
      ▼
Resultado
```

---

# 8. Recorrer Resultado

```csharp
foreach(var grupo in resultado)
{
    Console.WriteLine(
        grupo.Departamento
    );

    foreach(var empleado in grupo.Empleados)
    {
        Console.WriteLine(
            empleado.Nombre
        );
    }
}
```

---

# 9. Resultado Esperado

```text
Ventas
Juan
Ana

Sistemas
Luis

Contabilidad
```

---

# 10. Relación Uno a Muchos

GroupJoin representa:

```text
1 → N
```

Ejemplo:

```text
Cliente → Facturas
Departamento → Empleados
Categoría → Productos
```

---

# 11. Entity Framework

Consulta:

```csharp
var resultado =
    contexto.Departamentos
        .GroupJoin(
            contexto.Empleados,
            d => d.Id,
            e => e.DepartamentoId,
            (d, e) =>
                new
                {
                    Departamento = d,
                    Empleados = e
                }
        );
```

---

# 12. SQL Aproximado

```sql
SELECT *
FROM Departamentos d
LEFT JOIN Empleados e
ON d.Id = e.DepartamentoId
```

---

# 13. Caso Empresarial

Clientes y facturas:

```csharp
clientes.GroupJoin(
    facturas,
    c => c.Id,
    f => f.ClienteId,
    (c,f) => new
    {
        Cliente = c,
        Facturas = f
    }
);
```

---

# 14. Productos y Categorías

```csharp
categorias.GroupJoin(
    productos,
    c => c.Id,
    p => p.CategoriaId,
    (c,p) => new
    {
        Categoria = c,
        Productos = p
    }
);
```

---

# 15. Diferencia entre Join y GroupJoin

## Join

```csharp
Join()
```

Resultado:

```text
1 fila por coincidencia
```

---

## GroupJoin

```csharp
GroupJoin()
```

Resultado:

```text
1 grupo por elemento principal
```

---

# 16. Ventajas

- Relaciones uno a muchos.
- Compatible con Entity Framework.
- Muy útil para reportes.
- Base conceptual de los Left Join.

---

# 17. Ejemplos Reales

Departamento → Empleados

```csharp
departamentos.GroupJoin(...)
```

Cliente → Facturas

```csharp
clientes.GroupJoin(...)
```

Curso → Estudiantes

```csharp
cursos.GroupJoin(...)
```

---

# 18. Resumen

`GroupJoin()` relaciona dos colecciones agrupando los elementos coincidentes.

Características:

- Relación uno a muchos.
- Similar a LEFT JOIN.
- Compatible con Entity Framework.
- Muy utilizado en sistemas empresariales.
- Genera grupos de elementos relacionados.
