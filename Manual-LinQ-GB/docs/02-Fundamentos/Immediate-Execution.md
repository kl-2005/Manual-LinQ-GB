# Immediate Execution (Ejecución Inmediata)

La **Ejecución Inmediata (Immediate Execution)** es el proceso mediante el cual una consulta LINQ se ejecuta en el momento exacto en que se invoca un método terminal que necesita obtener resultados.

A diferencia de la **Ejecución Diferida (Deferred Execution)**, donde la consulta solo se construye y espera hasta ser utilizada, la Ejecución Inmediata obliga a LINQ a recorrer la secuencia y producir un resultado inmediatamente.

---

# 1. ¿Qué es Immediate Execution?

Cuando escribimos una consulta LINQ:

```csharp
var consulta =
    numeros.Where(n => n > 10);
```

Todavía no ocurre ninguna ejecución.

La consulta simplemente queda almacenada.

```text
Consulta creada
↓
No ejecutada aún
```

Sin embargo, cuando usamos:

```csharp
var resultado = consulta.ToList();
```

LINQ ejecuta inmediatamente toda la consulta.

```text
Consulta creada
↓
Consulta ejecutada
↓
Resultados obtenidos
```

---

# 2. ¿Por Qué Existe?

LINQ utiliza por defecto **Deferred Execution** para optimizar recursos.

Sin embargo, existen situaciones donde necesitamos:

* Obtener resultados inmediatamente.
* Almacenar una copia de los datos.
* Evitar múltiples recorridos.
* Congelar el estado actual de una colección.

Para ello se utilizan métodos de Ejecución Inmediata.

---

# 3. Diferencia Entre Deferred e Immediate Execution

## Deferred Execution

```csharp
var consulta =
    numeros.Where(x => x > 10);
```

Resultado:

```text
No se ejecuta todavía
```

---

## Immediate Execution

```csharp
var lista =
    numeros.Where(x => x > 10)
           .ToList();
```

Resultado:

```text
La consulta se ejecuta inmediatamente
```

---

# 4. Ejemplo Básico

Colección:

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
var consulta =
    numeros.Where(n => n > 10);
```

Aún no ocurre nada.

Ahora:

```csharp
var resultado =
    consulta.ToList();
```

LINQ ejecuta:

```text
15
20
25
```

y almacena los resultados en memoria.

---

# 5. Métodos que Provocan Immediate Execution

## Materialización

Estos métodos ejecutan la consulta y generan una colección.

### ToList()

```csharp
var lista = consulta.ToList();
```

Retorna:

```csharp
List<T>
```

---

### ToArray()

```csharp
var arreglo = consulta.ToArray();
```

Retorna:

```csharp
T[]
```

---

### ToDictionary()

```csharp
var diccionario =
    clientes.ToDictionary(
        c => c.Id
    );
```

Retorna:

```csharp
Dictionary<TKey,TValue>
```

---

### ToLookup()

```csharp
var lookup =
    clientes.ToLookup(
        c => c.Ciudad
    );
```

---

# 6. Métodos de Agregación

También ejecutan inmediatamente porque necesitan recorrer toda la colección.

---

## Count()

```csharp
int total =
    consulta.Count();
```

Resultado:

```text
Cantidad de elementos
```

---

## Sum()

```csharp
decimal total =
    ventas.Sum(v => v.Total);
```

---

## Average()

```csharp
double promedio =
    notas.Average();
```

---

## Max()

```csharp
var maximo =
    ventas.Max(v => v.Total);
```

---

## Min()

```csharp
var minimo =
    ventas.Min(v => v.Total);
```

---

# 7. Métodos de Selección Única

Necesitan ejecutar la consulta para devolver un valor.

---

## First()

```csharp
var cliente =
    clientes.First();
```

Retorna:

```text
Primer elemento
```

---

## FirstOrDefault()

```csharp
var cliente =
    clientes.FirstOrDefault();
```

---

## Single()

```csharp
var cliente =
    clientes.Single();
```

Debe existir exactamente un registro.

---

## SingleOrDefault()

```csharp
var cliente =
    clientes.SingleOrDefault();
```

---

## Last()

```csharp
var ultimo =
    clientes.Last();
```

---

# 8. Ejemplo Visual

Consulta:

```csharp
var consulta =
    numeros
        .Where(x => x > 10)
        .OrderBy(x => x);
```

Arquitectura:

```text
Where()
     ↓
OrderBy()
     ↓
Pendiente
```

---

Cuando ejecutamos:

```csharp
consulta.ToList();
```

Ocurre:

```text
Where()
     ↓
OrderBy()
     ↓
ToList()
     ↓
Ejecución inmediata
```

---

# 9. Immediate Execution en Entity Framework

Supongamos:

```csharp
var consulta =
    contexto.Clientes
            .Where(c => c.Activo);
```

Tipo:

```csharp
IQueryable<Cliente>
```

Todavía no existe SQL ejecutado.

---

Cuando hacemos:

```csharp
var lista =
    consulta.ToList();
```

EF Core genera SQL:

```sql
SELECT *
FROM Clientes
WHERE Activo = 1;
```

y ejecuta inmediatamente la consulta.

---

# 10. Congelación de Datos

Una ventaja importante es que los resultados quedan almacenados.

Ejemplo:

```csharp
var lista =
    numeros.Where(n => n > 10)
           .ToList();
```

Luego:

```csharp
numeros.Add(100);
```

La lista obtenida anteriormente NO cambia.

Porque ya fue materializada.

---

# 11. Deferred Execution vs Immediate Execution

## Deferred

```csharp
var consulta =
    numeros.Where(x => x > 10);
```

Cada recorrido vuelve a ejecutarse.

```csharp
consulta.Count();
consulta.Count();
```

Puede recorrer nuevamente la colección.

---

## Immediate

```csharp
var lista =
    numeros.Where(x => x > 10)
           .ToList();
```

Ahora:

```csharp
lista.Count
lista.Count
```

No vuelve a ejecutar la consulta.

---

# 12. Caso Empresarial

Supongamos una aplicación de ventas.

Consulta:

```csharp
var productos =
    contexto.Productos
            .Where(p => p.Activo);
```

Si usamos:

```csharp
foreach(var p in productos)
{
}
```

La consulta puede ejecutarse múltiples veces.

---

Mejor:

```csharp
var listaProductos =
    productos.ToList();
```

Ahora los datos están cargados una sola vez.

---

# 13. Ventajas

## Evita Reejecuciones

La consulta se ejecuta una única vez.

---

## Mejora el Rendimiento

Cuando los datos serán reutilizados varias veces.

---

## Congela el Estado Actual

Los cambios posteriores en la fuente no afectan los resultados.

---

## Facilita el Trabajo en Memoria

Permite trabajar con listas y arreglos locales.

---

# 14. Desventajas

## Mayor Consumo de Memoria

Todos los resultados se almacenan.

---

## Puede Cargar Datos Innecesarios

Si se materializa demasiado pronto.

Ejemplo incorrecto:

```csharp
var clientes =
    contexto.Clientes
            .ToList();

var activos =
    clientes.Where(c => c.Activo);
```

Aquí primero se cargan todos los registros.

---

# 15. Buenas Prácticas

### Correcto

Filtrar antes de materializar.

```csharp
var activos =
    contexto.Clientes
            .Where(c => c.Activo)
            .ToList();
```

---

### Incorrecto

Materializar antes de filtrar.

```csharp
var activos =
    contexto.Clientes
            .ToList()
            .Where(c => c.Activo);
```

---

# 16. Flujo Completo

```text
LINQ Query
      │
      ▼
Where()
      │
      ▼
OrderBy()
      │
      ▼
ToList()
      │
      ▼
Immediate Execution
      │
      ▼
Resultados Materializados
      │
      ▼
List<T>
```

---

# 17. Resumen

La **Immediate Execution** ocurre cuando una consulta LINQ es forzada a ejecutarse inmediatamente mediante un método terminal.

Características principales:

* Ejecuta la consulta en el momento.
* Materializa resultados en memoria.
* Es utilizada por `ToList()`, `ToArray()`, `Count()`, `Sum()`, `First()`, entre otros.
* Congela el estado actual de los datos.
* Evita reejecuciones innecesarias.
* Es fundamental en Entity Framework Core para lanzar consultas SQL.
* Debe utilizarse después de aplicar filtros y ordenamientos.

Comprender la diferencia entre **Deferred Execution** e **Immediate Execution** es esencial para escribir consultas LINQ eficientes y optimizar el rendimiento de aplicaciones empresariales.
