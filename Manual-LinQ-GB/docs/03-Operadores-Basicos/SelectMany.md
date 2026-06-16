# SelectMany

## 1. Introducción

`SelectMany()` permite aplanar colecciones anidadas en una sola secuencia.

Es uno de los operadores más potentes de LINQ.

---

# 2. Problema

Entidad:

```csharp
public class Cliente
{
    public string Nombre { get; set; }

    public List<Pedido> Pedidos { get; set; }
}
```

Cada cliente tiene varios pedidos.

---

# 3. Select

```csharp
var resultado =
    clientes.Select(
        c => c.Pedidos
    );
```

Resultado:

```text
Lista<List<Pedido>>
```

---

# 4. SelectMany

```csharp
var resultado =
    clientes.SelectMany(
        c => c.Pedidos
    );
```

Resultado:

```text
Lista<Pedido>
```

---

# 5. Arquitectura

```text
Clientes
     │
     ▼
Pedidos
     │
     ▼
SelectMany
     │
     ▼
Todos los Pedidos
```

---

# 6. Ejemplo

```csharp
var pedidos =
    clientes.SelectMany(
        c => c.Pedidos
    );
```

---

# 7. SQL Aproximado

```sql
SELECT *
FROM Pedidos;
```

---

# 8. Caso Empresarial

```csharp
var productosVendidos =
    ventas.SelectMany(
        v => v.Detalles
    );
```

---

# 9. Ventajas

- Elimina estructuras anidadas.
- Facilita reportes.
- Compatible con EF Core.

---

# 10. Resumen

SelectMany transforma múltiples colecciones internas en una sola secuencia continua.
