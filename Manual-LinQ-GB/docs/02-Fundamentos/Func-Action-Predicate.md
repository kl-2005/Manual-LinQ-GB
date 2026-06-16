# Func, Action y Predicate

## 1. Introducción

**Func**, **Action** y **Predicate** son delegates genéricos incluidos en .NET que simplifican enormemente el uso de métodos, expresiones lambda y LINQ.

Antes de su aparición, era necesario crear delegates personalizados:

```csharp
public delegate int Operacion(int a, int b);
```

Actualmente puede utilizarse:

```csharp
Func<int, int, int>
```

Estos delegates genéricos son fundamentales para:

* LINQ
* Lambda Expressions
* Entity Framework Core
* Eventos
* Programación Funcional en C#

---

# 2. Func<T>

## ¿Qué es Func?

`Func` representa un método que recibe parámetros y devuelve un valor.

Sintaxis general:

```csharp
Func<T1, T2, TResult>
```

Donde:

* `T1` = Primer parámetro
* `T2` = Segundo parámetro
* `TResult` = Valor retornado

---

## Ejemplo Básico

```csharp
Func<int, int> cuadrado =
    x => x * x;

Console.WriteLine(
    cuadrado(5)
);
```

Salida:

```text
25
```

---

## Func con Múltiples Parámetros

```csharp
Func<int, int, int> suma =
    (a, b) => a + b;

Console.WriteLine(
    suma(10, 20)
);
```

Salida:

```text
30
```

---

## Arquitectura de Func

```text
Func<int,int>
      │
      ▼
x => x * x
      │
      ▼
25
```

---

## Func en LINQ

```csharp
var nombres =
    clientes.Select(
        c => c.Nombre
    );
```

La expresión:

```csharp
c => c.Nombre
```

se representa internamente como:

```csharp
Func<Cliente,string>
```

---

# 3. Action<T>

## ¿Qué es Action?

`Action` representa un método que recibe parámetros pero no retorna ningún valor.

Sintaxis:

```csharp
Action<T>
```

o

```csharp
Action<T1,T2,T3>
```

---

## Ejemplo Básico

```csharp
Action<string> mostrar =
    mensaje =>
        Console.WriteLine(mensaje);

mostrar("Hola Mundo");
```

Salida:

```text
Hola Mundo
```

---

## Action con Múltiples Parámetros

```csharp
Action<string, int> mostrarEmpleado =
    (nombre, edad) =>
    {
        Console.WriteLine(
            $"{nombre} - {edad}"
        );
    };

mostrarEmpleado(
    "Gabriel",
    22
);
```

Salida:

```text
Gabriel - 22
```

---

## Arquitectura de Action

```text
Action<string>
        │
        ▼
mensaje => Console.WriteLine(mensaje)
        │
        ▼
void
```

---

## Action en Eventos

```csharp
Action notificacion =
    () =>
    {
        Console.WriteLine(
            "Proceso completado"
        );
    };

notificacion();
```

Salida:

```text
Proceso completado
```

---

# 4. Predicate<T>

## ¿Qué es Predicate?

`Predicate<T>` representa un método que recibe un parámetro y devuelve un valor booleano.

Sintaxis:

```csharp
Predicate<T>
```

Equivale conceptualmente a:

```csharp
Func<T,bool>
```

---

## Ejemplo Básico

```csharp
Predicate<int> esPar =
    x => x % 2 == 0;

Console.WriteLine(
    esPar(10)
);
```

Salida:

```text
True
```

---

## Uso con FindAll

```csharp
List<int> numeros =
[
    1,
    2,
    3,
    4,
    5,
    6
];

Predicate<int> esPar =
    x => x % 2 == 0;

List<int> pares =
    numeros.FindAll(esPar);

foreach(var numero in pares)
{
    Console.WriteLine(numero);
}
```

Salida:

```text
2
4
6
```

---

## Arquitectura de Predicate

```text
Predicate<int>
        │
        ▼
x => x % 2 == 0
        │
        ▼
True / False
```

---

# 5. Comparación Entre Func, Action y Predicate

| Delegate  | Retorna Valor | Uso Principal               |
| --------- | ------------- | --------------------------- |
| Func      | Sí            | Cálculos y transformaciones |
| Action    | No            | Procedimientos              |
| Predicate | bool          | Validaciones y filtros      |

---

# 6. Ejemplos Comparativos

## Func

```csharp
Func<int,int> duplicar =
    x => x * 2;

Console.WriteLine(
    duplicar(10)
);
```

Salida:

```text
20
```

---

## Action

```csharp
Action<string> mostrar =
    texto =>
        Console.WriteLine(texto);

mostrar("Hola");
```

Salida:

```text
Hola
```

---

## Predicate

```csharp
Predicate<int> positivo =
    x => x > 0;

Console.WriteLine(
    positivo(5)
);
```

Salida:

```text
True
```

---

# 7. Relación con Lambda Expressions

Las expresiones lambda suelen convertirse automáticamente en alguno de estos delegates.

Ejemplo:

```csharp
x => x * x
```

Puede convertirse en:

```csharp
Func<int,int>
```

---

Ejemplo:

```csharp
x => x > 0
```

Puede convertirse en:

```csharp
Predicate<int>
```

---

# 8. Uso en Entity Framework Core

Consulta:

```csharp
var clientes =
    contexto.Clientes
            .Where(
                c => c.Activo
            );
```

La expresión:

```csharp
c => c.Activo
```

se transforma internamente en:

```csharp
Expression<Func<Cliente,bool>>
```

que Entity Framework traduce posteriormente a SQL.

---

# 9. Caso Empresarial

Entidad:

```csharp
public class Empleado
{
    public string Nombre { get; set; }

    public decimal Salario { get; set; }

    public bool Activo { get; set; }
}
```

---

## Predicate

```csharp
Predicate<Empleado> activo =
    e => e.Activo;
```

---

## Func

```csharp
Func<Empleado, decimal> bono =
    e => e.Salario * 0.10m;
```

---

## Action

```csharp
Action<Empleado> mostrar =
    e =>
    {
        Console.WriteLine(
            e.Nombre
        );
    };
```

---

# 10. Ventajas

## Menos Código

No es necesario crear delegates personalizados.

---

## Integración con LINQ

Funcionan perfectamente con consultas LINQ.

---

## Compatibilidad con Lambdas

Permiten escribir código más limpio.

---

## Reutilización

Pueden almacenarse y reutilizarse fácilmente.

---

# 11. Desventajas

## Curva de Aprendizaje

Puede ser difícil distinguir cuándo usar cada uno.

---

## Lambdas Complejas

Una lambda excesivamente grande puede dificultar la lectura.

---

# 12. Evolución Conceptual

```text
Métodos
      │
      ▼
Delegates
      │
      ▼
Func / Action / Predicate
      │
      ▼
Lambda Expressions
      │
      ▼
Expression Trees
      │
      ▼
LINQ
```

---

# 13. Resumen

**Func**, **Action** y **Predicate** son delegates genéricos incorporados en .NET que permiten trabajar con métodos de manera flexible y elegante.

Características principales:

* Func retorna valores.
* Action no retorna valores.
* Predicate retorna valores booleanos.
* Son la base de las expresiones lambda.
* Son ampliamente utilizados por LINQ.
* Son fundamentales para Entity Framework Core.
* Permiten escribir código más limpio y reutilizable.

Comprender estos tres delegates es un paso esencial para dominar LINQ, las Lambda Expressions y el desarrollo moderno en C#.
