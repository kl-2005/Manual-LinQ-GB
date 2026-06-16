# Chunk

## 1. Introducción

`Chunk()` divide una secuencia en grupos de tamaño fijo.

Disponible desde:

```text
.NET 6
```

---

# 2. Sintaxis

```csharp
Chunk(int size)
```

---

# 3. Ejemplo Básico

```csharp
var numeros =
    Enumerable.Range(1,10);

var resultado =
    numeros.Chunk(3);
```

Resultado:

```text
[1,2,3]
[4,5,6]
[7,8,9]
[10]
```

---

# 4. Arquitectura

```text
1 2 3 4 5 6 7 8 9 10
            ↓
        Chunk(3)
            ↓
[1 2 3]
[4 5 6]
[7 8 9]
[10]
```

---

# 5. Recorrido

```csharp
foreach(var grupo in resultado)
{
    foreach(var item in grupo)
    {
        Console.WriteLine(item);
    }
}
```

---

# 6. Caso Empresarial

Procesar registros por lotes:

```csharp
var lotes =
    clientes.Chunk(100);
```

---

# 7. Ventajas

- Procesamiento por bloques.
- Menor consumo de memoria.
- Importaciones masivas.

---

# 8. Resumen

Chunk divide una colección en múltiples grupos de tamaño fijo.
