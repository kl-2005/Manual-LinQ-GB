# Aggregate

## 1. Introducción

`Aggregate()` es el operador de agregación más flexible y poderoso de LINQ.

Permite combinar todos los elementos de una secuencia en un único resultado mediante una función personalizada.

A diferencia de:

```csharp
Sum()
Average()
Min()
Max()
Count()
```

`Aggregate()` permite definir exactamente cómo se realizará la acumulación.

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. ¿Por Qué Existe?

Operadores como:

```csharp
Sum()
```

internamente realizan una acumulación.

Por ejemplo:

```csharp
1 + 2 + 3 + 4
```

Produce:

```text
10
```

`Aggregate()` permite crear nuestra propia lógica de acumulación.

---

# 3. Sintaxis

## Forma Básica

```csharp
Aggregate(
    Func<TSource,TSource,TSource> func
)
```

## Con Valor Inicial

```csharp
Aggregate(
    TAccumulate seed,
    Func<TAccumulate,TSource,TAccumulate> func
)
```

## Con Transformación Final

```csharp
Aggregate(
    seed,
    func,
    resultSelector
)
```

---

# 4. Ejemplo Básico

Colección:

```csharp
List<int> numeros =
[
    1,
    2,
    3,
    4
];
```

Consulta:

```csharp
int resultado =
    numeros.Aggregate(
        (acumulador, numero) =>
            acumulador + numero
    );
```

Proceso:

```text
1 + 2 = 3
3 + 3 = 6
6 + 4 = 10
```

Resultado:

```text
10
```

---

# 5. Arquitectura Interna

```text
Colección
      │
      ▼
 Aggregate()
      │
      ▼
Acumulador
      │
      ▼
Procesar Elemento
      │
      ▼
Actualizar Acumulador
      │
      ▼
Resultado Final
```

---

# 6. Equivalencia con Sum

```csharp
int suma =
    numeros.Sum();
```

Equivalente:

```csharp
int suma =
    numeros.Aggregate(
        (a,b) => a + b
    );
```

Resultado:

```text
10
```

---

# 7. Multiplicación de Valores

```csharp
int producto =
    numeros.Aggregate(
        (a,b) => a * b
    );
```

Proceso:

```text
1 * 2 = 2
2 * 3 = 6
6 * 4 = 24
```

Resultado:

```text
24
```

---

# 8. Concatenar Texto

```csharp
List<string> nombres =
[
    "Ana",
    "Carlos",
    "Luis"
];

string resultado =
    nombres.Aggregate(
        (a,b) => a + ", " + b
    );
```

Resultado:

```text
Ana, Carlos, Luis
```

---

# 9. Aggregate con Valor Inicial

```csharp
int total =
    numeros.Aggregate(
        100,
        (acumulador, numero) =>
            acumulador + numero
    );
```

Proceso:

```text
100 + 1 = 101
101 + 2 = 103
103 + 3 = 106
106 + 4 = 110
```

Resultado:

```text
110
```

---

# 10. Aggregate con Objetos

Clase:

```csharp
public class Venta
{
    public decimal Total { get; set; }
}
```

Consulta:

```csharp
decimal totalVentas =
    ventas.Aggregate(
        0m,
        (acumulado, venta) =>
            acumulado + venta.Total
    );
```

Resultado:

```text
Suma total de ventas
```

---

# 11. Aggregate con Entity Framework

Entity Framework generalmente no traduce Aggregate personalizado a SQL.

Ejemplo:

```csharp
var resultado =
    contexto.Productos
        .AsEnumerable()
        .Aggregate(
            (a,b) => a
        );
```

Por ello suele utilizarse más sobre:

```csharp
IEnumerable<T>
```

que sobre:

```csharp
IQueryable<T>
```

---

# 12. Aggregate con Transformación Final

```csharp
string resultado =
    numeros.Aggregate(
        0,
        (acc,n) => acc + n,
        total => $"Total: {total}"
    );
```

Resultado:

```text
Total: 10
```

---

# 13. Casos Empresariales

### Calcular ventas totales

```csharp
decimal total =
    ventas.Aggregate(
        0m,
        (acc,v) => acc + v.Total
    );
```

### Generar cadenas dinámicas

```csharp
string nombres =
    clientes.Aggregate(
        "",
        (acc,c) => acc + c.Nombre + "; "
    );
```

### Construir reportes

```csharp
string reporte =
    registros.Aggregate(
        "",
        (acc,r) =>
            acc + r.Descripcion + "\n"
    );
```

---

# 14. Diferencias con Otros Operadores

| Operador | Función |
|-----------|-----------|
| Count | Cuenta |
| Sum | Suma |
| Average | Promedio |
| Min | Menor valor |
| Max | Mayor valor |
| Aggregate | Acumulación personalizada |

---

# 15. Ventajas

- Extremadamente flexible.
- Permite lógica personalizada.
- Puede reemplazar múltiples ciclos `foreach`.
- Ideal para transformaciones complejas.
- Muy expresivo en programación funcional.

---

# 16. Consideraciones

Si la colección está vacía:

```csharp
numeros.Aggregate(
    (a,b) => a + b
);
```

Se produce:

```text
InvalidOperationException
```

Para evitarlo:

```csharp
numeros.Aggregate(
    0,
    (a,b) => a + b
);
```

---

# 17. Comparación con foreach

## foreach

```csharp
int total = 0;

foreach(var numero in numeros)
{
    total += numero;
}
```

## Aggregate

```csharp
int total =
    numeros.Aggregate(
        (a,b) => a + b
    );
```

Ambos producen:

```text
10
```

---

# 18. Resumen

`Aggregate()` es el operador de agregación más avanzado de LINQ.

Características principales:

- Permite acumulaciones personalizadas.
- Puede sumar, multiplicar, concatenar y transformar datos.
- Utiliza un acumulador interno.
- Compatible principalmente con IEnumerable.
- Base conceptual de muchos operadores de agregación.
- Muy utilizado en programación funcional y transformaciones complejas.
