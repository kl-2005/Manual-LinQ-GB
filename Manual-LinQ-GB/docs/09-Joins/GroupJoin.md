# GroupJoin

## 1. Introducción

`GroupJoin()` permite relacionar dos colecciones agrupando los elementos relacionados.

Representa relaciones:

```text
Uno a Muchos
```

---

# 2. Ejemplo Conceptual

```text
Departamento
    │
    ├── Empleado
    ├── Empleado
    └── Empleado
```

---

# 3. Sintaxis

```csharp
GroupJoin(
    inner,
    outerKeySelector,
    innerKeySelector,
    resultSelector
)
```

---

# 4. Ejemplo

```csharp
var resultado =
    departamentos.GroupJoin(
        empleados,
        d => d.Id,
        e => e.DepartamentoId,
        (d, empleadosGrupo) =>
            new
            {
                Departamento = d.Nombre,
                Empleados = empleadosGrupo
            }
    );
```

---

# 5. Resultado

```text
Ventas
 ├ Juan
 ├ Ana

Sistemas
 ├ Luis
```

---

# 6. SQL Aproximado

```sql
SELECT *
FROM Departamentos d
LEFT JOIN Empleados e
ON d.Id = e.DepartamentoId
```

---

# 7. Caso Empresarial

```csharp
var clientesFacturas =
    clientes.GroupJoin(
        facturas,
        c => c.Id,
        f => f.ClienteId,
        (c,f) => new
        {
            Cliente = c.Nombre,
            Facturas = f
        }
    );
```

---

# 8. Ventajas

- Relación uno a muchos.
- Compatible con EF Core.
- Base para LEFT JOIN.

---

# 9. Resumen

`GroupJoin()` agrupa elementos relacionados entre dos colecciones.
