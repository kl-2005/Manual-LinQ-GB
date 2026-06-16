# Max

## 1. Introducción

`Max()` es un operador de agregación de LINQ que permite obtener el valor más grande de una secuencia.

Es equivalente a la función SQL:

```sql
MAX()
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

## Valor máximo de una colección

```csharp
Max()
```

## Valor máximo mediante selector

```csharp
Max(
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
int maximo =
    numeros.Max();
```

Resultado:

```text
40
```

---

# 4. Arquitectura Interna

```text
Colección
      │
      ▼
    Max()
      │
      ▼
Comparar Valores
      │
      ▼
Encontrar Mayor
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

int edadMaxima =
    edades.Max();

Console.WriteLine(edadMaxima);
```

Resultado:

```text
40
```

---

# 6. Max con Objetos

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
decimal precioMaximo =
    productos.Max(
        p => p.Precio
    );
```

---

# 7. Max con Fechas

```csharp
DateTime fechaMasReciente =
    ventas.Max(
        v => v.Fecha
    );
```

Resultado:

```text
Fecha más reciente registrada
```

---

# 8. Max con Where

```csharp
decimal mayorPrecio =
    productos
        .Where(
            p => p.Stock > 0
        )
        .Max(
            p => p.Precio
        );
```

---

# 9. Entity Framework

Consulta:

```csharp
decimal maximo =
    contexto.Productos
        .Max(
            p => p.Precio
        );
```

SQL aproximado:

```sql
SELECT MAX(Precio)
FROM Productos;
```

---

# 10. Caso Empresarial

Producto más caro:

```csharp
decimal precioMaximo =
    contexto.Productos
        .Max(
            p => p.Precio
        );
```

Salario más alto:

```csharp
decimal salarioMaximo =
    contexto.Empleados
        .Max(
            e => e.Salario
        );
```

Venta más grande:

```csharp
decimal ventaMaxima =
    contexto.Ventas
        .Max(
            v => v.Total
        );
```

---

# 11. Tipos Compatibles

Max puede utilizarse con:

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
var maximo =
    numeros.Max();
```

Se produce:

```text
InvalidOperationException
```

Por ello suele validarse:

```csharp
if(numeros.Any())
{
    var maximo =
        numeros.Max();
}
```

---

# 14. Rendimiento

- Muy eficiente.
- Compatible con SQL.
- Optimizado por LINQ.
- Ideal para reportes y métricas.

---

# 15. Ejemplos Reales

Mayor edad:

```csharp
int mayorEdad =
    personas.Max(
        p => p.Edad
    );
```

Nota más alta:

```csharp
double notaMaxima =
    calificaciones.Max();
```

Producto más costoso:

```csharp
decimal mayorPrecio =
    productos.Max(
        p => p.Precio
    );
```

Última fecha registrada:

```csharp
DateTime ultimaVenta =
    ventas.Max(
        v => v.Fecha
    );
```

---

# 16. Resumen

`Max()` devuelve el valor más grande de una secuencia.

Características:

- Equivalente a MAX de SQL.
- Compatible con Entity Framework.
- Soporta números, fechas y cadenas.
- Ejecución inmediata.
- Muy utilizado en reportes, análisis y dashboards empresariales.
