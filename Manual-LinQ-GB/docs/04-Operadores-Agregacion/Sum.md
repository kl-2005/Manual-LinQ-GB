# Sum

## 1. Introducción

`Sum()` es un operador de agregación de LINQ que permite calcular la suma total de los valores de una secuencia.

Es equivalente a la función SQL:

```sql
SUM()
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

## Suma directa

```csharp
Sum()
```

## Suma mediante selector

```csharp
Sum(
    Func<TSource,TResult> selector
)
```

---

# 3. ¿Cómo Funciona?

Colección:

```csharp
List<int> numeros =
[
    10,
    20,
    30,
    40
];
```

Consulta:

```csharp
int total =
    numeros.Sum();
```

Resultado:

```text
100
```

---

# 4. Arquitectura Interna

```text
Colección
      │
      ▼
    Sum()
      │
      ▼
Recorrer Elementos
      │
      ▼
Acumular Valores
      │
      ▼
Resultado Final
```

---

# 5. Ejemplo Básico

```csharp
int[] numeros =
{
    5,
    10,
    15,
    20
};

int total =
    numeros.Sum();

Console.WriteLine(total);
```

Resultado:

```text
50
```

---

# 6. Sum con Objetos

Clase:

```csharp
public class Producto
{
    public string Nombre { get; set; }
    public decimal Precio { get; set; }
}
```

Consulta:

```csharp
decimal total =
    productos.Sum(
        p => p.Precio
    );
```

Resultado:

```text
Suma total de precios
```

---

# 7. Sum con Decimales

```csharp
List<decimal> precios =
[
    12.50m,
    8.25m,
    15.00m
];

decimal total =
    precios.Sum();
```

Resultado:

```text
35.75
```

---

# 8. Sum con Where

```csharp
decimal total =
    productos
        .Where(
            p => p.Stock > 0
        )
        .Sum(
            p => p.Precio
        );
```

---

# 9. Sum con Entity Framework

```csharp
decimal ventas =
    contexto.Ventas
        .Sum(
            v => v.Total
        );
```

SQL aproximado:

```sql
SELECT SUM(Total)
FROM Ventas;
```

---

# 10. Ejemplo Empresarial

```csharp
decimal ingresosMes =
    contexto.Ventas
        .Where(
            v => v.Fecha.Month ==
                 DateTime.Now.Month
        )
        .Sum(
            v => v.Total
        );
```

---

# 11. Tipos Compatibles

Sum puede trabajar con:

```csharp
int
long
float
double
decimal
```

---

# 12. Diferencia con Aggregate

## Sum

```csharp
numeros.Sum()
```

Suma automáticamente.

---

## Aggregate

```csharp
numeros.Aggregate(
    (a,b) => a + b
);
```

Permite lógica personalizada.

---

# 13. Rendimiento

- Muy eficiente.
- Optimizado por LINQ.
- Traducible a SQL.
- Ideal para reportes.

---

# 14. Caso Real

Calcular ventas totales:

```csharp
decimal totalVentas =
    ventas.Sum(
        v => v.Monto
    );
```

Calcular salarios:

```csharp
decimal totalNomina =
    empleados.Sum(
        e => e.Salario
    );
```

---

# 15. Resumen

`Sum()` calcula la suma total de una secuencia.

Características:

- Equivalente a SUM de SQL.
- Compatible con Entity Framework.
- Soporta múltiples tipos numéricos.
- Ejecución inmediata.
- Muy utilizado en dashboards, reportes y sistemas financieros.
