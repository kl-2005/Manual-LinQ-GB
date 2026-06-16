# SkipWhile

## 1. Introducción

`SkipWhile()` omite elementos mientras una condición sea verdadera.

Cuando la condición se vuelve falsa, devuelve todos los elementos restantes.

---

# 2. Sintaxis

```csharp
SkipWhile(
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
    numeros.SkipWhile(
        n => n < 5
    );
```

Proceso:

```text
1 → omitir
2 → omitir
3 → omitir
6 → detener
```

Resultado:

```text
6
7
8
```

---

# 4. Arquitectura

```text
Colección
      │
      ▼
SkipWhile
      │
      ▼
Condición Verdadera
      │
      ▼
Omitir
      │
      ▼
Condición Falsa
      │
      ▼
Devolver Restantes
```

---

# 5. Diferencia con Skip

## Skip

```csharp
.Skip(3)
```

Omite cantidad fija.

---

## SkipWhile

```csharp
.SkipWhile(
    x => x < 5
)
```

Omite según condición.

---

# 6. Caso Empresarial

```csharp
var pendientes =
    tareas.SkipWhile(
        t => t.Estado == "Completada"
    );
```

---

# 7. Resumen

SkipWhile omite elementos mientras una condición permanezca verdadera.
