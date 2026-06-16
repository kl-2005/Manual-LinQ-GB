# CrossJoin

## 1. Introducción

Un Cross Join genera todas las combinaciones posibles entre dos colecciones.

Equivale a:

```sql
CROSS JOIN
```

---

# 2. ¿Qué Hace?

Si tenemos:

```text
3 clientes
```

y

```text
2 productos
```

Resultado:

```text
6 combinaciones
```

---

# 3. Ejemplo

```csharp
var resultado =
    from cliente in clientes
    from producto in productos
    select new
    {
        Cliente = cliente.Nombre,
        Producto = producto.Nombre
    };
```

---

# 4. SQL Equivalente

```sql
SELECT *
FROM Clientes
CROSS JOIN Productos
```

---

# 5. Resultado

```text
Juan Laptop
Juan Mouse

Ana Laptop
Ana Mouse

Luis Laptop
Luis Mouse
```

---

# 6. Arquitectura

```text
Clientes
    ×
Productos
    │
    ▼
Combinaciones
```

---

# 7. Caso Empresarial

Generar todas las combinaciones posibles:

```csharp
var promociones =
    from producto in productos
    from ciudad in ciudades
    select new
    {
        producto.Nombre,
        ciudad.Nombre
    };
```

---

# 8. Precaución

Puede generar enormes cantidades de registros.

Ejemplo:

```text
1000 x 1000
=
1,000,000 filas
```

---

# 9. Resumen

Cross Join combina todos los elementos de ambas colecciones.
