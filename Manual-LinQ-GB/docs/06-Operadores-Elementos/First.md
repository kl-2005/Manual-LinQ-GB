# First

## 1. Introducción

`First()` es un operador de elementos de LINQ que permite obtener el primer elemento de una secuencia.

Pertenece al namespace:

```csharp
System.Linq
```

Es uno de los operadores más utilizados tanto en:

- LINQ to Objects
- Entity Framework Core
- LINQ to SQL

---

# 2. ¿Qué Hace First?

Devuelve:

```text
El primer elemento encontrado
```

Si no existe ningún elemento:

```text
Lanza una excepción
```

---

# 3. Sintaxis

## Obtener el primer elemento

```csharp
First()
```

## Obtener el primer elemento que cumpla una condición

```csharp
First(
    Func<TSource,bool> predicate
)
```

---

# 4. Ejemplo Básico

```csharp
List<int> numeros =
[
    10,
    20,
    30,
    40
];

int primero =
    numeros.First();

Console.WriteLine(primero);
```

Resultado:

```text
10
```

---

# 5. Arquitectura Interna

```text
Colección
      │
      ▼
   First()
      │
      ▼
Tomar Primer Elemento
      │
      ▼
Resultado
```

---

# 6. First con Condición

```csharp
int numero =
    numeros.First(
        n => n > 15
    );
```

Proceso:

```text
10 -> No
20 -> Sí
```

Resultado:

```text
20
```

---

# 7. First con Objetos

Clase:

```csharp
public class Cliente
{
    public int Id { get; set; }
    public string Nombre { get; set; }
}
```

Consulta:

```csharp
Cliente cliente =
    clientes.First(
        c => c.Id == 5
    );
```

---

# 8. Excepción

Si no existe ningún elemento:

```csharp
List<int> numeros = [];

var resultado =
    numeros.First();
```

Produce:

```text
InvalidOperationException
```

Mensaje típico:

```text
Sequence contains no elements
```

---

# 9. Excepción con Condición

```csharp
var resultado =
    numeros.First(
        n => n > 100
    );
```

Resultado:

```text
InvalidOperationException
```

Mensaje:

```text
Sequence contains no matching element
```

---

# 10. Entity Framework

Consulta:

```csharp
var cliente =
    contexto.Clientes
        .First(
            c => c.Id == 5
        );
```

SQL aproximado:

```sql
SELECT TOP(1) *
FROM Clientes
WHERE Id = 5
```

---

# 11. Caso Empresarial

Obtener el primer cliente activo:

```csharp
var cliente =
    clientes.First(
        c => c.Activo
    );
```

Obtener la primera venta:

```csharp
var venta =
    ventas.First();
```

---

# 12. Rendimiento

Ventajas:

- Se detiene al encontrar el primer resultado.
- No recorre toda la colección.
- Muy eficiente.

---

# 13. Diferencia con Where

## Where

```csharp
clientes.Where(
    c => c.Activo
);
```

Devuelve:

```text
Muchos elementos
```

---

## First

```csharp
clientes.First(
    c => c.Activo
);
```

Devuelve:

```text
Un único elemento
```

---

# 14. Diferencia con FirstOrDefault

## First

```csharp
clientes.First();
```

Si no existe:

```text
Excepción
```

---

## FirstOrDefault

```csharp
clientes.FirstOrDefault();
```

Si no existe:

```text
null o valor por defecto
```

---

# 15. Ejemplos Reales

Primer empleado:

```csharp
var empleado =
    empleados.First();
```

Primer producto con stock:

```csharp
var producto =
    productos.First(
        p => p.Stock > 0
    );
```

Primera factura pendiente:

```csharp
var factura =
    facturas.First(
        f => !f.Pagada
    );
```

---

# 16. Resumen

`First()` devuelve el primer elemento de una secuencia.

Características:

- Devuelve un único elemento.
- Puede utilizar condiciones.
- Compatible con Entity Framework.
- Muy eficiente.
- Ejecución inmediata.
- Lanza excepción si no encuentra resultados.
