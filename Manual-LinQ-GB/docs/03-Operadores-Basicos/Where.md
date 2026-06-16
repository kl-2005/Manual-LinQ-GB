# Where

## 1. Introducción

`Where()` es uno de los operadores más importantes y utilizados de LINQ.

Su función es filtrar elementos de una secuencia según una condición determinada.

Pertenece al namespace:

```csharp
System.Linq
```

Permite seleccionar únicamente los elementos que cumplen una condición específica.

---

# 2. Sintaxis

```csharp
Where(Func<T,bool> predicate)
```

Donde:

* `T` representa el tipo de dato.
* `predicate` representa una condición booleana.

---

# 3. ¿Cómo Funciona?

Supongamos una colección:

```csharp
List<int> numeros =
[
    5,
    10,
    15,
    20,
    25
];
```

Consulta:

```csharp
var resultado =
    numeros.Where(
        n => n > 10
    );
```

Proceso:

```text
5   → False
10  → False
15  → True
20  → True
25  → True
```

Resultado:

```text
15
20
25
```

---

# 4. Arquitectura Interna

```text
Colección
    │
    ▼
Where()
    │
    ▼
Condición
    │
    ▼
Elementos Filtrados
```

---

# 5. Ejemplo Básico

```csharp
List<int> numeros =
[
    1,
    2,
    3,
    4,
    5
];

var pares =
    numeros.Where(
        n => n % 2 == 0
    );

foreach(var numero in pares)
{
    Console.WriteLine(numero);
}
```

Salida:

```text
2
4
```

---

# 6. Where con Objetos

Entidad:

```csharp
public class Cliente
{
    public string Nombre { get; set; }

    public bool Activo { get; set; }
}
```

Consulta:

```csharp
var clientesActivos =
    clientes.Where(
        c => c.Activo
    );
```

Resultado:

```text
Solo clientes activos
```

---

# 7. Where con Múltiples Condiciones

```csharp
var resultado =
    productos.Where(
        p => p.Activo &&
             p.Stock > 0
    );
```

Equivalente SQL:

```sql
WHERE Activo = 1
AND Stock > 0
```

---

# 8. Uso con Strings

```csharp
var nombres =
    personas.Where(
        p => p.Nombre.StartsWith("A")
    );
```

Resultado:

```text
Ana
Andrés
Alberto
```

---

# 9. Deferred Execution

Where utiliza ejecución diferida.

```csharp
var consulta =
    numeros.Where(
        n => n > 10
    );
```

En este punto:

```text
No se ejecuta todavía
```

La ejecución ocurre cuando:

```csharp
consulta.ToList();
```

o

```csharp
foreach(var n in consulta)
{
}
```

---

# 10. Where en IEnumerable

```csharp
IEnumerable<int> resultado =
    numeros.Where(
        n => n > 10
    );
```

El filtrado ocurre en memoria RAM.

---

# 11. Where en IQueryable

```csharp
var consulta =
    contexto.Clientes
            .Where(
                c => c.Activo
            );
```

Entity Framework genera:

```sql
SELECT *
FROM Clientes
WHERE Activo = 1;
```

---

# 12. Combinación con Select

```csharp
var nombres =
    clientes
        .Where(c => c.Activo)
        .Select(c => c.Nombre);
```

Proceso:

```text
Filtrar
    ↓
Transformar
```

---

# 13. Combinación con OrderBy

```csharp
var resultado =
    clientes
        .Where(c => c.Activo)
        .OrderBy(c => c.Nombre);
```

Proceso:

```text
Where
   ↓
OrderBy
   ↓
Resultado
```

---

# 14. Caso Empresarial

Entidad:

```csharp
public class Producto
{
    public string Nombre { get; set; }

    public decimal Precio { get; set; }

    public bool Activo { get; set; }
}
```

Consulta:

```csharp
var productos =
    contexto.Productos
            .Where(
                p => p.Activo &&
                     p.Precio > 100
            );
```

SQL aproximado:

```sql
SELECT *
FROM Productos
WHERE Activo = 1
AND Precio > 100;
```

---

# 15. Ventajas

## Código Legible

Permite expresar filtros de forma natural.

---

## Integración con LINQ

Funciona con todos los operadores LINQ.

---

## Compatible con EF Core

Puede traducirse a SQL.

---

## Deferred Execution

Optimiza recursos.

---

# 16. Errores Comunes

Incorrecto:

```csharp
var clientes =
    contexto.Clientes
            .ToList()
            .Where(c => c.Activo);
```

Problema:

```text
Carga todos los registros
```

---

Correcto:

```csharp
var clientes =
    contexto.Clientes
            .Where(c => c.Activo)
            .ToList();
```

---

# 17. Flujo Completo

```text
Colección
      │
      ▼
Where()
      │
      ▼
Condición
      │
      ▼
Elementos Válidos
      │
      ▼
Resultado
```

---

# 18. Resumen

`Where()` es el operador de filtrado principal de LINQ.

Características principales:

* Filtra elementos según una condición.
* Utiliza delegates y lambdas.
* Implementa Deferred Execution.
* Funciona con IEnumerable e IQueryable.
* Puede traducirse a SQL mediante Entity Framework Core.
* Es uno de los operadores más utilizados en aplicaciones empresariales.

Prácticamente toda consulta LINQ comienza con un `Where()` para limitar la cantidad de datos que serán procesados.
