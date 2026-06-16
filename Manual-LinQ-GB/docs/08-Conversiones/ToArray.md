# ToArray

## 1. Introducción

`ToArray()` es un operador de conversión de LINQ que transforma una secuencia en un arreglo.

Devuelve:

```csharp
T[]
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. ¿Qué Hace ToArray?

Convierte:

```csharp
IEnumerable<T>
```

en:

```csharp
T[]
```

---

# 3. Sintaxis

```csharp
ToArray()
```

---

# 4. Ejemplo Básico

```csharp
List<int> numeros =
[
    10,
    20,
    30
];

int[] arreglo =
    numeros.ToArray();
```

Resultado:

```text
int[]
```

---

# 5. Arquitectura Interna

```text
IEnumerable
      │
      ▼
 ToArray()
      │
      ▼
Crear Arreglo
      │
      ▼
Resultado
```

---

# 6. Materialización

```csharp
var consulta =
    productos.Where(
        p => p.Stock > 0
    );

var arreglo =
    consulta.ToArray();
```

Ejecuta inmediatamente la consulta.

---

# 7. Entity Framework

```csharp
var clientes =
    contexto.Clientes
        .ToArray();
```

La consulta SQL se ejecuta cuando se llama a:

```csharp
ToArray()
```

---

# 8. Diferencia con ToList

## ToArray

```csharp
int[]
```

Tamaño fijo.

---

## ToList

```csharp
List<int>
```

Tamaño dinámico.

---

# 9. Caso Empresarial

```csharp
var codigos =
    productos
        .Select(
            p => p.Codigo
        )
        .ToArray();
```

---

# 10. Ventajas

- Más ligero.
- Más rápido para lectura.
- Ideal para datos fijos.

---

# 11. Desventajas

- Tamaño fijo.
- Menos flexible que List.

---

# 12. Resumen

`ToArray()` convierte una secuencia en un arreglo.

Características:

- Devuelve T[].
- Ejecución inmediata.
- Materializa consultas.
- Compatible con Entity Framework.
