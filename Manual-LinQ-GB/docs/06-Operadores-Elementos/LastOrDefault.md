# LastOrDefault

## 1. Introducción

`LastOrDefault()` es un operador de elementos de LINQ que devuelve el último elemento de una secuencia.

Si la secuencia está vacía o no existe ningún elemento que cumpla la condición especificada, devuelve el valor predeterminado del tipo en lugar de generar una excepción.

Pertenece al namespace:

```csharp
System.Linq
```

Es el equivalente seguro de:

```csharp
Last()
```

---

# 2. ¿Por Qué Existe?

`Last()` genera una excepción cuando no encuentra elementos.

Ejemplo:

```csharp
var resultado =
    numeros.Last();
```

Resultado:

```text
InvalidOperationException
```

Para evitar esto existe:

```csharp
LastOrDefault()
```

---

# 3. Sintaxis

## Obtener último elemento

```csharp
LastOrDefault()
```

## Obtener último elemento con condición

```csharp
LastOrDefault(
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

int ultimo =
    numeros.LastOrDefault();
```

Resultado:

```text
40
```

---

# 5. Colección Vacía

```csharp
List<int> numeros = [];

int resultado =
    numeros.LastOrDefault();
```

Resultado:

```text
0
```

Porque:

```csharp
default(int)
```

es:

```text
0
```

---

# 6. Arquitectura Interna

```text
Colección
      │
      ▼
LastOrDefault()
      │
      ▼
¿Existe Elemento?
      │
 ┌────┴────┐
 │         │
 ▼         ▼
Sí       No
 │         │
 ▼         ▼
Último   Default(T)
```

---

# 7. Valores Default

## int

```csharp
0
```

---

## double

```csharp
0.0
```

---

## bool

```csharp
false
```

---

## string

```csharp
null
```

---

## objetos

```csharp
null
```

---

# 8. LastOrDefault con Condición

```csharp
int resultado =
    numeros.LastOrDefault(
        n => n > 15
    );
```

Resultado:

```text
40
```

---

# 9. Cuando No Encuentra Coincidencias

```csharp
int resultado =
    numeros.LastOrDefault(
        n => n > 100
    );
```

Resultado:

```text
0
```

---

# 10. LastOrDefault con Objetos

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
    clientes.LastOrDefault(
        c => c.Id == 500
    );
```

Resultado:

```text
null
```

---

# 11. Validación Segura

```csharp
var cliente =
    clientes.LastOrDefault(
        c => c.Id == id
    );

if(cliente != null)
{
    Console.WriteLine(
        cliente.Nombre
    );
}
```

---

# 12. Entity Framework

Consulta:

```csharp
var venta =
    contexto.Ventas
        .OrderBy(
            v => v.Id
        )
        .LastOrDefault();
```

SQL aproximado:

```sql
SELECT TOP(1) *
FROM Ventas
ORDER BY Id DESC
```

---

# 13. Caso Empresarial

Última venta registrada:

```csharp
var venta =
    contexto.Ventas
        .OrderBy(
            v => v.Fecha
        )
        .LastOrDefault();
```

Última factura:

```csharp
var factura =
    facturas.LastOrDefault();
```

Último usuario conectado:

```csharp
var usuario =
    usuarios.LastOrDefault();
```

---

# 14. Diferencia entre Last y LastOrDefault

## Last

```csharp
clientes.Last();
```

Si no existe:

```text
Excepción
```

---

## LastOrDefault

```csharp
clientes.LastOrDefault();
```

Si no existe:

```text
Valor por defecto
```

---

# 15. Rendimiento

- Similar a Last().
- Compatible con SQL.
- Muy utilizado en Entity Framework.
- Más seguro para aplicaciones empresariales.

---

# 16. Ejemplos Reales

Última compra:

```csharp
var compra =
    compras.LastOrDefault();
```

Último log:

```csharp
var log =
    logs.LastOrDefault();
```

Última notificación:

```csharp
var notificacion =
    notificaciones.LastOrDefault();
```

---

# 17. Resumen

`LastOrDefault()` devuelve el último elemento de una secuencia o el valor predeterminado si no existe.

Características:

- Versión segura de Last().
- No genera excepciones por ausencia de resultados.
- Compatible con Entity Framework.
- Ejecución inmediata.
- Muy utilizado en sistemas empresariales.
