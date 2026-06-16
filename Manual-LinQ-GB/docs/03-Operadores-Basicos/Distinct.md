# Distinct

## 1. Introducción

`Distinct()` es un operador de LINQ que permite eliminar elementos duplicados de una secuencia.

Su objetivo es devolver únicamente valores únicos.

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

```csharp
Distinct()
```

No requiere parámetros cuando trabaja con tipos primitivos.

También existe una sobrecarga que utiliza comparadores personalizados:

```csharp
Distinct(IEqualityComparer<T>)
```

---

# 3. ¿Cómo Funciona?

Colección original:

```csharp
List<int> numeros =
[
    1,
    2,
    2,
    3,
    4,
    4,
    5
];
```

Consulta:

```csharp
var resultado =
    numeros.Distinct();
```

Proceso:

```text
1
2
2
3
4
4
5
    ↓
1
2
3
4
5
```

---

# 4. Arquitectura Interna

```text
Colección
     │
     ▼
Distinct()
     │
     ▼
Eliminar Duplicados
     │
     ▼
Resultado Único
```

---

# 5. Ejemplo Básico

```csharp
List<int> numeros =
[
    10,
    20,
    20,
    30,
    30,
    30
];

var resultado =
    numeros.Distinct();

foreach(var numero in resultado)
{
    Console.WriteLine(numero);
}
```

Salida:

```text
10
20
30
```

---

# 6. Distinct con Strings

```csharp
List<string> ciudades =
[
    "Quito",
    "Ambato",
    "Quito",
    "Guayaquil",
    "Ambato"
];

var resultado =
    ciudades.Distinct();
```

Salida:

```text
Quito
Ambato
Guayaquil
```

---

# 7. Distinct con Select

Uno de los usos más comunes.

```csharp
var ciudades =
    clientes
        .Select(c => c.Ciudad)
        .Distinct();
```

Proceso:

```text
Clientes
    ↓
Select(Ciudad)
    ↓
Distinct()
    ↓
Ciudades Únicas
```

---

# 8. Distinct en Objetos

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
    clientes.Distinct();
```

Problema:

```text
No elimina duplicados correctamente
```

Porque LINQ compara referencias de objetos.

---

# 9. Comparador Personalizado

```csharp
public class ClienteComparer :
    IEqualityComparer<Cliente>
{
    public bool Equals(
        Cliente x,
        Cliente y)
    {
        return x.Id == y.Id;
    }

    public int GetHashCode(
        Cliente obj)
    {
        return obj.Id.GetHashCode();
    }
}
```

Uso:

```csharp
var resultado =
    clientes.Distinct(
        new ClienteComparer()
    );
```

---

# 10. DistinctBy (.NET 6+)

A partir de .NET 6 existe:

```csharp
DistinctBy()
```

Ejemplo:

```csharp
var resultado =
    clientes.DistinctBy(
        c => c.Id
    );
```

Resultado:

```text
Clientes únicos por Id
```

---

# 11. Deferred Execution

Distinct utiliza ejecución diferida.

```csharp
var consulta =
    numeros.Distinct();
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

# 12. Distinct en IEnumerable

```csharp
IEnumerable<int> resultado =
    numeros.Distinct();
```

La eliminación de duplicados ocurre en memoria RAM.

---

# 13. Distinct en IQueryable

```csharp
var consulta =
    contexto.Clientes
            .Select(c => c.Ciudad)
            .Distinct();
```

SQL generado:

```sql
SELECT DISTINCT Ciudad
FROM Clientes;
```

---

# 14. Combinación con Where

```csharp
var resultado =
    clientes
        .Where(c => c.Activo)
        .Select(c => c.Ciudad)
        .Distinct();
```

Proceso:

```text
Where
   ↓
Select
   ↓
Distinct
   ↓
Resultado
```

---

# 15. Combinación con OrderBy

```csharp
var resultado =
    clientes
        .Select(c => c.Ciudad)
        .Distinct()
        .OrderBy(c => c);
```

Proceso:

```text
Select
   ↓
Distinct
   ↓
OrderBy
```

---

# 16. Caso Empresarial

Entidad:

```csharp
public class Venta
{
    public string Ciudad { get; set; }

    public decimal Total { get; set; }
}
```

Consulta:

```csharp
var ciudades =
    contexto.Ventas
            .Select(v => v.Ciudad)
            .Distinct();
```

SQL aproximado:

```sql
SELECT DISTINCT Ciudad
FROM Ventas;
```

Uso típico:

```text
Llenar un ComboBox
Filtro de búsqueda
Lista de ciudades
```

---

# 17. Ventajas

## Elimina Duplicados

Obtiene únicamente valores únicos.

---

## Menor Cantidad de Datos

Reduce información repetida.

---

## Compatible con SQL

Se traduce como DISTINCT.

---

## Fácil de Utilizar

Requiere una sola llamada.

---

# 18. Errores Comunes

Incorrecto:

```csharp
clientes
    .Distinct()
```

cuando se trabaja con objetos.

Problema:

```text
Compara referencias
No propiedades
```

---

Correcto:

```csharp
clientes
    .DistinctBy(c => c.Id)
```

o

```csharp
clientes
    .Distinct(new ClienteComparer())
```

---

# 19. Diferencia Entre Distinct y GroupBy

## Distinct

```csharp
.Select(c => c.Ciudad)
.Distinct()
```

Obtiene:

```text
Quito
Ambato
Guayaquil
```

---

## GroupBy

```csharp
.GroupBy(c => c.Ciudad)
```

Obtiene:

```text
Grupo Quito
Grupo Ambato
Grupo Guayaquil
```

---

# 20. Flujo Completo

```text
Colección
      │
      ▼
Distinct()
      │
      ▼
Eliminar Repetidos
      │
      ▼
Elementos Únicos
      │
      ▼
Resultado
```

---

# 21. Resumen

`Distinct()` elimina elementos duplicados de una secuencia y devuelve únicamente valores únicos.

Características principales:

* Elimina duplicados.
* Implementa Deferred Execution.
* Funciona con IEnumerable e IQueryable.
* Se traduce a SQL mediante la cláusula DISTINCT.
* Puede utilizar comparadores personalizados.
* En .NET 6+ puede complementarse con DistinctBy().
* Es ampliamente utilizado para filtros, reportes y listas únicas.

En aplicaciones empresariales es común utilizar `Distinct()` para obtener listas únicas de ciudades, categorías, departamentos, clientes o cualquier dato repetido que necesite presentarse una sola vez.
