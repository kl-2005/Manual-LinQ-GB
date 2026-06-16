# Árboles de Expresiones (Expression Trees)

Los **Árboles de Expresiones** (Expression Trees) son estructuras de datos fuertemente tipadas que representan código de nivel de lenguaje en forma de árbol. En lugar de compilarse directamente en instrucciones binarias ejecutables (código IL), la lógica se preserva como una estructura jerárquica de nodos que puede ser inspeccionada, modificada o traducida en tiempo de ejecución.

En el ecosistema de .NET, son la columna vertebral que permite el funcionamiento de proveedores de datos remotos como **Entity Framework Core (LINQ to Entities)**.

---

# 1. El Fundamento Teórico: ¿Por qué existen?

Cuando ejecutas una consulta LINQ local (`IEnumerable`), usas un **delegado** compilado (código binario que tu CPU ejecuta directamente en la memoria RAM). Sin embargo, un servidor de base de datos como SQL Server o PostgreSQL no entiende código binario de .NET.

Para resolver esto, la arquitectura de .NET introduce `Expression<Func<T, TResult>>`. En lugar de compilar la función, el compilador genera un **modelo de datos del código**. El proveedor LINQ (por ejemplo EF Core) actúa como un compilador secundario: recorre este árbol analizando cada nodo y traduce tu lógica de C# a una cadena de texto estructurada en SQL nativo.

```text
[ Código C#: c => c.Activo ]
            |
            v
[ Árbol de Expresiones ]
            |
            v
[ SQL: WHERE Activo = 1 ]
```

---

# 2. Anatomía Jerárquica de un Nodo

Cada parte de una expresión lambda se convierte en un objeto derivado de la clase base abstracta:

```csharp
System.Linq.Expressions.Expression
```

Para la expresión:

```csharp
num => num > 5
```

La arquitectura genera la siguiente estructura:

```text
              [ LambdaExpression ]
         (Punto de entrada de la función)
                       |
                       v
              [ BinaryExpression ]
         (Operación binaria: Mayor Que)
                       |
        +--------------+--------------+
        |                             |
        v                             v
[ ParameterExpression ]       [ ConstantExpression ]
(Representa 'num')            (Representa 5)
```

## Tipos de nodos más comunes

### LambdaExpression

Representa la firma completa de una expresión lambda.

```csharp
num => num > 5
```

---

### BinaryExpression

Representa operaciones binarias:

```csharp
num > 5
num == 5
num && activo
```

---

### ParameterExpression

Representa los parámetros de entrada.

```csharp
num
```

---

### ConstantExpression

Representa valores constantes o literales.

```csharp
5
"Gabriel"
true
```

---

# 3. Ejemplo Práctico: Creación Funcional vs Creación Dinámica

## A. Compilación Automática por el Compilador

La forma más común de crear un árbol de expresiones es asignar una expresión lambda a una variable de tipo `Expression<TDelegate>`.

```csharp
using System;
using System.Linq.Expressions;

class Program
{
    static void Main()
    {
        Expression<Func<int, bool>> esMayorExp = num => num > 5;

        Console.WriteLine($"Tipo de Nodo Raíz: {esMayorExp.NodeType}");
        Console.WriteLine($"Tipo del Cuerpo: {esMayorExp.Body.NodeType}");
    }
}
```

### Salida

```text
Tipo de Nodo Raíz: Lambda
Tipo del Cuerpo: GreaterThan
```

---

## B. Construcción Manual y Dinámica (API de Expresiones)

Los árboles de expresiones pueden construirse completamente en tiempo de ejecución utilizando la clase `Expression`.

```csharp
using System;
using System.Linq.Expressions;

class Program
{
    static void Main()
    {
        // 1. Parámetro de entrada
        ParameterExpression parametroNum =
            Expression.Parameter(typeof(int), "num");

        // 2. Constante
        ConstantExpression constanteCinco =
            Expression.Constant(5);

        // 3. Operación num > 5
        BinaryExpression operacionMayorQue =
            Expression.GreaterThan(
                parametroNum,
                constanteCinco
            );

        // 4. Crear Lambda
        Expression<Func<int, bool>> expressionFinal =
            Expression.Lambda<Func<int, bool>>(
                operacionMayorQue,
                parametroNum
            );

        // 5. Compilar en memoria
        Func<int, bool> funcionCompilada =
            expressionFinal.Compile();

        bool resultado = funcionCompilada(12);

        Console.WriteLine(resultado);
    }
}
```

### Salida

```text
True
```

---

# 4. Visualización del Árbol Construido

El árbol generado para:

```csharp
num => num > 5
```

equivale a:

```text
Lambda
│
└── GreaterThan
    │
    ├── Parameter (num)
    │
    └── Constant (5)
```

---

# 5. Uso Real en Entity Framework Core

Supongamos una entidad:

```csharp
public class Cliente
{
    public int Id { get; set; }
    public string Nombre { get; set; }
    public bool Activo { get; set; }
}
```

Consulta LINQ:

```csharp
var clientes =
    contexto.Clientes
            .Where(c => c.Activo)
            .ToList();
```

EF Core recibe realmente:

```csharp
Expression<Func<Cliente, bool>>
```

Representación lógica:

```text
c => c.Activo
```

EF Core analiza el árbol y genera:

```sql
SELECT *
FROM Clientes
WHERE Activo = 1;
```

---

# 6. Filtros Dinámicos

Una ventaja enorme de los árboles de expresiones es crear consultas dinámicas sin concatenar SQL.

Ejemplo:

```csharp
string nombreBuscado = "Gabriel";

Expression<Func<Cliente, bool>> filtro =
    c => c.Nombre == nombreBuscado;
```

EF Core lo transforma automáticamente en SQL parametrizado:

```sql
SELECT *
FROM Clientes
WHERE Nombre = @p0;
```

Parámetro:

```text
@p0 = Gabriel
```

---

# 7. Beneficios en Arquitecturas Empresariales

## Seguridad contra Inyección SQL

Al trabajar con nodos tipados:

```csharp
Expression.Constant(valor);
```

los valores se convierten automáticamente en parámetros SQL.

Ejemplo:

```sql
WHERE Nombre = @p0
```

en lugar de:

```sql
WHERE Nombre = 'valor'
```

Esto elimina prácticamente el riesgo de SQL Injection.

---

## Filtros Dinámicos

Permiten construir filtros complejos:

```csharp
Nombre = "Gabriel"
AND
Activo = true
AND
Edad > 18
```

de forma totalmente dinámica.

---

## Reutilización de Reglas de Negocio

Una misma expresión puede utilizarse:

* Sobre listas en memoria (`IEnumerable`)
* Sobre Entity Framework (`IQueryable`)
* Sobre APIs
* Sobre motores de búsqueda

Ejemplo:

```csharp
Expression<Func<Cliente, bool>> clientesActivos =
    c => c.Activo;
```

Puede reutilizarse en cualquier capa de la aplicación.

---

# 8. Diferencia Entre Func y Expression

## Func

Se compila inmediatamente.

```csharp
Func<int, bool> esMayor =
    num => num > 5;
```

Representación:

```text
Código IL ejecutable
```

Se ejecuta directamente.

---

## Expression

Se almacena como estructura de nodos.

```csharp
Expression<Func<int, bool>> esMayor =
    num => num > 5;
```

Representación:

```text
Árbol de Expresiones
```

Puede inspeccionarse, modificarse o traducirse.

---

# 9. Resumen

Los Árboles de Expresiones son una representación estructurada del código en forma de árbol.

Características principales:

* Representan código como datos.
* Son fuertemente tipados.
* Permiten inspección en tiempo de ejecución.
* Son la base de Entity Framework Core.
* Facilitan consultas dinámicas.
* Previenen SQL Injection.
* Pueden compilarse dinámicamente mediante `Compile()`.

Son uno de los pilares fundamentales de LINQ avanzado y del funcionamiento interno de Entity Framework Core.
