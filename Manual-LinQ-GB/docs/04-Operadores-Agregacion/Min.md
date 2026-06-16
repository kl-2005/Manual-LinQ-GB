# Min

## 1. Introducción

`Min()` es un operador de agregación de LINQ que permite obtener el valor más pequeño de una secuencia.

Es equivalente a la función SQL:

```sql
MIN()
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

## Valor mínimo de una colección

```csharp
Min()
```

## Valor mínimo mediante selector

```csharp
Min(
    Func<TSource,TResult> selector
)
```

---

# 3. ¿Cómo Funciona?

Colección:

```csharp
List<int> numeros =
[
    25,
    10,
    40,
    5,
    30
];
```

Consulta:

```csharp
int minimo =
    numeros.Min();
```

Resultado:

```text
5
```

---

# 4. Arquitectura Interna

```text
Colección
      │
      ▼
    Min()
      │
      ▼
Comparar Valores
      │
      ▼
Encontrar Menor
      │
      ▼
Resultado
```

---

# 5. Ejemplo Básico

```csharp
int[] edades =
{
    18,
    25,
    32,
    15,
    40
};

int edadMinima =
    edades.Min();

Console.WriteLine(edadMinima);
```

Resultado:

```text
15
```

---

# 6. Min con Objetos

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
decimal precioMinimo =
    productos.Min(
        p => p.Precio
    );
```

---

# 7. Min con Fechas

```csharp
DateTime fechaMasAntigua =
    ventas.Min(
        v => v.Fecha
    );
```

Resultado:

```text
Fecha más antigua registrada
```

---

# 8. Min con Where

```csharp
decimal menorPrecio =
    productos
        .Where(
            p => p.Stock > 0
        )
        .Min(
            p => p.Precio
        );
```

---

# 9. Entity Framework

Consulta:

```csharp
decimal minimo =
    contexto.Productos
        .Min(
            p => p.Precio
        );
```

SQL aproximado:

```sql
SELECT MIN(Precio)
FROM Productos;
```

---

# 10. Caso Empresarial

Precio más bajo:

```csharp
decimal precioMinimo =
    contexto.Productos
        .Min(
            p => p.Precio
        );
```

Salario mínimo:

```csharp
decimal salarioMinimo =
    contexto.Empleados
        .Min(
            e => e.Salario
        );
```

Venta más pequeña:

```csharp
decimal ventaMinima =
    contexto.Ventas
        .Min(
            v => v.Total
        );
```

---

# 11. Tipos Compatibles

Min puede utilizarse con:

```csharp
int
long
float
double
decimal
DateTime
string
```

---

# 12. Diferencia entre Min y Max

## Min

```csharp
numeros.Min();
```

Resultado:

```text
5
```

---

## Max

```csharp
numeros.Max();
```

Resultado:

```text
40
```

---

# 13. Consideraciones

Si la colección está vacía:

```csharp
var minimo =
    numeros.Min();
```

Se produce:

```text
InvalidOperationException
```

Por ello suele validarse:

```csharp
if(numeros.Any())
{
    var minimo =
        numeros.Min();
}
```

---

# 14. Rendimiento

- Muy eficiente.
- Compatible con SQL.
- Optimizado por LINQ.
- Ideal para estadísticas.

---

# 15. Ejemplos Reales

Edad mínima:

```csharp
int menorEdad =
    personas.Min(
        p => p.Edad
    );
```

Nota más baja:

```csharp
double notaMinima =
    calificaciones.Min();
```

Precio más económico:

```csharp
decimal menorPrecio =
    productos.Min(
        p => p.Precio
    );
```

---

# 16. Resumen

`Min()` devuelve el valor más pequeño de una secuencia.

Características:

- Equivalente a MIN de SQL.
- Compatible con Entity Framework.
- Soporta tipos numéricos, fechas y cadenas.
- Ejecución inmediata.
- Muy utilizado en reportes, análisis y métricas empresariales.
