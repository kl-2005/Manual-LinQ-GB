# Concat

## 1. Introducción

`Concat()` permite unir dos secuencias en una única secuencia manteniendo todos los elementos, incluidos los duplicados.

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

```csharp
Concat(IEnumerable<T> second)
```

---

# 3. ¿Cómo Funciona?

Colección A:

```csharp
List<int> numeros1 =
[
    1,
    2,
    3
];
```

Colección B:

```csharp
List<int> numeros2 =
[
    4,
    5,
    6
];
```

Consulta:

```csharp
var resultado =
    numeros1.Concat(numeros2);
```

Resultado:

```text
1
2
3
4
5
6
```

---

# 4. Concat Conserva Duplicados

```csharp
List<int> lista1 =
[
    1,
    2,
    3
];

List<int> lista2 =
[
    3,
    4,
    5
];

var resultado =
    lista1.Concat(lista2);
```

Resultado:

```text
1
2
3
3
4
5
```

---

# 5. Arquitectura Interna

```text
Colección A
      │
      ▼
   Concat
      ▲
      │
Colección B
      │
      ▼
Nueva Colección
```

---

# 6. Deferred Execution

```csharp
var consulta =
    lista1.Concat(lista2);
```

La operación se ejecuta cuando se recorre la secuencia.

---

# 7. IQueryable

```csharp
var resultado =
    consulta1.Concat(consulta2);
```

Entity Framework puede traducirlo como:

```sql
UNION ALL
```

---

# 8. Caso Empresarial

```csharp
var todosLosClientes =
    clientesActivos
        .Concat(clientesInactivos);
```

---

# 9. Diferencia con Union

| Método | Duplicados |
|----------|----------|
| Concat | Sí |
| Union | No |

---

# 10. Resumen

Concat une dos secuencias manteniendo todos los elementos y respetando los duplicados.
