# Lambda Expressions (Expresiones Lambda)

Las **Lambda Expressions** son funciones anónimas que permiten escribir bloques de lógica de forma compacta y expresiva. Fueron introducidas en **C# 3.0** y constituyen uno de los pilares fundamentales de **LINQ**, **Entity Framework Core**, **Delegates** y **Expression Trees**.

Una expresión lambda puede considerarse una forma abreviada de escribir métodos pequeños que pueden pasarse como parámetros, almacenarse en variables o utilizarse en consultas LINQ.

---

# 1. ¿Qué es una Lambda Expression?

Una expresión lambda es una función sin nombre que puede recibir parámetros y devolver un resultado.

Sintaxis general:

```csharp
(parametros) => expresion
```

o

```csharp
(parametros) =>
{
    // bloque de código
}
```

Ejemplo:

```csharp
x => x * 2
```

Significa:

```csharp
Recibe un valor x
↓
Lo multiplica por 2
↓
Retorna el resultado
```

---

# 2. ¿Por Qué Existen?

Antes de C# 3.0 se utilizaban métodos tradicionales o delegados anónimos.

Ejemplo tradicional:

```csharp
public static int Duplicar(int numero)
{
    return numero * 2;
}
```

Uso:

```csharp
Func<int, int> funcion = Duplicar;
```

Con Lambda:

```csharp
Func<int, int> funcion =
    x => x * 2;
```

Resultado:

```text
Menos código
Mayor legibilidad
Mayor productividad
```

---

# 3. Anatomía de una Lambda

Ejemplo:

```csharp
x => x * 2
```

Desglose:

```text
x       → Parámetro
=>
        → Operador Lambda
x * 2   → Expresión de retorno
```

---

# 4. Sintaxis Básica

## Un parámetro

```csharp
x => x + 10
```

---

## Múltiples parámetros

```csharp
(x, y) => x + y
```

---

## Sin parámetros

```csharp
() => DateTime.Now
```

---

## Bloque de código

```csharp
x =>
{
    int resultado = x * 2;
    return resultado;
}
```

---

# 5. Lambdas y Delegates

Las lambdas suelen almacenarse en delegados.

Ejemplo:

```csharp
Func<int, int> duplicar =
    x => x * 2;
```

Uso:

```csharp
Console.WriteLine(
    duplicar(10)
);
```

Salida:

```text
20
```

---

# 6. Delegados Más Utilizados

## Func

Representa métodos que retornan valores.

```csharp
Func<int, int> cuadrado =
    x => x * x;
```

Uso:

```csharp
Console.WriteLine(
    cuadrado(5)
);
```

Salida:

```text
25
```

---

## Action

Representa métodos que no retornan valores.

```csharp
Action<string> mostrar =
    mensaje => Console.WriteLine(mensaje);
```

Uso:

```csharp
mostrar("Hola Mundo");
```

---

## Predicate

Representa condiciones booleanas.

```csharp
Predicate<int> esPar =
    x => x % 2 == 0;
```

Uso:

```csharp
Console.WriteLine(
    esPar(10)
);
```

Salida:

```text
True
```

---

# 7. Lambdas en LINQ

Son utilizadas constantemente en LINQ.

Lista:

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
var mayores =
    numeros.Where(
        n => n > 10
    );
```

Lambda:

```csharp
n => n > 10
```

Significa:

```text
Recibir cada elemento
↓
Evaluar si es mayor que 10
↓
Retornar true o false
```

Resultado:

```text
15
20
25
```

---

# 8. Lambdas con Select

Permiten transformar datos.

```csharp
var cuadrados =
    numeros.Select(
        n => n * n
    );
```

Proceso:

```text
5  → 25
10 → 100
15 → 225
20 → 400
```

---

# 9. Lambdas y Objetos

Entidad:

```csharp
public class Cliente
{
    public int Id { get; set; }

    public string Nombre { get; set; }

    public bool Activo { get; set; }
}
```

Consulta:

```csharp
var activos =
    clientes.Where(
        c => c.Activo
    );
```

Lambda:

```csharp
c => c.Activo
```

---

# 10. Lambdas y Ordenamiento

```csharp
var ordenados =
    clientes.OrderBy(
        c => c.Nombre
    );
```

Lambda:

```csharp
c => c.Nombre
```

Indica qué propiedad utilizar para ordenar.

---

# 11. Lambdas y Métodos

Método:

```csharp
public static void Procesar(
    Func<int, bool> criterio)
{
    Console.WriteLine(
        criterio(10)
    );
}
```

Uso:

```csharp
Procesar(
    x => x > 5
);
```

Salida:

```text
True
```

---

# 12. Lambdas Multilínea

Cuando la lógica es más compleja.

```csharp
Func<int, int> calculo =
    x =>
    {
        int doble = x * 2;

        int resultado =
            doble + 5;

        return resultado;
    };
```

Uso:

```csharp
Console.WriteLine(
    calculo(10)
);
```

Salida:

```text
25
```

---

# 13. Inferencia de Tipos

El compilador puede inferir los tipos.

Ejemplo:

```csharp
Func<int, int> cuadrado =
    x => x * x;
```

Aunque no escribimos:

```csharp
(int x) => x * x
```

el compilador sabe que:

```text
x es int
```

---

# 14. Lambda Expression vs Método Tradicional

## Método

```csharp
public static bool EsPar(
    int numero)
{
    return numero % 2 == 0;
}
```

Uso:

```csharp
Func<int, bool> funcion =
    EsPar;
```

---

## Lambda

```csharp
Func<int, bool> funcion =
    n => n % 2 == 0;
```

---

# 15. Lambda Expression vs Anonymous Method

## Método Anónimo

```csharp
Func<int, bool> esPar =
delegate(int n)
{
    return n % 2 == 0;
};
```

---

## Lambda

```csharp
Func<int, bool> esPar =
    n => n % 2 == 0;
```

Más simple y legible.

---

# 16. Lambda Expressions y Expression Trees

Las lambdas pueden convertirse en árboles de expresiones.

Ejemplo:

```csharp
Expression<Func<int, bool>>
    expresion =
        n => n > 5;
```

Resultado:

```text
LambdaExpression
        │
        ▼
GreaterThan
    │       │
    ▼       ▼
 n         5
```

Esta capacidad es utilizada por Entity Framework Core.

---

# 17. Caso Empresarial

Supongamos una aplicación de ventas.

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
            .Where(
                p => p.Activo
            )
            .OrderBy(
                p => p.Nombre
            )
            .Select(
                p => p.Nombre
            );
```

Lambdas utilizadas:

```csharp
p => p.Activo

p => p.Nombre

p => p.Nombre
```

EF Core las transforma en SQL.

---

# 18. Ventajas

## Menos Código

Permiten escribir funciones de forma compacta.

---

## Mayor Legibilidad

Especialmente en LINQ.

---

## Reutilización

Pueden almacenarse en variables.

---

## Compatibilidad con LINQ

Son la base de todas las consultas LINQ.

---

## Compatibilidad con Expression Trees

Permiten construir consultas dinámicas.

---

# 19. Desventajas

## Complejidad

Las lambdas muy largas pueden dificultar la lectura.

---

## Depuración

A veces son más difíciles de depurar que métodos tradicionales.

---

# 20. Flujo Completo

```text
Lambda Expression
        │
        ▼
Delegate (Func)
        │
        ▼
LINQ
        │
        ▼
IEnumerable
        │
        ▼
IQueryable
        │
        ▼
Expression Tree
        │
        ▼
Entity Framework Core
        │
        ▼
SQL
```

---

# 21. Resumen

Las **Lambda Expressions** son funciones anónimas que permiten expresar lógica de manera compacta y flexible.

Características principales:

* Son funciones sin nombre.
* Utilizan el operador `=>`.
* Pueden almacenarse en delegados.
* Son la base de LINQ.
* Funcionan con `Func`, `Action` y `Predicate`.
* Permiten construir Expression Trees.
* Son ampliamente utilizadas en Entity Framework Core.
* Mejoran la legibilidad y productividad del código.

Prácticamente todas las consultas LINQ modernas utilizan expresiones lambda para definir filtros, transformaciones, ordenamientos y agrupaciones de datos.
