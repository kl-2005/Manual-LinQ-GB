# Delegates (Delegados)

Los **Delegates** son tipos especiales de datos en C# que permiten almacenar referencias a métodos. Funcionan de manera similar a los punteros a funciones en otros lenguajes, pero de forma segura y orientada a objetos.

Son uno de los pilares fundamentales sobre los que se construyen:

* Lambda Expressions
* LINQ
* Eventos
* Func
* Action
* Predicate
* Programación Asíncrona

Namespace relacionado:

```csharp
System
```

---

# 1. ¿Qué es un Delegate?

Un delegate es un tipo que puede almacenar la referencia de uno o varios métodos que tengan la misma firma.

Ejemplo:

```csharp
public delegate int Operacion(int a, int b);
```

Representación:

```text
Delegate Operacion
       │
       ▼
Método Compatible
       │
       ▼
int Metodo(int a, int b)
```

---

# 2. ¿Por Qué Existen?

Supongamos que tenemos:

```csharp
public static int Sumar(int a, int b)
{
    return a + b;
}
```

Podemos guardar este método dentro de una variable:

```csharp
Operacion op = Sumar;
```

Y ejecutarlo posteriormente:

```csharp
int resultado = op(10, 5);
```

Salida:

```text
15
```

---

# 3. Declaración de un Delegate

Sintaxis:

```csharp
public delegate TipoRetorno NombreDelegate(
    Parametros
);
```

Ejemplo:

```csharp
public delegate void Mensaje(string texto);
```

---

# 4. Primer Ejemplo Completo

```csharp
using System;

public delegate int Operacion(int a, int b);

class Program
{
    static int Sumar(int x, int y)
    {
        return x + y;
    }

    static void Main()
    {
        Operacion op = Sumar;

        int resultado = op(5, 10);

        Console.WriteLine(resultado);
    }
}
```

Salida:

```text
15
```

---

# 5. Arquitectura Interna

```text
Delegate
    │
    ▼
Método
    │
    ▼
Invocación
```

Ejemplo:

```text
Operacion op
      │
      ▼
Sumar()
      │
      ▼
Resultado
```

---

# 6. Delegates con Métodos Diferentes

```csharp
public delegate int Operacion(int a, int b);
```

Métodos compatibles:

```csharp
static int Sumar(int a, int b)
{
    return a + b;
}

static int Restar(int a, int b)
{
    return a - b;
}
```

Uso:

```csharp
Operacion op;

op = Sumar;
Console.WriteLine(op(10,5));

op = Restar;
Console.WriteLine(op(10,5));
```

Salida:

```text
15
5
```

---

# 7. Multicast Delegates

Un delegate puede almacenar varios métodos.

```csharp
public delegate void Notificacion();
```

Métodos:

```csharp
static void Correo()
{
    Console.WriteLine("Correo enviado");
}

static void SMS()
{
    Console.WriteLine("SMS enviado");
}
```

Uso:

```csharp
Notificacion notificar =
    Correo;

notificar += SMS;

notificar();
```

Salida:

```text
Correo enviado
SMS enviado
```

---

# 8. Delegate como Parámetro

Podemos enviar métodos como parámetros.

```csharp
public delegate int Operacion(
    int a,
    int b
);
```

Método:

```csharp
static void Procesar(
    Operacion operacion)
{
    Console.WriteLine(
        operacion(10,5)
    );
}
```

Uso:

```csharp
Procesar(Sumar);
```

Salida:

```text
15
```

---

# 9. Delegates Anónimos

Antes de las Lambdas existían los métodos anónimos.

```csharp
Operacion op =
delegate(int a, int b)
{
    return a + b;
};
```

Uso:

```csharp
Console.WriteLine(
    op(10,20)
);
```

Salida:

```text
30
```

---

# 10. Relación con Lambda Expressions

Delegate tradicional:

```csharp
Operacion op =
delegate(int a, int b)
{
    return a + b;
};
```

Lambda:

```csharp
Operacion op =
    (a,b) => a + b;
```

Resultado:

```text
Menos código
Mayor legibilidad
```

---

# 11. Delegates en LINQ

LINQ utiliza delegates constantemente.

Ejemplo:

```csharp
var resultado =
    numeros.Where(
        n => n > 10
    );
```

La expresión:

```csharp
n => n > 10
```

se convierte internamente en:

```csharp
Func<int,bool>
```

que es un delegate.

---

# 12. Caso Empresarial

Sistema de pagos:

```csharp
public delegate decimal
MetodoPago(decimal valor);
```

Métodos:

```csharp
static decimal PagoEfectivo(
    decimal valor)
{
    return valor;
}

static decimal PagoTarjeta(
    decimal valor)
{
    return valor * 1.05m;
}
```

Uso:

```csharp
MetodoPago pago =
    PagoTarjeta;

decimal total =
    pago(100);

Console.WriteLine(total);
```

Salida:

```text
105
```

---

# 13. Ventajas

## Reutilización

Permiten reutilizar lógica.

---

## Flexibilidad

Permiten intercambiar métodos dinámicamente.

---

## Base de Eventos

Son la base del sistema de eventos de .NET.

---

## Base de LINQ

LINQ depende completamente de delegates.

---

# 14. Desventajas

## Complejidad Inicial

Pueden resultar difíciles de comprender al principio.

---

## Menor Legibilidad

Cuando existen demasiados delegates personalizados.

---

# 15. Evolución de los Delegates

```text
Métodos
      │
      ▼
Delegates
      │
      ▼
Métodos Anónimos
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

# 16. Diferencia Entre Delegate y Método

| Característica                | Método | Delegate |
| ----------------------------- | ------ | -------- |
| Ejecuta lógica                | Sí     | No       |
| Almacena referencia           | No     | Sí       |
| Puede cambiar método          | No     | Sí       |
| Puede enviarse como parámetro | No     | Sí       |

---

# 17. Resumen

Los Delegates son tipos especiales que permiten almacenar referencias a métodos.

Características principales:

* Funcionan como punteros seguros a funciones.
* Permiten almacenar métodos en variables.
* Permiten enviar métodos como parámetros.
* Soportan múltiples métodos (Multicast).
* Son la base de Eventos.
* Son la base de Func, Action y Predicate.
* Son fundamentales para LINQ.
* Son el precursor de las Lambda Expressions.

Comprender Delegates es esencial para entender cómo funcionan internamente LINQ, los eventos y gran parte de la arquitectura moderna de .NET.
