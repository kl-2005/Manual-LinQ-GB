# Prepend

## 1. Introducción

`Prepend()` permite agregar un elemento al inicio de una secuencia sin modificar la colección original.

Es el complemento directo de:

```csharp
Append()
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

```csharp
Prepend(TSource element)
```

---

# 3. Ejemplo Básico

```csharp
List<int> numeros =
[
    2,
    3,
    4
];

var resultado =
    numeros.Prepend(1);
```

Resultado:

```text
1
2
3
4
```

---

# 4. Arquitectura

```text
Colección
    │
    ▼
Prepend()
    │
    ▼
Agregar al Inicio
    │
    ▼
Nueva Secuencia
```

---

# 5. Encadenamiento

```csharp
var resultado =
    numeros
        .Prepend(0)
        .Prepend(-1);
```

Resultado:

```text
-1
0
1
2
3
4
```

---

# 6. Deferred Execution

```csharp
var consulta =
    numeros.Prepend(0);
```

No se ejecuta hasta enumerar la secuencia.

---

# 7. Diferencia con Append

| Método | Acción |
|----------|----------|
| Prepend | Inicio |
| Append | Final |

---

# 8. Caso Empresarial

```csharp
var productos =
    listaProductos.Prepend(
        productoPromocional
    );
```

---

# 9. Resumen

Prepend agrega un elemento al inicio de una secuencia manteniendo la colección original intacta.
