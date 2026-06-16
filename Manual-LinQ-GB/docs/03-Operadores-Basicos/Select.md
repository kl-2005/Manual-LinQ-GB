# Select

## 1. Introducción

`Select()` es uno de los operadores fundamentales de LINQ. Su función principal es **proyectar** o **transformar** los elementos de una colección en una nueva forma.

Mientras que `Where()` filtra elementos, `Select()` transforma cada elemento de la secuencia.

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

```csharp
Select(Func<TSource, TResult> selector)
```

Donde:

* `TSource` = Tipo original.
* `TResult` = Tipo resultante.

---

# 3. ¿Cómo Funciona?

Colección original:

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
var cuadrados =
    numeros.Select(
        n => n * n
    );
```

Proceso:

```text
1 → 1
2 → 4
3 → 9
4 → 16
5 → 25
```

Resultado:

```text
1
4
9
16
25
```

---

# 4. Arquitectura Interna

```text
Colección Original
        │
        ▼
      Select
        │
        ▼
Transformación
        │
        ▼
Nueva Colección
```

---

# 5. Ejemplo Básico

```csharp
List<int> numeros =
[
    1,
    2,
    3,
    4
];

var resultado =
    numeros.Select(
        n => n * 10
    );

foreach(var item in resultado)
{
    Console.WriteLine(item);
}
```

Salida:

```text
10
20
30
40
```

---

# 6. Select con Strings

```csharp
List<string> nombres =
[
    "gabriel",
    "ana",
    "carlos"
];

var mayusculas =
    nombres.Select(
        n => n.ToUpper()
    );
```

Resultado:

```text
GABRIEL
ANA
CARLOS
```

---

# 7. Select con Objetos

Entidad:

```csharp
public class Cliente
{
    public int Id { get; set; }

    public string Nombre { get; set; }

    public string Email { get; set; }
}
```

Consulta:

```csharp
var nombres =
    clientes.Select(
        c => c.Nombre
    );
```

Resultado:

```text
Solo la propiedad Nombre
```

---

# 8. Proyección de Múltiples Propiedades

```csharp
var datos =
    clientes.Select(
        c => new
        {
            c.Nombre,
            c.Email
        }
    );
```

Resultado:

```text
Nombre
Email
```

---

# 9. Tipos Anónimos

Uno de los usos más frecuentes de Select.

```csharp
var resultado =
    clientes.Select(
        c => new
        {
            NombreCompleto = c.Nombre,
            Correo = c.Email
        }
    );
```

Genera una nueva estructura temporal.

---

# 10. Select en IEnumerable

```csharp
IEnumerable<string> nombres =
    clientes.Select(
        c => c.Nombre
    );
```

La transformación ocurre en memoria.

---

# 11. Select en IQueryable

```csharp
var consulta =
    contexto.Clientes
            .Select(
                c => c.Nombre
            );
```

Entity Framework traduce:

```sql
SELECT Nombre
FROM Clientes;
```

---

# 12. Deferred Execution

Select utiliza ejecución diferida.

```csharp
var consulta =
    numeros.Select(
        n => n * 2
    );
```

En este momento:

```text
No se ejecuta
```

La ejecución ocurre cuando:

```csharp
consulta.ToList();
```

---

# 13. Select con Índice

LINQ permite acceder al índice.

```csharp
var resultado =
    nombres.Select(
        (nombre, indice) =>
        $"{indice}: {nombre}"
    );
```

Salida:

```text
0: Gabriel
1: Ana
2: Carlos
```

---

# 14. Combinación con Where

```csharp
var resultado =
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

# 15. Combinación con OrderBy

```csharp
var resultado =
    clientes
        .OrderBy(c => c.Nombre)
        .Select(c => c.Nombre);
```

Proceso:

```text
Ordenar
     ↓
Seleccionar
```

---

# 16. Caso Empresarial

Entidad:

```csharp
public class Producto
{
    public int Id { get; set; }

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
            .Select(p => new
            {
                p.Nombre,
                p.Precio
            });
```

SQL aproximado:

```sql
SELECT Nombre,
       Precio
FROM Productos
WHERE Activo = 1;
```

---

# 17. Ventajas

## Menor Consumo de Memoria

Solo obtiene los datos necesarios.

---

## Mayor Rendimiento

Reduce la cantidad de datos transferidos.

---

## Proyecciones Personalizadas

Permite crear DTOs y tipos anónimos.

---

## Integración con EF Core

Se traduce eficientemente a SQL.

---

# 18. Errores Comunes

Incorrecto:

```csharp
var resultado =
    contexto.Clientes
            .ToList()
            .Select(c => c.Nombre);
```

Problema:

```text
Carga todos los datos antes de proyectar
```

---

Correcto:

```csharp
var resultado =
    contexto.Clientes
            .Select(c => c.Nombre)
            .ToList();
```

---

# 19. Diferencia Entre Where y Select

| Operador | Función     |
| -------- | ----------- |
| Where    | Filtrar     |
| Select   | Transformar |

Ejemplo:

```csharp
.Where(c => c.Activo)
```

↓

```text
Filtra
```

Ejemplo:

```csharp
.Select(c => c.Nombre)
```

↓

```text
Transforma
```

---

# 20. Flujo Completo

```text
Colección
      │
      ▼
Select()
      │
      ▼
Transformación
      │
      ▼
Nueva Estructura
      │
      ▼
Resultado
```

---

# 21. Resumen

`Select()` es el operador encargado de transformar o proyectar elementos de una colección.

Características principales:

* Transforma elementos.
* Permite seleccionar propiedades específicas.
* Soporta tipos anónimos.
* Implementa Deferred Execution.
* Funciona con IEnumerable e IQueryable.
* Es ampliamente utilizado con Entity Framework Core.
* Mejora el rendimiento al recuperar únicamente los datos necesarios.

En aplicaciones empresariales modernas, `Select()` se utiliza constantemente para construir DTOs, ViewModels y respuestas optimizadas para APIs y aplicaciones web.
