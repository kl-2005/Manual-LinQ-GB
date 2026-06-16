# InnerJoin

## 1. Introducción

En LINQ no existe un método llamado oficialmente:

```csharp
InnerJoin()
```

El operador que realiza un INNER JOIN es:

```csharp
Join()
```

---

# 2. ¿Qué es un INNER JOIN?

Devuelve únicamente los registros que coinciden en ambas colecciones.

Ejemplo:

```text
Clientes
1 Juan
2 María

Pedidos
1 Cliente 1
2 Cliente 1
```

Resultado:

```text
Juan Pedido1
Juan Pedido2
```

María no aparece porque no tiene pedidos.

---

# 3. Ejemplo

```csharp
var resultado =
    clientes.Join(
        pedidos,
        c => c.Id,
        p => p.ClienteId,
        (c,p) => new
        {
            c.Nombre,
            p.Id
        }
    );
```

---

# 4. SQL Equivalente

```sql
SELECT *
FROM Clientes c
INNER JOIN Pedidos p
ON c.Id = p.ClienteId
```

---

# 5. Resumen

En LINQ:

```csharp
Join()
```

es el equivalente directo de:

```sql
INNER JOIN
```
