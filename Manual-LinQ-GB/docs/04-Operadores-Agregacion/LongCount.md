# LongCount

## 1. Introducción

`LongCount()` es un operador de agregación de LINQ que permite contar la cantidad de elementos de una secuencia utilizando un valor de tipo `long`.

Es similar a:

```csharp
Count()
```

La diferencia es que devuelve un:

```csharp
Int64
```

en lugar de:

```csharp
Int32
```

---

# 2. ¿Por qué Existe?

`Count()` devuelve un entero de 32 bits:

```csharp
int
```

Capacidad máxima:

```text
2,147,483,647
```

Si una secuencia supera esa cantidad de registros, se debe utilizar:

```csharp
LongCount()
```

que devuelve:

```csharp
long
```

Capacidad máxima:

```text
9,223,372,036,854,775,807
```

---

# 3. Sintaxis

## Contar todos los elementos

```csharp
LongCount()
```

## Contar con condición

```csharp
LongCount(
    Func<TSource,bool> predicate
)
```

---

# 4. Ejemplo Básico

```csharp
var numeros =
    Enumerable.Range(1,100);

long cantidad =
    numeros.LongCount();

Console.WriteLine(cantidad);
```

Resultado:

```text
100
```

---

# 5. Tipo de Retorno

```csharp
var total =
    numeros.LongCount();

Console.WriteLine(
    total.GetType()
);
```

Resultado:

```text
System.Int64
```

---

# 6. LongCount con Condición

```csharp
long mayores =
    numeros.LongCount(
        n => n > 50
    );
```

Resultado:

```text
50
```

---

# 7. Arquitectura

```text
Colección
      │
      ▼
 LongCount()
      │
      ▼
Cuenta Elementos
      │
      ▼
Retorna Int64
```

---

# 8. Ejemplo con Objetos

```csharp
long clientesActivos =
    clientes.LongCount(
        c => c.Activo
    );
```

---

# 9. Entity Framework

Consulta:

```csharp
long total =
    contexto.Clientes
        .LongCount();
```

SQL aproximado:

```sql
SELECT COUNT_BIG(*)
FROM Clientes;
```

---

# 10. Diferencia entre Count y LongCount

| Método | Retorno |
|----------|----------|
| Count | Int32 |
| LongCount | Int64 |

---

## Count

```csharp
int total =
    clientes.Count();
```

---

## LongCount

```csharp
long total =
    clientes.LongCount();
```

---

# 11. ¿Cuándo Usarlo?

Usar LongCount cuando:

- Existen millones de registros.
- Se trabaja con Big Data.
- Sistemas financieros.
- Sistemas gubernamentales.
- Grandes almacenes de datos.

---

# 12. Rendimiento

La diferencia de rendimiento es prácticamente imperceptible.

La elección depende del tamaño esperado de los datos.

---

# 13. Caso Empresarial

```csharp
long registrosHistoricos =
    contexto.Logs
        .LongCount();
```

Puede utilizarse para conocer el número total de eventos almacenados.

---

# 14. Resumen

`LongCount()` cuenta elementos de una secuencia devolviendo un valor de tipo `Int64`.

Características:

- Similar a Count.
- Retorna long.
- Soporta cantidades extremadamente grandes.
- Compatible con Entity Framework.
- Ejecución inmediata.
- Equivalente a COUNT_BIG en SQL Server.
