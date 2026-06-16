# IEnumerable<T>

`IEnumerable<T>` es una de las interfaces fundamentales de .NET y constituye la base sobre la que funciona LINQ. Representa una colección de elementos que puede recorrerse secuencialmente mediante iteración.

Pertenece al namespace:

```csharp
System.Collections.Generic
```

y define la capacidad de recorrer una secuencia de objetos utilizando un enumerador.

---

# 1. ¿Qué es IEnumerable?

La interfaz `IEnumerable<T>` representa una secuencia de elementos que pueden recorrerse uno a uno.

Por ejemplo:

```csharp
List<string> nombres =
[
    "Gabriel",
    "Ana",
    "Luis"
];
```

Internamente, una lista implementa:

```csharp
IEnumerable<string>
```

Por lo tanto podemos recorrerla:

```csharp
foreach(var nombre in nombres)
{
    Console.WriteLine(nombre);
}
```

Salida:

```text
Gabriel
Ana
Luis
```

---

# 2. Definición Simplificada

La interfaz contiene un único método:

```csharp
public interface IEnumerable<out T>
{
    IEnumerator<T> GetEnumerator();
}
```

Su función es devolver un objeto enumerador capaz de recorrer la colección.

---

# 3. Relación con IEnumerator

Cuando usamos:

```csharp
foreach(var item in coleccion)
{
}
```

El compilador lo transforma aproximadamente en:

```csharp
IEnumerator<string> enumerador =
    coleccion.GetEnumerator();

while(enumerador.MoveNext())
{
    string item = enumerador.Current;
}
```

---

# 4. Arquitectura Interna

```text
IEnumerable<T>
        │
        ▼
GetEnumerator()
        │
        ▼
IEnumerator<T>
        │
        ├── Current
        ├── MoveNext()
        └── Reset()
```

---

# 5. Colecciones que Implementan IEnumerable

Muchas estructuras del Framework implementan esta interfaz.

```csharp
List<T>
Array
Dictionary<TKey,TValue>
HashSet<T>
Queue<T>
Stack<T>
LinkedList<T>
```

Ejemplo:

```csharp
int[] numeros =
{
    1,2,3,4,5
};

IEnumerable<int> secuencia = numeros;
```

---

# 6. IEnumerable y LINQ

LINQ trabaja principalmente sobre:

```csharp
IEnumerable<T>
```

Ejemplo:

```csharp
List<int> numeros =
[
    5,10,15,20
];

var resultado =
    numeros.Where(x => x > 10);
```

Tipo real:

```csharp
IEnumerable<int>
```

---

# 7. Ejecución Diferida (Deferred Execution)

Una característica importante de `IEnumerable` es la ejecución diferida.

Ejemplo:

```csharp
var consulta =
    numeros.Where(x => x > 10);
```

En este momento:

```text
No se ejecuta nada.
```

La consulta se ejecuta únicamente cuando se recorre.

```csharp
foreach(var n in consulta)
{
    Console.WriteLine(n);
}
```

Salida:

```text
15
20
```

---

# 8. Ejemplo Visual

```csharp
var consulta =
    numeros
        .Where(x => x > 10)
        .OrderBy(x => x);
```

Construcción:

```text
Where()
    ↓
OrderBy()
    ↓
Resultado
```

La ejecución ocurre únicamente al enumerar.

```csharp
consulta.ToList();
consulta.ToArray();
consulta.Count();
foreach(...)
```

---

# 9. IEnumerable en Memoria

Trabaja sobre datos ya cargados en RAM.

Ejemplo:

```csharp
List<Cliente> clientes =
    ObtenerClientes();
```

Consulta:

```csharp
var activos =
    clientes.Where(c => c.Activo);
```

Todo el procesamiento ocurre en memoria.

---

# 10. IEnumerable vs IQueryable

## IEnumerable

```csharp
IEnumerable<Cliente>
```

Procesamiento:

```text
Memoria RAM
```

Utiliza:

```text
Delegados (Func)
```

---

## IQueryable

```csharp
IQueryable<Cliente>
```

Procesamiento:

```text
Base de Datos
```

Utiliza:

```text
Expression Trees
```

---

## Ejemplo

```csharp
var clientes =
    contexto.Clientes
            .ToList();
```

Ahora tenemos:

```csharp
IEnumerable<Cliente>
```

Luego:

```csharp
clientes.Where(c => c.Activo);
```

El filtro ocurre en memoria.

---

# 11. Materialización

Materializar significa ejecutar realmente la consulta.

Métodos de materialización:

```csharp
ToList()
ToArray()
First()
FirstOrDefault()
Single()
SingleOrDefault()
Count()
Max()
Min()
Average()
```

Ejemplo:

```csharp
var lista =
    consulta.ToList();
```

Aquí la consulta se ejecuta completamente.

---

# 12. Crear Nuestro Propio IEnumerable

Podemos implementar la interfaz manualmente.

```csharp
public class Numeros
    : IEnumerable<int>
{
    public IEnumerator<int> GetEnumerator()
    {
        yield return 1;
        yield return 2;
        yield return 3;
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}
```

Uso:

```csharp
Numeros numeros = new();

foreach(var n in numeros)
{
    Console.WriteLine(n);
}
```

Salida:

```text
1
2
3
```

---

# 13. Yield Return

La palabra clave:

```csharp
yield return
```

simplifica enormemente la creación de enumeradores.

Ejemplo:

```csharp
public static IEnumerable<int> ObtenerNumeros()
{
    yield return 10;
    yield return 20;
    yield return 30;
}
```

Uso:

```csharp
foreach(var n in ObtenerNumeros())
{
    Console.WriteLine(n);
}
```

Salida:

```text
10
20
30
```

---

# 14. Ventajas

## Bajo Consumo de Memoria

Los elementos se producen bajo demanda.

```text
Lazy Loading
```

---

## Compatibilidad con LINQ

Todos los operadores LINQ funcionan sobre `IEnumerable`.

---

## Iteración Sencilla

Compatible con:

```csharp
foreach
```

---

## Reutilización

Permite crear secuencias personalizadas.

---

# 15. Desventajas

## Acceso Secuencial

No permite acceso por índice.

```csharp
coleccion[0]
```

No forma parte de la interfaz.

---

## Reenumeración

Cada recorrido puede volver a ejecutar la consulta.

```csharp
consulta.Count();
consulta.Count();
```

La consulta puede ejecutarse dos veces.

---

# 16. Caso Empresarial

Supongamos una lista de clientes:

```csharp
List<Cliente> clientes =
    servicio.ObtenerClientes();
```

Filtramos:

```csharp
var activos =
    clientes.Where(c => c.Activo);
```

Ordenamos:

```csharp
var ordenados =
    activos.OrderBy(c => c.Nombre);
```

Materializamos:

```csharp
var resultado =
    ordenados.ToList();
```

Todo ocurre localmente en memoria RAM.

---

# 17. Flujo Completo

```text
List<Cliente>
        │
        ▼
IEnumerable<Cliente>
        │
        ▼
Where()
        │
        ▼
OrderBy()
        │
        ▼
Select()
        │
        ▼
ToList()
        │
        ▼
Resultado Final
```

---

# 18. Resumen

`IEnumerable<T>` es la interfaz fundamental para representar secuencias de datos en .NET.

Características principales:

* Permite recorrer colecciones.
* Es la base de LINQ.
* Utiliza enumeradores.
* Soporta ejecución diferida.
* Trabaja sobre datos en memoria.
* Es compatible con `foreach`.
* Puede implementarse manualmente.
* Utiliza `yield return` para generar secuencias dinámicas.

Prácticamente todas las colecciones del Framework implementan `IEnumerable<T>`, convirtiéndola en una de las interfaces más importantes del ecosistema .NET.
