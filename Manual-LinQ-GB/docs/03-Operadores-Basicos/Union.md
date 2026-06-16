# Union

## 1. Introducción

`Union()` permite combinar dos secuencias eliminando automáticamente los elementos duplicados.

Es equivalente al operador UNION de SQL.

---

# 2. Sintaxis

```csharp
Union(IEnumerable<T> second)
```

---

# 3. Ejemplo Básico

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
    lista1.Union(lista2);
```

Resultado:

```text
1
2
3
4
5
```

---

# 4. Proceso Interno

```text
Lista 1
1
2
3

Lista 2
3
4
5

Union
   ↓

1
2
3
4
5
```

---

# 5. Arquitectura

```text
Colección A
      │
      ▼
    Union
      ▲
      │
Colección B
      │
      ▼
Elementos Únicos
```

---

# 6. Union con Strings

```csharp
var resultado =
    ciudades1.Union(ciudades2);
```

Resultado:

```text
Quito
Ambato
Guayaquil
```

---

# 7. Deferred Execution

```csharp
var consulta =
    lista1.Union(lista2);
```

La consulta no se ejecuta inmediatamente.

---

# 8. IQueryable

```csharp
var resultado =
    consulta1.Union(consulta2);
```

SQL:

```sql
SELECT ...
UNION
SELECT ...
```

---

# 9. Caso Empresarial

```csharp
var ciudades =
    clientes
        .Select(c => c.Ciudad)
        .Union(
            proveedores.Select(
                p => p.Ciudad
            )
        );
```

---

# 10. Ventajas

- Elimina duplicados.
- Compatible con SQL.
- Fácil de utilizar.
- Mejora la calidad de datos.

---

# 11. Resumen

Union combina dos secuencias eliminando registros repetidos y generando un conjunto único de resultados.
