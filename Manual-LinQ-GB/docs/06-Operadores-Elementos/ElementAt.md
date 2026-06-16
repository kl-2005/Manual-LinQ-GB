# ElementAt

## 1. Introducción

`ElementAt()` es un operador de elementos de LINQ que permite obtener un elemento específico de una secuencia utilizando su índice.

Pertenece al namespace:

```csharp
System.Linq
```

Funciona de manera similar a acceder a una posición de un arreglo o lista.

---

# 2. ¿Qué Hace ElementAt?

Devuelve:

```text
El elemento ubicado en una posición específica.
```

Ejemplo:

```text
Índice:   0   1   2   3
Valor :  10  20  30  40
```

Consulta:

```csharp
ElementAt(2)
```

Resultado:

```text
30
```

---

# 3. Sintaxis

```csharp
ElementAt(int index)
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

int resultado =
    numeros.ElementAt(2);

Console.WriteLine(resultado);
```

Resultado:

```text
30
```

---

# 5. Arquitectura Interna

```text
Colección
      │
      ▼
ElementAt(2)
      │
      ▼
Posición 2
      │
      ▼
Resultado
```

---

# 6. Indexación

```text
Posición    Valor

0 --------> 10
1 --------> 20
2 --------> 30
3 --------> 40
```

---

# 7. ElementAt con Strings

```csharp
List<string> nombres =
[
    "Ana",
    "Luis",
    "Carlos"
];

string nombre =
    nombres.ElementAt(1);
```

Resultado:

```text
Luis
```

---

# 8. ElementAt con Objetos

Clase:

```csharp
public class Producto
{
    public string Nombre { get; set; }
}
```

Consulta:

```csharp
var producto =
    productos.ElementAt(0);
```

Resultado:

```text
Primer producto de la colección
```

---

# 9. Índice Fuera de Rango

```csharp
List<int> numeros =
[
    10,
    20,
    30
];

var resultado =
    numeros.ElementAt(10);
```

Resultado:

```text
ArgumentOutOfRangeException
```

---

# 10. Entity Framework

Consulta:

```csharp
var cliente =
    contexto.Clientes
        .OrderBy(c => c.Id)
        .ElementAt(4);
```

SQL aproximado:

```sql
SELECT *
FROM Clientes
ORDER BY Id
OFFSET 4 ROWS
FETCH NEXT 1 ROW ONLY
```

---

# 11. Caso Empresarial

Obtener quinto cliente:

```csharp
var cliente =
    clientes.ElementAt(4);
```

Obtener tercer producto:

```csharp
var producto =
    productos.ElementAt(2);
```

Obtener décima factura:

```csharp
var factura =
    facturas.ElementAt(9);
```

---

# 12. Diferencia con First

## First

```csharp
clientes.First();
```

Resultado:

```text
Primer elemento
```

---

## ElementAt

```csharp
clientes.ElementAt(3);
```

Resultado:

```text
Cuarto elemento
```

---

# 13. Rendimiento

En:

```csharp
List<T>
```

es muy rápido.

En:

```csharp
IEnumerable<T>
```

puede requerir recorrer elementos anteriores.

---

# 14. Ejemplos Reales

Obtener segundo estudiante:

```csharp
var estudiante =
    estudiantes.ElementAt(1);
```

Obtener quinto empleado:

```csharp
var empleado =
    empleados.ElementAt(4);
```

Obtener tercer pedido:

```csharp
var pedido =
    pedidos.ElementAt(2);
```

---

# 15. Resumen

`ElementAt()` devuelve el elemento ubicado en una posición específica.

Características:

- Utiliza índices.
- Compatible con Entity Framework.
- Ejecución inmediata.
- Lanza excepción si el índice no existe.
- Similar al acceso por posición de listas y arreglos.
