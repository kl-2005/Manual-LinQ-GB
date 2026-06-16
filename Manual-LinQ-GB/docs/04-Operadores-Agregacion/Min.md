# Average

## 1. Introducción

`Average()` es un operador de agregación de LINQ que permite calcular el promedio aritmético de una secuencia de valores numéricos.

Es equivalente a la función SQL:

```sql
AVG()
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

## Promedio directo

```csharp
Average()
```

## Promedio mediante selector

```csharp
Average(
    Func<TSource,TResult> selector
)
```

---

# 3. Fórmula Matemática

```text
Promedio = Suma de Valores / Cantidad de Valores
```

Ejemplo:

```text
10 + 20 + 30 + 40 = 100

100 / 4 = 25
```

Resultado:

```text
25
```

---

# 4. Ejemplo Básico

```csharp
List<int> numeros =
[
    10,
    20,
    30,
    40
];

double promedio =
    numeros.Average();

Console.WriteLine(promedio);
```

Resultado:

```text
25
```

---

# 5. Arquitectura Interna

```text
Colección
      │
      ▼
 Average()
      │
      ▼
Calcular Suma
      │
      ▼
Contar Elementos
      │
      ▼
Dividir
      │
      ▼
Resultado
```

---

# 6. Average con Objetos

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
decimal promedio =
    productos.Average(
        p => p.Precio
    );
```

---

# 7. Average con Decimales

```csharp
List<decimal> salarios =
[
    1200m,
    1500m,
    1800m,
    2000m
];

decimal promedio =
    salarios.Average();
```

Resultado:

```text
1625
```

---

# 8. Average con Where

```csharp
decimal promedio =
    productos
        .Where(
            p => p.Stock > 0
        )
        .Average(
            p => p.Precio
        );
```

---

# 9. Entity Framework

Consulta:

```csharp
decimal promedio =
    contexto.Ventas
        .Average(
            v => v.Total
        );
```

SQL aproximado:

```sql
SELECT AVG(Total)
FROM Ventas;
```

---

# 10. Caso Empresarial

Promedio de ventas:

```csharp
decimal promedioVentas =
    contexto.Ventas
        .Average(
            v => v.Total
        );
```

Promedio de salarios:

```csharp
decimal promedioSalarios =
    contexto.Empleados
        .Average(
            e => e.Salario
        );
```

---

# 11. Tipos Compatibles

Average soporta:

```csharp
int
long
float
double
decimal
```

---

# 12. Diferencia entre Sum y Average

## Sum

```csharp
numeros.Sum();
```

Resultado:

```text
100
```

---

## Average

```csharp
numeros.Average();
```

Resultado:

```text
25
```

---

# 13. Consideraciones

Si la colección está vacía:

```csharp
var promedio =
    numeros.Average();
```

Se genera una excepción:

```text
InvalidOperationException
```

Por ello suele validarse previamente:

```csharp
if(numeros.Any())
{
    var promedio =
        numeros.Average();
}
```

---

# 14. Rendimiento

- Muy eficiente.
- Compatible con SQL.
- Optimizado por LINQ.
- Ideal para estadísticas y reportes.

---

# 15. Ejemplos Reales

Promedio de calificaciones:

```csharp
double promedio =
    notas.Average();
```

Promedio de edad:

```csharp
double promedioEdad =
    personas.Average(
        p => p.Edad
    );
```

Promedio de ventas mensuales:

```csharp
decimal promedioMensual =
    ventas.Average(
        v => v.Total
    );
```

---

# 16. Resumen

`Average()` calcula el promedio de una secuencia numérica.

Características:

- Equivalente a AVG de SQL.
- Compatible con Entity Framework.
- Utiliza suma y conteo internamente.
- Ejecución inmediata.
- Muy utilizado en análisis estadístico, dashboards y reportes empresariales.
