# ElementAtOrDefault

## 1. Introducción

`ElementAtOrDefault()` es un operador de elementos de LINQ que devuelve el elemento ubicado en una posición específica de una secuencia.

Si el índice no existe, devuelve el valor predeterminado del tipo en lugar de generar una excepción.

Pertenece al namespace:

```csharp
System.Linq
```

Es la versión segura de:

```csharp
ElementAt()
```

---

# 2. ¿Qué Hace ElementAtOrDefault?

Devuelve:

```text
El elemento del índice solicitado
```

Si el índice no existe:

```text
Valor predeterminado (Default)
```

---

# 3. Sintaxis

```csharp
ElementAtOrDefault(
    int index
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

int resultado =
    numeros.ElementAtOrDefault(2);
```

Resultado:

```text
30
```

---

# 5. Índice Inexistente

```csharp
int resultado =
    numeros.ElementAtOrDefault(100);
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
ElementAtOrDefault()
      │
      ▼
¿Existe Índice?
      │
 ┌────┴────┐
 │         │
 ▼         ▼
Sí       No
 │         │
 ▼         ▼
Elemento Default(T)
```

---

# 7. Valores Predeterminados

## int

```csharp
0
```

---

## bool

```csharp
false
```

---

## double

```csharp
0.0
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

# 8. ElementAtOrDefault con Strings

```csharp
List<string> nombres =
[
    "Ana",
    "Luis"
];

string nombre =
    nombres.ElementAtOrDefault(10);
```

Resultado:

```text
null
```

---

# 9. ElementAtOrDefault con Objetos

```csharp
var producto =
    productos.ElementAtOrDefault(100);
```

Resultado:

```text
null
```

---

# 10. Validación Segura

```csharp
var cliente =
    clientes.ElementAtOrDefault(5);

if(cliente != null)
{
    Console.WriteLine(
        cliente.Nombre
    );
}
```

---

# 11. Entity Framework

Consulta:

```csharp
var cliente =
    contexto.Clientes
        .OrderBy(c => c.Id)
        .ElementAtOrDefault(3);
```

SQL aproximado:

```sql
SELECT *
FROM Clientes
ORDER BY Id
OFFSET 3 ROWS
FETCH NEXT 1 ROW ONLY
```

---

# 12. Caso Empresarial

Obtener quinto cliente:

```csharp
var cliente =
    clientes.ElementAtOrDefault(4);
```

Obtener décimo producto:

```csharp
var producto =
    productos.ElementAtOrDefault(9);
```

Obtener factura específica:

```csharp
var factura =
    facturas.ElementAtOrDefault(15);
```

---

# 13. Diferencia entre ElementAt y ElementAtOrDefault

## ElementAt

```csharp
clientes.ElementAt(100);
```

Resultado:

```text
Excepción
```

---

## ElementAtOrDefault

```csharp
clientes.ElementAtOrDefault(100);
```

Resultado:

```text
Default(T)
```

---

# 14. Rendimiento

- Similar a ElementAt.
- Compatible con SQL.
- Más seguro para aplicaciones empresariales.
- Evita errores por índices inválidos.

---

# 15. Ejemplos Reales

Obtener segundo usuario:

```csharp
var usuario =
    usuarios.ElementAtOrDefault(1);
```

Obtener sexto estudiante:

```csharp
var estudiante =
    estudiantes.ElementAtOrDefault(5);
```

Obtener octavo pedido:

```csharp
var pedido =
    pedidos.ElementAtOrDefault(7);
```

---

# 16. Resumen

`ElementAtOrDefault()` devuelve un elemento por índice o el valor predeterminado si dicho índice no existe.

Características:

- Utiliza índices.
- No genera excepciones por posiciones inválidas.
- Compatible con Entity Framework.
- Ejecución inmediata.
- Ideal para accesos seguros a colecciones.
