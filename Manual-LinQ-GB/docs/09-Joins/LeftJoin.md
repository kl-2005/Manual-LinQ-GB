# LeftJoin

## 1. Introducción

LINQ no posee un método llamado:

```csharp
LeftJoin()
```

Para realizar un LEFT JOIN se utiliza:

```csharp
GroupJoin()
```

junto con:

```csharp
DefaultIfEmpty()
```

---

# 2. ¿Qué Hace?

Devuelve todos los registros de la colección izquierda aunque no tengan coincidencias.

---

# 3. Ejemplo

```csharp
var resultado =
    from c in clientes
    join p in pedidos
    on c.Id equals p.ClienteId
    into grupoPedidos
    from pedido in grupoPedidos.DefaultIfEmpty()
    select new
    {
        Cliente = c.Nombre,
        Pedido = pedido
    };
```

---

# 4. SQL Equivalente

```sql
SELECT *
FROM Clientes c
LEFT JOIN Pedidos p
ON c.Id = p.ClienteId
```

---

# 5. Resultado

```text
Juan Pedido1
Juan Pedido2
María NULL
```

---

# 6. Caso Empresarial

Clientes sin compras:

```csharp
var clientesSinCompras =
    from c in clientes
    join p in pedidos
    on c.Id equals p.ClienteId
    into gp
    from pedido in gp.DefaultIfEmpty()
    where pedido == null
    select c;
```

---

# 7. Resumen

LEFT JOIN en LINQ se construye mediante:

```csharp
GroupJoin()
```

y

```csharp
DefaultIfEmpty()
```
