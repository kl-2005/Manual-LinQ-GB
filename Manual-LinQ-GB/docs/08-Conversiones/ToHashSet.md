# ToHashSet

## 1. Introducción

`ToHashSet()` es un operador de conversión que transforma una secuencia en una colección de tipo:

```csharp
HashSet<T>
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. ¿Qué Hace ToHashSet?

Convierte:

```csharp
IEnumerable<T>
```

en:

```csharp
HashSet<T>
```

eliminando automáticamente los elementos duplicados.

---

# 3. Sintaxis

```csharp
ToHashSet()
```

---

# 4. Ejemplo Básico

```csharp
List<int> numeros =
[
    1,
    1,
    2,
    2,
    3,
    3
];

HashSet<int> resultado =
    numeros.ToHashSet();
```

Resultado:

```text
1
2
3
```

---

# 5. Arquitectura Interna

```text
Colección
      │
      ▼
ToHashSet()
      │
      ▼
Eliminar Duplicados
      │
      ▼
HashSet<T>
```

---

# 6. Búsquedas Rápidas

```csharp
resultado.Contains(2);
```

Complejidad:

```text
O(1)
```

---

# 7. HashSet y Duplicados

```csharp
HashSet<int> numeros =
[
    10,
    10,
    10
];
```

Resultado:

```text
10
```

---

# 8. Entity Framework

```csharp
var ids =
    contexto.Clientes
        .Select(
            c => c.Id
        )
        .ToHashSet();
```

---

# 9. Caso Empresarial

Correos únicos:

```csharp
var correos =
    clientes
        .Select(
            c => c.Email
        )
        .ToHashSet();
```

Productos únicos:

```csharp
var productos =
    ventas
        .Select(
            v => v.CodigoProducto
        )
        .ToHashSet();
```

---

# 10. Ventajas

- Elimina duplicados.
- Búsquedas rápidas.
- Excelente rendimiento.
- Muy útil para validaciones.

---

# 11. Diferencia con List

## List

```csharp
Permite duplicados
```

---

## HashSet

```csharp
No permite duplicados
```

---

# 12. Ejemplos Reales

```csharp
var permisos =
    usuarios
        .Select(
            u => u.Rol
        )
        .ToHashSet();
```

```csharp
var categorias =
    productos
        .Select(
            p => p.Categoria
        )
        .ToHashSet();
```

---

# 13. Resumen

`ToHashSet()` convierte una secuencia en un HashSet.

Características:

- Elimina duplicados.
- Búsquedas O(1).
- Ejecución inmediata.
- Muy eficiente.
- Ideal para validaciones y filtrados.
