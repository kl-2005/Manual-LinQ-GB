# Join

## 1. Introducción

`Join()` es un operador de LINQ que permite combinar dos colecciones utilizando una clave común.

Es equivalente al:

```sql
INNER JOIN
```

de SQL.

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. ¿Qué Hace Join?

Relaciona registros de dos colecciones.

Ejemplo:

```text
Clientes
    │
    └── Id

Pedidos
    │
    └── ClienteId
```

Resultado:

```text
Cliente + Pedido
```

---

# 3. Sintaxis

```csharp
Join(
    inner,
    outerKeySelector,
    innerKeySelector,
    resultSelector
)
```

---

# 4. Ejemplo Básico

```csharp
var resultado =
    clientes.Join(
        pedidos,
        c => c.Id,
        p => p.ClienteId,
        (c,p) => new
        {
            Cliente = c.Nombre,
            Pedido = p.Id
        }
    );
```

---

# 5. Arquitectura Interna

```text
Colección A
      │
      ▼
     Join
      ▲
      │
Colección B
      │
      ▼
Resultado
```

---

# 6. SQL Equivalente

```sql
SELECT *
FROM Clientes c
INNER JOIN Pedidos p
ON c.Id = p.ClienteId
```

---

# 7. Caso Empresarial

```csharp
var ventas =
    clientes.Join(
        facturas,
        c => c.Id,
        f => f.ClienteId,
        (c,f) => new
        {
            c.Nombre,
            f.Total
        }
    );
```

---

# 8. Ventajas

- Relaciona datos.
- Compatible con EF Core.
- Similar a SQL.

---

# 9. Resumen

`Join()` combina dos colecciones mediante una clave común.
