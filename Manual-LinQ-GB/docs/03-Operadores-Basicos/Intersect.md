# Intersect

## 1. Introducción

`Intersect()` devuelve únicamente los elementos que existen en ambas secuencias.

Es equivalente a una intersección matemática de conjuntos.

---

# 2. Sintaxis

```csharp
Intersect(IEnumerable<T> second)
```

---

# 3. Ejemplo Básico

```csharp
List<int> lista1 =
[
    1,
    2,
    3,
    4
];

List<int> lista2 =
[
    3,
    4,
    5,
    6
];

var resultado =
    lista1.Intersect(lista2);
```

Resultado:

```text
3
4
```

---

# 4. Representación Visual

```text
A = {1,2,3,4}

B = {3,4,5,6}

Intersección

{3,4}
```

---

# 5. Arquitectura

```text
Colección A
      │
      ▼
 Intersect
      ▲
      │
Colección B
      │
      ▼
Coincidencias
```

---

# 6. Deferred Execution

```csharp
var consulta =
    lista1.Intersect(lista2);
```

No se ejecuta inmediatamente.

---

# 7. IQueryable

```csharp
var resultado =
    consulta1.Intersect(consulta2);
```

Puede traducirse a SQL mediante consultas de intersección equivalentes.

---

# 8. Caso Empresarial

Clientes que compraron en ambos años:

```csharp
var clientesComunes =
    clientes2024.Intersect(
        clientes2025
    );
```

Resultado:

```text
Clientes presentes en ambas listas
```

---

# 9. Ventajas

- Encuentra coincidencias.
- Reduce procesamiento manual.
- Compatible con LINQ.

---

# 10. Resumen

Intersect devuelve únicamente los elementos que aparecen simultáneamente en ambas colecciones.
