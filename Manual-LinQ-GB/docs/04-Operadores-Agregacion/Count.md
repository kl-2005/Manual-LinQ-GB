# Count

## 1. Introducción

`Count()` es un operador de agregación de LINQ que permite obtener la cantidad de elementos de una secuencia.

Es equivalente a la función SQL:

```sql
COUNT(*)
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

## Contar todos los elementos

```csharp
Count()
```

## Contar según una condición

```csharp
Count(
    Func<TSource,bool> predicate
)
```

---

# 3. ¿Cómo Funciona?

Colección:

```csharp
List<int> numeros =
[
    10,
    20,
    30,
    40,
    50
];
```

Consulta:

```csharp
int cantidad =
    numeros.Count();
```

Resultado:

```text
5
```

---

# 4. Arquitectura Interna

```text
Colección
      │
      ▼
   Count()
      │
      ▼
Cantidad de Elementos
      │
      ▼
Resultado Entero
```

---

# 5. Ejemplo Básico

```csharp
string[] nombres =
{
    "Ana",
    "Carlos",
    "Luis",
    "María"
};

int total =
    nombres.Count();

Console.WriteLine(total);
```

Salida:

```text
4
```

---

# 6. Count con Condición

```csharp
var total =
    numeros.Count(
        n => n > 25
    );
```

Resultado:

```text
3
```

Elementos:

```text
30
40
50
```

---

# 7. Count con Objetos

```csharp
var clientesActivos =
    clientes.Count(
        c => c.Activo
    );
```

Resultado:

```text
Cantidad de clientes activos
```

---

# 8. SQL Generado por Entity Framework

Consulta:

```csharp
var total =
    contexto.Clientes.Count();
```

SQL aproximado:

```sql
SELECT COUNT(*)
FROM Clientes;
```

---

# 9. Count con Where

```csharp
var total =
    productos
        .Where(
            p => p.Stock > 0
        )
        .Count();
```

---

# 10. Deferred Execution

Count es un operador de ejecución inmediata.

```csharp
var total =
    numeros.Count();
```

La consulta se ejecuta inmediatamente.

---

# 11. Caso Empresarial

Contar ventas realizadas:

```csharp
var ventasHoy =
    contexto.Ventas.Count(
        v => v.Fecha ==
             DateTime.Today
    );
```

---

# 12. Ventajas

* Muy rápido.
* Compatible con SQL.
* Fácil de leer.
* Ideal para reportes.

---

# 13. Diferencia con Any

## Count

```csharp
clientes.Count() > 0
```

Cuenta todos los registros.

---

## Any

```csharp
clientes.Any()
```

Solo verifica existencia.

Más eficiente cuando únicamente se desea saber si existen registros.

---

# 14. Resumen

`Count()` devuelve la cantidad de elementos de una secuencia.

Características principales:

* Equivalente a COUNT de SQL.
* Puede utilizar condiciones.
* Compatible con Entity Framework.
* Ejecución inmediata.
* Muy utilizado en dashboards y reportes.
* Retorna un valor de tipo Int32.
