# TakeWhile

## 1. Introducción

`TakeWhile()` obtiene elementos mientras una condición sea verdadera.

Cuando la condición falla, la lectura termina.

---

# 2. Sintaxis

```csharp
TakeWhile(
    Func<T,bool> predicate
)
```

---

# 3. Ejemplo Básico

```csharp
List<int> numeros =
[
    1,
    2,
    3,
    6,
    7,
    8
];

var resultado =
    numeros.TakeWhile(
        n => n < 5
    );
```

Proceso:

```text
1 → tomar
2 → tomar
3 → tomar
6 → detener
```

Resultado:

```text
1
2
3
```

---

# 4. Arquitectura

```text
Colección
      │
      ▼
TakeWhile
      │
      ▼
Condición Verdadera
      │
      ▼
Tomar
      │
      ▼
Condición Falsa
      │
      ▼
Detener
```

---

# 5. Diferencia con Take

## Take

```csharp
.Take(3)
```

Toma cantidad fija.

---

## TakeWhile

```csharp
.TakeWhile(
    x => x < 5
)
```

Toma según condición.

---

# 6. Caso Empresarial

```csharp
var ventasDelDia =
    ventas.TakeWhile(
        v => v.Fecha == DateTime.Today
    );
```

---

# 7. Comparación

| Método | Función |
|----------|----------|
| Take | Cantidad fija |
| TakeWhile | Condición |
| Skip | Omitir cantidad |
| SkipWhile | Omitir condición |

---

# 8. Resumen

TakeWhile obtiene elementos mientras una condición permanezca verdadera y detiene la enumeración al primer elemento que no cumple la condición.
