# Reverse

## 1. Introducción

`Reverse()` invierte el orden de los elementos de una secuencia.

---

# 2. Sintaxis

```csharp
Reverse()
```

---

# 3. Ejemplo Básico

```csharp
List<int> numeros =
[
    1,
    2,
    3,
    4,
    5
];

var resultado =
    numeros.Reverse();
```

Resultado:

```text
5
4
3
2
1
```

---

# 4. Arquitectura

```text
1 2 3 4 5
      ↓
Reverse
      ↓
5 4 3 2 1
```

---

# 5. Reverse con Strings

```csharp
var resultado =
    nombres.Reverse();
```

---

# 6. Deferred Execution

```csharp
var consulta =
    numeros.Reverse();
```

---

# 7. Reverse vs OrderByDescending

## Reverse

```csharp
5
4
3
2
1
```

Invierte el orden actual.

---

## OrderByDescending

```csharp
5
4
3
2
1
```

Reordena según una clave.

---

# 8. Caso Empresarial

Mostrar registros más recientes:

```csharp
var recientes =
    historial.Reverse();
```

---

# 9. Resumen

Reverse invierte completamente el orden de una secuencia existente.
