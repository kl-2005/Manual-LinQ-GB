# OrderBy

## 1. Introducción

`OrderBy()` es el operador de LINQ utilizado para ordenar una secuencia de elementos en orden ascendente.

Es uno de los operadores más utilizados junto con:

* Where()
* Select()
* ThenBy()

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

```csharp
OrderBy(Func<TSource, TKey> keySelector)
```

Donde:

* `TSource` = Tipo original de la colección.
* `TKey` = Campo utilizado para ordenar.

---

# 3. ¿Cómo Funciona?

Colección:

```csharp
List<int> numeros =
[
    8,
    3,
    1,
    9,
    5
];
```

Consulta:

```csharp
var resultado =
    numeros.OrderBy(
        n => n
    );
```

Proceso:

```text
8
3
1
9
5
     ↓
1
3
5
8
9
```

---

# 4. Arquitectura Interna

```text
Colección
     │
     ▼
 OrderBy()
     │
     ▼
Campo Clave
     │
     ▼
Orden Ascendente
```

---

# 5. Ejemplo Básico

```csharp
List<int> numeros =
[
    20,
    5,
    12,
    1
];

var ordenados =
    numeros.OrderBy(
        n => n
    );

foreach(var numero in ordenados)
{
    Console.WriteLine(numero);
}
```

Salida:

```text
1
5
12
20
```

---

# 6. OrderBy con Strings

```csharp
List<string> nombres =
[
    "Carlos",
    "Ana",
    "Gabriel",
    "Pedro"
];

var resultado =
    nombres.OrderBy(
        n => n
    );
```

Salida:

```text
Ana
Carlos
Gabriel
Pedro
```

---

# 7. OrderBy con Objetos

Entidad:

```csharp
public class Cliente
{
    public int Id { get; set; }

    public string Nombre { get; set; }

    public int Edad { get; set; }
}
```

Consulta:

```csharp
var clientesOrdenados =
    clientes.OrderBy(
        c => c.Nombre
    );
```

Resultado:

```text
Ordenados alfabéticamente por nombre
```

---

# 8. Ordenamiento Numérico

```csharp
var resultado =
    clientes.OrderBy(
        c => c.Edad
    );
```

Proceso:

```text
18
25
32
40
50
```

---

# 9. Deferred Execution

OrderBy utiliza ejecución diferida.

```csharp
var consulta =
    clientes.OrderBy(
        c => c.Nombre
    );
```

En este punto:

```text
La consulta aún no se ejecuta
```

Se ejecuta cuando:

```csharp
consulta.ToList();
```

o

```csharp
foreach(var item in consulta)
{
}
```

---

# 10. OrderBy en IEnumerable

```csharp
IEnumerable<Cliente> resultado =
    clientes.OrderBy(
        c => c.Nombre
    );
```

El ordenamiento ocurre en memoria RAM.

---

# 11. OrderBy en IQueryable

```csharp
var resultado =
    contexto.Clientes
            .OrderBy(
                c => c.Nombre
            );
```

Entity Framework genera:

```sql
SELECT *
FROM Clientes
ORDER BY Nombre ASC;
```

---

# 12. Combinación con Where

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

# 13. Combinación con Select

```csharp
var resultado =
    clientes
        .OrderBy(c => c.Nombre)
        .Select(c => c.Nombre);
```

Proceso:

```text
OrderBy
    ↓
Select
    ↓
Resultado
```

---

# 14. OrderByDescending

Para orden descendente:

```csharp
var resultado =
    clientes.OrderByDescending(
        c => c.Edad
    );
```

Salida:

```text
50
40
32
25
18
```

SQL:

```sql
ORDER BY Edad DESC
```

---

# 15. Caso Empresarial

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
            .Where(p => p.Activo)
            .OrderBy(p => p.Precio);
```

SQL aproximado:

```sql
SELECT *
FROM Productos
WHERE Activo = 1
ORDER BY Precio ASC;
```

---

# 16. Ventajas

## Organización de Datos

Permite presentar información ordenada.

---

## Integración con LINQ

Compatible con todos los operadores.

---

## Compatible con EF Core

Se traduce directamente a SQL.

---

## Legibilidad

Expresa claramente el criterio de ordenamiento.

---

# 17. Errores Comunes

Incorrecto:

```csharp
var clientes =
    contexto.Clientes
            .ToList()
            .OrderBy(c => c.Nombre);
```

Problema:

```text
Primero carga todos los datos y luego ordena en memoria
```

---

Correcto:

```csharp
var clientes =
    contexto.Clientes
            .OrderBy(c => c.Nombre)
            .ToList();
```

---

# 18. Diferencia Entre OrderBy y OrderByDescending

| Método            | Orden       |
| ----------------- | ----------- |
| OrderBy           | Ascendente  |
| OrderByDescending | Descendente |

Ejemplo:

```csharp
.OrderBy(x => x.Nombre)
```

↓

```text
A → Z
```

Ejemplo:

```csharp
.OrderByDescending(x => x.Nombre)
```

↓

```text
Z → A
```

---

# 19. Flujo Completo

```text
Colección
      │
      ▼
OrderBy()
      │
      ▼
Campo Clave
      │
      ▼
Ordenamiento
      │
      ▼
Resultado
```

---

# 20. Resumen

`OrderBy()` permite ordenar elementos de una secuencia utilizando una propiedad o clave específica.

Características principales:

* Ordena de forma ascendente.
* Utiliza una clave de ordenamiento.
* Implementa Deferred Execution.
* Funciona con IEnumerable e IQueryable.
* Se traduce eficientemente a SQL mediante Entity Framework Core.
* Se combina frecuentemente con Where y Select.
* Puede extenderse con ThenBy para ordenamientos múltiples.

En aplicaciones empresariales es común utilizar `OrderBy()` para ordenar clientes, productos, ventas, reportes y cualquier conjunto de datos antes de mostrarlos al usuario.
