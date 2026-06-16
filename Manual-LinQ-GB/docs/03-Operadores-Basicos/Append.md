# Append

## 1. Introducción

`Append()` es un operador de LINQ que permite agregar un elemento al final de una secuencia sin modificar la colección original.

Fue introducido en:

```text
.NET Core 1.0
.NET Framework 4.7.1+
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. Sintaxis

```csharp
Append(TSource element)
```

Donde:

- `TSource` = Tipo de dato de la colección.
- `element` = Elemento que será agregado al final.

---

# 3. ¿Cómo Funciona?

Colección original:

```csharp
List<int> numeros =
[
    1,
    2,
    3
];
```

Consulta:

```csharp
var resultado =
    numeros.Append(4);
```

Resultado:

```text
1
2
3
4
```

---

# 4. Arquitectura Interna

```text
Colección
    │
    ▼
 Append()
    │
    ▼
Agregar al Final
    │
    ▼
Nueva Secuencia
```

---

# 5. Ejemplo Básico

```csharp
List<int> numeros =
[
    10,
    20,
    30
];

var resultado =
    numeros.Append(40);

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

# 6. La Colección Original No Cambia

```csharp
List<int> numeros =
[
    1,
    2,
    3
];

var nuevaLista =
    numeros.Append(4);
```

Colección original:

```text
1
2
3
```

Nueva secuencia:

```text
1
2
3
4
```

---

# 7. Append con Strings

```csharp
List<string> nombres =
[
    "Ana",
    "Carlos"
];

var resultado =
    nombres.Append("Gabriel");
```

Resultado:

```text
Ana
Carlos
Gabriel
```

---

# 8. Append con Objetos

Entidad:

```csharp
public class Cliente
{
    public int Id { get; set; }

    public string Nombre { get; set; }
}
```

Uso:

```csharp
var resultado =
    clientes.Append(
        new Cliente
        {
            Id = 100,
            Nombre = "Nuevo Cliente"
        }
    );
```

---

# 9. Deferred Execution

Append utiliza ejecución diferida.

```csharp
var consulta =
    numeros.Append(5);
```

En este punto:

```text
No se ejecuta todavía
```

La ejecución ocurre cuando:

```csharp
consulta.ToList();
```

o

```csharp
foreach(var item in consulta)
{
}
```

---

# 10. Append en IEnumerable

```csharp
IEnumerable<int> resultado =
    numeros.Append(100);
```

La operación se realiza en memoria RAM.

---

# 11. Encadenamiento de Append

```csharp
var resultado =
    numeros
        .Append(4)
        .Append(5)
        .Append(6);
```

Resultado:

```text
1
2
3
4
5
6
```

---

# 12. Combinación con Where

```csharp
var resultado =
    numeros
        .Where(n => n > 2)
        .Append(100);
```

Resultado:

```text
3
4
5
100
```

---

# 13. Combinación con OrderBy

```csharp
var resultado =
    numeros
        .OrderBy(n => n)
        .Append(999);
```

Resultado:

```text
1
2
3
4
999
```

---

# 14. Diferencia Entre Append y Add

## Add()

```csharp
numeros.Add(4);
```

Modifica la colección original.

---

## Append()

```csharp
numeros.Append(4);
```

No modifica la colección original.

Genera una nueva secuencia.

---

# 15. Caso Empresarial

Supongamos una lista de productos:

```csharp
var productos =
    contexto.Productos
            .Where(p => p.Activo);
```

Agregar un registro temporal:

```csharp
var resultado =
    productos.Append(
        new Producto
        {
            Nombre = "Producto Promocional"
        }
    );
```

---

# 16. Ventajas

## Inmutabilidad

No modifica la colección original.

---

## Fácil Lectura

Expresa claramente la intención.

---

## Encadenable

Compatible con otros operadores LINQ.

---

## Deferred Execution

Optimiza recursos.

---

# 17. Errores Comunes

Incorrecto:

```csharp
numeros.Append(4);

Console.WriteLine(
    numeros.Count
);
```

Resultado:

```text
3
```

Porque Append no modifica la colección.

---

Correcto:

```csharp
numeros =
    numeros.Append(4)
           .ToList();
```

---

# 18. Diferencia Entre Append y Prepend

| Método | Acción |
|----------|----------|
| Append | Agrega al final |
| Prepend | Agrega al inicio |

Ejemplo:

```csharp
numeros.Append(10);
```

↓

```text
1 2 3 10
```

Ejemplo:

```csharp
numeros.Prepend(10);
```

↓

```text
10 1 2 3
```

---

# 19. Flujo Completo

```text
Colección
      │
      ▼
Append()
      │
      ▼
Agregar Elemento
      │
      ▼
Nueva Secuencia
      │
      ▼
Resultado
```

---

# 20. Resumen

`Append()` permite agregar un elemento al final de una secuencia LINQ sin modificar la colección original.

Características principales:

- Agrega elementos al final.
- Implementa Deferred Execution.
- Funciona sobre IEnumerable.
- Mantiene la inmutabilidad.
- Permite encadenamiento.
- Compatible con otros operadores LINQ.
- Genera una nueva secuencia.

En aplicaciones empresariales, Append suele utilizarse para agregar elementos temporales, registros calculados o información adicional antes de mostrar resultados al usuario.
