# Except

## 1. Introducción

`Except()` devuelve los elementos de la primera colección que no existen en la segunda colección.

Es equivalente a una diferencia de conjuntos.

---

# 2. Sintaxis

```csharp
Except(IEnumerable<T> second)
```

---

# 3. Ejemplo Básico

```csharp
List<int> lista1 =
[
    1,
    2,
    3,
    4,
    5
];

List<int> lista2 =
[
    3,
    4
];

var resultado =
    lista1.Except(lista2);
```

Resultado:

```text
1
2
5
```

---

# 4. Representación Visual

```text
A = {1,2,3,4,5}

B = {3,4}

A - B

{1,2,5}
```

---

# 5. Arquitectura

```text
Colección A
      │
      ▼
  Except
      ▲
      │
Colección B
      │
      ▼
Elementos Exclusivos
```

---

# 6. Deferred Execution

```csharp
var consulta =
    lista1.Except(lista2);
```

La ejecución ocurre al recorrer la secuencia.

---

# 7. IQueryable

```csharp
var resultado =
    consulta1.Except(consulta2);
```

Puede traducirse a SQL según el proveedor de base de datos.

---

# 8. Caso Empresarial

Clientes que compraron en 2025 pero no en 2024:

```csharp
var nuevosClientes =
    clientes2025.Except(
        clientes2024
    );
```

Resultado:

```text
Clientes nuevos
```

---

# 9. Ventajas

- Detecta diferencias.
- Ideal para auditorías.
- Compatible con EF Core.

---

# 10. Comparación General

| Operador | Resultado |
|-----------|-----------|
| Concat | Une todo |
| Union | Une sin duplicados |
| Intersect | Coincidencias |
| Except | Diferencias |

---

# 11. Resumen

Except devuelve los elementos que pertenecen exclusivamente a la primera colección y no existen en la segunda.
