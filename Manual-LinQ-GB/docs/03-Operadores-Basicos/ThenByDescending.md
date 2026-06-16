# ThenByDescending

## 1. Introducción

`ThenByDescending()` permite aplicar un ordenamiento secundario descendente después de un `OrderBy()` o `OrderByDescending()`.

---

# 2. Sintaxis

```csharp
ThenByDescending(
    Func<TSource,TKey> keySelector
)
```

---

# 3. Ejemplo

```csharp
var resultado =
    empleados
        .OrderBy(
            e => e.Departamento
        )
        .ThenByDescending(
            e => e.Salario
        );
```

Resultado:

```text
Departamento ASC
Salario DESC
```

---

# 4. SQL Generado

```sql
SELECT *
FROM Empleados
ORDER BY Departamento ASC,
         Salario DESC;
```

---

# 5. Uso Empresarial

```csharp
var ranking =
    vendedores
        .OrderBy(
            v => v.Ciudad
        )
        .ThenByDescending(
            v => v.Ventas
        );
```

---

# 6. Diferencia con ThenBy

| Método | Orden |
|----------|----------|
| ThenBy | ASC |
| ThenByDescending | DESC |

---

# 7. Resumen

Permite ordenar por múltiples campos aplicando un criterio descendente en niveles secundarios de ordenamiento.
