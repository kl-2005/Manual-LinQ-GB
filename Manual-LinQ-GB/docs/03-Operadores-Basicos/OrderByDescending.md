# OrderByDescending

## 1. Introducción

`OrderByDescending()` es un operador de LINQ que permite ordenar una secuencia en orden descendente.

Es el equivalente inverso de:

```csharp
OrderBy()
```

Se utiliza para mostrar primero los valores más altos, más recientes o más importantes.

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

```csharp
OrderByDescending(
    Func<TSource,TKey> keySelector
)
```

Donde:

- TSource = Tipo de la colección.
- TKey = Campo utilizado para ordenar.

---

# 3. ¿Cómo Funciona?

Colección:

```csharp
List<int> numeros =
[
    10,
    50,
    20,
    40,
    30
];
```

Consulta:

```csharp
var resultado =
    numeros.OrderByDescending(
        n => n
    );
```

Resultado:

```text
50
40
30
20
10
```

---

# 4. Arquitectura Interna

```text
Colección
     │
     ▼
OrderByDescending()
     │
     ▼
Campo Clave
     │
     ▼
Orden Descendente
```

---

# 5. Ejemplo Básico

```csharp
var resultado =
    numeros.OrderByDescending(
        n => n
    );
```

Salida:

```text
50
40
30
20
10
```

---

# 6. Ordenamiento de Strings

```csharp
var resultado =
    nombres.OrderByDescending(
        n => n
    );
```

Resultado:

```text
Z → A
```

---

# 7. Ordenamiento de Objetos

```csharp
var resultado =
    productos.OrderByDescending(
        p => p.Precio
    );
```

Resultado:

```text
Producto más caro primero
```

---

# 8. Top Ventas

```csharp
var topVentas =
    ventas
        .OrderByDescending(
            v => v.Total
        )
        .Take(10);
```

Resultado:

```text
Top 10 ventas
```

---

# 9. Deferred Execution

```csharp
var consulta =
    ventas.OrderByDescending(
        v => v.Total
    );
```

No se ejecuta hasta:

```csharp
consulta.ToList();
```

---

# 10. IQueryable

```csharp
var resultado =
    contexto.Productos
            .OrderByDescending(
                p => p.Precio
            );
```

SQL:

```sql
SELECT *
FROM Productos
ORDER BY Precio DESC;
```

---

# 11. Combinación con Take

```csharp
var mejores =
    productos
        .OrderByDescending(
            p => p.Stock
        )
        .Take(5);
```

---

# 12. Caso Empresarial

```csharp
var productosPremium =
    contexto.Productos
            .OrderByDescending(
                p => p.Precio
            );
```

---

# 13. Ventajas

- Orden descendente.
- Compatible con SQL.
- Ideal para rankings.
- Compatible con EF Core.

---

# 14. Resumen

OrderByDescending permite ordenar datos de mayor a menor utilizando una clave específica y es ampliamente utilizado en reportes, dashboards y rankings empresariales.
