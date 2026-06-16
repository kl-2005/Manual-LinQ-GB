# Last

## 1. Introducción

`Last()` es un operador de elementos de LINQ que permite obtener el último elemento de una secuencia.

Pertenece al namespace:

```csharp
System.Linq
```

Es el equivalente de `First()`, pero recuperando el último elemento disponible.

---

# 2. ¿Qué Hace Last?

Devuelve:

```text
El último elemento de la secuencia
```

Si la secuencia está vacía:

```text
Lanza una excepción
```

---

# 3. Sintaxis

## Obtener último elemento

```csharp
Last()
```

## Obtener último elemento que cumpla una condición

```csharp
Last(
    Func<TSource,bool> predicate
)
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

int ultimo =
    numeros.Last();

Console.WriteLine(ultimo);
```

Resultado:

```text
40
```

---

# 5. Arquitectura Interna

```text
Colección
      │
      ▼
    Last()
      │
      ▼
Buscar Último
      │
      ▼
Resultado
```

---

# 6. Last con Condición

```csharp
int resultado =
    numeros.Last(
        n => n > 15
    );
```

Proceso:

```text
20
30
40
```

Resultado:

```text
40
```

---

# 7. Last con Objetos

Clase:

```csharp
public class Venta
{
    public int Id { get; set; }
    public decimal Total { get; set; }
}
```

Consulta:

```csharp
Venta ultimaVenta =
    ventas.Last();
```

---

# 8. Colección Vacía

```csharp
List<int> numeros = [];

var resultado =
    numeros.Last();
```

Resultado:

```text
InvalidOperationException
```

Mensaje:

```text
Sequence contains no elements
```

---

# 9. Sin Coincidencias

```csharp
var resultado =
    numeros.Last(
        n => n > 100
    );
```

Resultado:

```text
InvalidOperationException
```

Mensaje:

```text
Sequence contains no matching element
```

---

# 10. Entity Framework

Consulta:

```csharp
var venta =
    contexto.Ventas
        .OrderBy(v => v.Id)
        .Last();
```

SQL aproximado:

```sql
SELECT TOP(1) *
FROM Ventas
ORDER BY Id DESC
```

---

# 11. Importancia del OrderBy

Cuando se utiliza Entity Framework es recomendable ordenar previamente.

Ejemplo:

```csharp
var venta =
    contexto.Ventas
        .OrderBy(
            v => v.Fecha
        )
        .Last();
```

De esta forma obtenemos:

```text
La venta más reciente
```

---

# 12. Diferencia entre First y Last

## First

```csharp
numeros.First();
```

Resultado:

```text
10
```

---

## Last

```csharp
numeros.Last();
```

Resultado:

```text
40
```

---

# 13. Caso Empresarial

Última venta:

```csharp
var venta =
    ventas.Last();
```

Último cliente registrado:

```csharp
var cliente =
    clientes.Last();
```

Última factura generada:

```csharp
var factura =
    facturas.Last();
```

---

# 14. Rendimiento

En listas:

```csharp
List<T>
```

es muy rápido.

En secuencias complejas:

```csharp
IEnumerable<T>
```

puede requerir recorrer toda la colección.

---

# 15. Ejemplos Reales

Último pedido:

```csharp
var pedido =
    pedidos.Last();
```

Último estudiante matriculado:

```csharp
var estudiante =
    estudiantes.Last();
```

Último registro del sistema:

```csharp
var log =
    logs.Last();
```

---

# 16. Resumen

`Last()` devuelve el último elemento de una secuencia.

Características:

- Devuelve un único elemento.
- Puede utilizar condiciones.
- Compatible con Entity Framework.
- Ejecución inmediata.
- Lanza excepción si no encuentra resultados.
- Es el complemento natural de First().
