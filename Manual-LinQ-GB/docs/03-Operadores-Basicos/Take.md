# Take

## 1. Introducción

`Take()` es un operador de LINQ que permite obtener una cantidad específica de elementos desde el inicio de una secuencia.

Se utiliza frecuentemente para:

- Limitar resultados
- Implementar paginación
- Obtener los primeros registros
- Mostrar vistas previas de datos
- Optimizar consultas

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

```csharp
Take(int count)
```

Donde:

- `count` = Cantidad de elementos a recuperar.

---

# 3. ¿Cómo Funciona?

Colección:

```csharp
List<int> numeros =
[
    1,
    2,
    3,
    4,
    5
];
```

Consulta:

```csharp
var resultado =
    numeros.Take(3);
```

Proceso:

```text
1
2
3
4
5

Tomar 3
    ↓

1
2
3
```

---

# 4. Arquitectura Interna

```text
Colección
     │
     ▼
 Take()
     │
     ▼
Tomar N elementos
     │
     ▼
Resultado
```

---

# 5. Ejemplo Básico

```csharp
List<int> numeros =
[
    10,
    20,
    30,
    40,
    50
];

var resultado =
    numeros.Take(2);

foreach(var item in resultado)
{
    Console.WriteLine(item);
}
```

Salida:

```text
10
20
```

---

# 6. Take con Strings

```csharp
List<string> nombres =
[
    "Ana",
    "Carlos",
    "Gabriel",
    "Pedro"
];

var resultado =
    nombres.Take(2);
```

Salida:

```text
Ana
Carlos
```

---

# 7. Take con Objetos

Entidad:

```csharp
public class Cliente
{
    public int Id { get; set; }

    public string Nombre { get; set; }
}
```

Consulta:

```csharp
var resultado =
    clientes.Take(5);
```

Resultado:

```text
Obtiene los primeros 5 clientes
```

---

# 8. Deferred Execution

Take utiliza ejecución diferida.

```csharp
var consulta =
    numeros.Take(3);
```

En este momento:

```text
Todavía no se ejecuta
```

La ejecución ocurre cuando:

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

# 9. Take en IEnumerable

```csharp
IEnumerable<int> resultado =
    numeros.Take(3);
```

La operación ocurre en memoria RAM.

---

# 10. Take en IQueryable

```csharp
var resultado =
    contexto.Clientes
            .Take(10);
```

SQL generado:

```sql
SELECT TOP(10) *
FROM Clientes;
```

En SQL Server.

---

# 11. Combinación con Skip

La combinación más utilizada.

```csharp
var resultado =
    numeros
        .Skip(2)
        .Take(3);
```

Proceso:

```text
1
2
3
4
5
6
7

Skip(2)
     ↓

3
4
5
6
7

Take(3)
     ↓

3
4
5
```

---

# 12. Implementación de Paginación

Supongamos:

```text
Página = 2

Tamaño = 10
```

Consulta:

```csharp
int pagina = 2;
int tamanio = 10;

var resultado =
    clientes
        .Skip(
            (pagina - 1) * tamanio
        )
        .Take(tamanio);
```

Resultado:

```text
Registros 11 al 20
```

---

# 13. Fórmula de Paginación

```text
Skip = (Página - 1) × Tamaño

Take = Tamaño
```

Ejemplo:

| Página | Tamaño | Skip | Take |
|----------|----------|----------|----------|
| 1 | 10 | 0 | 10 |
| 2 | 10 | 10 | 10 |
| 3 | 10 | 20 | 10 |
| 4 | 10 | 30 | 10 |

---

# 14. Combinación con Where

```csharp
var resultado =
    clientes
        .Where(c => c.Activo)
        .Take(10);
```

Proceso:

```text
Where
   ↓
Take
   ↓
Resultado
```

---

# 15. Combinación con OrderBy

```csharp
var resultado =
    productos
        .OrderBy(p => p.Precio)
        .Take(5);
```

Resultado:

```text
Los 5 productos más baratos
```

---

# 16. Top N Registros

Caso muy común.

```csharp
var topVentas =
    ventas
        .OrderByDescending(v => v.Total)
        .Take(10);
```

Resultado:

```text
Top 10 ventas
```

SQL aproximado:

```sql
SELECT TOP(10) *
FROM Ventas
ORDER BY Total DESC;
```

---

# 17. Caso Empresarial

Entidad:

```csharp
public class Producto
{
    public string Nombre { get; set; }

    public decimal Precio { get; set; }
}
```

Consulta:

```csharp
var destacados =
    contexto.Productos
            .OrderByDescending(p => p.Precio)
            .Take(5);
```

Uso típico:

```text
Productos destacados
Top ventas
Ranking
Dashboard
```

---

# 18. Ventajas

## Limita Datos

Reduce la cantidad de registros recuperados.

---

## Mejor Rendimiento

Menor uso de memoria y red.

---

## Compatible con SQL

Se traduce a TOP o LIMIT.

---

## Fundamental para Paginación

Trabaja junto con Skip.

---

# 19. Errores Comunes

Incorrecto:

```csharp
contexto.Clientes
        .ToList()
        .Take(10);
```

Problema:

```text
Carga todos los registros antes de limitar
```

---

Correcto:

```csharp
contexto.Clientes
        .Take(10)
        .ToList();
```

---

# 20. Diferencia Entre Skip y Take

| Método | Función |
|----------|----------|
| Skip | Omite elementos |
| Take | Obtiene elementos |

Ejemplo:

```csharp
.Skip(10)
```

↓

```text
Ignora los primeros 10
```

Ejemplo:

```csharp
.Take(10)
```

↓

```text
Obtiene los primeros 10
```

---

# 21. TakeWhile

Existe una variante llamada:

```csharp
TakeWhile()
```

Ejemplo:

```csharp
var resultado =
    numeros.TakeWhile(
        n => n < 5
    );
```

Resultado:

```text
Toma elementos mientras la condición sea verdadera
```

---

# 22. Flujo Completo

```text
Colección
      │
      ▼
Take()
      │
      ▼
Tomar N Elementos
      │
      ▼
Resultado
```

---

# 23. Resumen

`Take()` permite obtener una cantidad específica de elementos desde el inicio de una secuencia.

Características principales:

- Limita resultados.
- Implementa Deferred Execution.
- Funciona con IEnumerable e IQueryable.
- Se traduce eficientemente a SQL.
- Es esencial para la paginación.
- Se utiliza frecuentemente con Skip().
- Mejora el rendimiento al reducir la cantidad de datos recuperados.

En aplicaciones empresariales modernas, `Take()` es utilizado constantemente para construir dashboards, rankings, reportes, APIs REST paginadas y consultas optimizadas sobre grandes volúmenes de información.
