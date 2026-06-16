# Skip

## 1. Introducción

`Skip()` es un operador de LINQ que permite omitir una cantidad específica de elementos al inicio de una secuencia.

Se utiliza frecuentemente junto con `Take()` para implementar:

* Paginación
* Carga incremental de datos
* Navegación por registros
* APIs REST
* Reportes empresariales

Pertenece al namespace:

```csharp id="8m4b2k"
System.Linq
```

---

# 2. Sintaxis

```csharp id="k8zv4x"
Skip(int count)
```

Donde:

* `count` = Número de elementos que serán omitidos.

---

# 3. ¿Cómo Funciona?

Colección:

```csharp id="d6k9rm"
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

```csharp id="7g4zba"
var resultado =
    numeros.Skip(2);
```

Proceso:

```text id="4v2mcu"
1
2
3
4
5

Omitir 2
    ↓

3
4
5
```

---

# 4. Arquitectura Interna

```text id="v5k8ew"
Colección
     │
     ▼
 Skip()
     │
     ▼
Omitir N elementos
     │
     ▼
Resultado
```

---

# 5. Ejemplo Básico

```csharp id="m3h7zq"
List<int> numeros =
[
    10,
    20,
    30,
    40,
    50
];

var resultado =
    numeros.Skip(2);

foreach(var item in resultado)
{
    Console.WriteLine(item);
}
```

Salida:

```text id="7ebq0l"
30
40
50
```

---

# 6. Skip con Strings

```csharp id="g6n3tj"
List<string> nombres =
[
    "Ana",
    "Carlos",
    "Gabriel",
    "Pedro"
];

var resultado =
    nombres.Skip(1);
```

Salida:

```text id="t3e0vc"
Carlos
Gabriel
Pedro
```

---

# 7. Skip con Objetos

Entidad:

```csharp id="w0m1bs"
public class Cliente
{
    public int Id { get; set; }

    public string Nombre { get; set; }
}
```

Consulta:

```csharp id="j2n7hk"
var resultado =
    clientes.Skip(5);
```

Resultado:

```text id="lr5s9d"
Omite los primeros 5 clientes
```

---

# 8. Deferred Execution

Skip utiliza ejecución diferida.

```csharp id="e5n7cw"
var consulta =
    numeros.Skip(3);
```

En este momento:

```text id="h0d4qt"
Todavía no se ejecuta
```

La ejecución ocurre cuando:

```csharp id="q8m6xf"
consulta.ToList();
```

o

```csharp id="s1j5ye"
foreach(var item in consulta)
{
}
```

---

# 9. Skip en IEnumerable

```csharp id="n4v2yr"
IEnumerable<int> resultado =
    numeros.Skip(2);
```

La operación se realiza en memoria RAM.

---

# 10. Skip en IQueryable

```csharp id="v7c9zu"
var resultado =
    contexto.Clientes
            .Skip(10);
```

SQL generado:

```sql id="j5r8ok"
OFFSET 10 ROWS
```

En SQL Server:

```sql id="b3k1xp"
SELECT *
FROM Clientes
ORDER BY Id
OFFSET 10 ROWS;
```

---

# 11. Combinación con Take

La combinación más común.

```csharp id="r8h4yj"
var resultado =
    numeros
        .Skip(2)
        .Take(3);
```

Proceso:

```text id="k0z3mt"
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

# 12. Paginación

Supongamos:

```text id="n6p4ea"
Página = 3

Tamaño = 10
```

Cálculo:

```csharp id="y4t8xn"
int pagina = 3;
int tamanio = 10;

var resultado =
    clientes
        .Skip(
            (pagina - 1) * tamanio
        )
        .Take(tamanio);
```

Resultado:

```text id="x2m5dz"
Registros 21 al 30
```

---

# 13. Fórmula de Paginación

```text id="0g8u3v"
Skip = (Página - 1) × TamañoPágina
```

Ejemplos:

| Página | Tamaño | Skip |
| ------ | ------ | ---- |
| 1      | 10     | 0    |
| 2      | 10     | 10   |
| 3      | 10     | 20   |
| 4      | 10     | 30   |

---

# 14. Combinación con Where

```csharp id="m7q4cb"
var resultado =
    clientes
        .Where(c => c.Activo)
        .Skip(5);
```

Proceso:

```text id="k3r2vw"
Where
   ↓
Skip
   ↓
Resultado
```

---

# 15. Combinación con OrderBy

```csharp id="u9x4lj"
var resultado =
    clientes
        .OrderBy(c => c.Nombre)
        .Skip(10);
```

Proceso:

```text id="p4w8km"
OrderBy
   ↓
Skip
   ↓
Resultado
```

---

# 16. Caso Empresarial

Entidad:

```csharp id="f1y7ez"
public class Venta
{
    public int Id { get; set; }

    public decimal Total { get; set; }
}
```

Consulta:

```csharp id="v8j2na"
var ventas =
    contexto.Ventas
            .OrderBy(v => v.Id)
            .Skip(50)
            .Take(25);
```

SQL aproximado:

```sql id="a0c9bh"
SELECT *
FROM Ventas
ORDER BY Id
OFFSET 50 ROWS
FETCH NEXT 25 ROWS ONLY;
```

---

# 17. Ventajas

## Paginación

Ideal para sistemas con grandes volúmenes de datos.

---

## Mejor Rendimiento

Reduce la cantidad de registros recuperados.

---

## Compatible con SQL

Entity Framework lo traduce eficientemente.

---

## Fácil Implementación

Requiere muy poco código.

---

# 18. Errores Comunes

Incorrecto:

```csharp id="z7w3dh"
contexto.Clientes
        .ToList()
        .Skip(100);
```

Problema:

```text id="j2v9ke"
Carga todos los registros en memoria
```

---

Correcto:

```csharp id="q5n8mu"
contexto.Clientes
        .Skip(100);
```

---

# 19. Diferencia Entre Skip y Take

| Método | Función           |
| ------ | ----------------- |
| Skip   | Omite elementos   |
| Take   | Obtiene elementos |

Ejemplo:

```csharp id="e6x4jo"
.Skip(5)
```

↓

```text id="j7z1pq"
Ignora los primeros 5
```

Ejemplo:

```csharp id="n0r3lx"
.Take(5)
```

↓

```text id="c4k9tw"
Obtiene los primeros 5
```

---

# 20. Flujo Completo

```text id="s8y2ab"
Colección
      │
      ▼
Skip()
      │
      ▼
Omitir Registros
      │
      ▼
Elementos Restantes
      │
      ▼
Resultado
```

---

# 21. Resumen

`Skip()` permite omitir una cantidad determinada de elementos al inicio de una secuencia.

Características principales:

* Omite registros.
* Implementa Deferred Execution.
* Funciona con IEnumerable e IQueryable.
* Se traduce eficientemente a SQL.
* Es fundamental para la paginación.
* Se utiliza habitualmente junto con Take().
* Mejora el rendimiento al trabajar con grandes volúmenes de datos.

En aplicaciones empresariales modernas, `Skip()` es una herramienta esencial para implementar tablas paginadas, reportes, APIs REST y cualquier escenario donde se necesite navegar grandes conjuntos de información de forma eficiente.
