# FirstOrDefault

## 1. Introducción

`FirstOrDefault()` es un operador de elementos de LINQ que devuelve el primer elemento de una secuencia.

Si la secuencia está vacía o ningún elemento cumple la condición especificada, devuelve el valor predeterminado del tipo en lugar de lanzar una excepción.

Pertenece al namespace:

```csharp
System.Linq
```

Es uno de los métodos más utilizados en:

- Entity Framework Core
- LINQ to Objects
- APIs REST
- Aplicaciones empresariales

---

# 2. ¿Por Qué Existe?

`First()` genera una excepción cuando no encuentra resultados.

Ejemplo:

```csharp
var cliente =
    clientes.First(
        c => c.Id == 100
    );
```

Resultado:

```text
InvalidOperationException
```

Para evitarlo se utiliza:

```csharp
FirstOrDefault()
```

---

# 3. Sintaxis

## Obtener primer elemento

```csharp
FirstOrDefault()
```

## Obtener primer elemento con condición

```csharp
FirstOrDefault(
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
    30
];

int resultado =
    numeros.FirstOrDefault();
```

Resultado:

```text
10
```

---

# 5. Colección Vacía

```csharp
List<int> numeros = [];

int resultado =
    numeros.FirstOrDefault();
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
FirstOrDefault()
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
Elemento  Default(T)
```

---

# 7. Tipos de Valores Default

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

# 8. FirstOrDefault con Condición

```csharp
int numero =
    numeros.FirstOrDefault(
        n => n > 15
    );
```

Resultado:

```text
20
```

---

# 9. Cuando No Encuentra Resultados

```csharp
int numero =
    numeros.FirstOrDefault(
        n => n > 100
    );
```

Resultado:

```text
0
```

---

# 10. FirstOrDefault con Objetos

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
    clientes.FirstOrDefault(
        c => c.Id == 100
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
    clientes.FirstOrDefault(
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
var cliente =
    contexto.Clientes
        .FirstOrDefault(
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

# 13. Caso Empresarial

Buscar cliente:

```csharp
var cliente =
    contexto.Clientes
        .FirstOrDefault(
            c => c.Cedula == cedula
        );
```

Buscar producto:

```csharp
var producto =
    contexto.Productos
        .FirstOrDefault(
            p => p.Codigo == codigo
        );
```

---

# 14. Diferencia entre First y FirstOrDefault

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
Valor por defecto
```

---

# 15. Rendimiento

- Muy eficiente.
- Se detiene al encontrar el primer resultado.
- Compatible con SQL.
- Ideal para búsquedas únicas.

---

# 16. Ejemplos Reales

Primer usuario administrador:

```csharp
var admin =
    usuarios.FirstOrDefault(
        u => u.Rol == "Admin"
    );
```

Primer producto disponible:

```csharp
var producto =
    productos.FirstOrDefault(
        p => p.Stock > 0
    );
```

---

# 17. Resumen

`FirstOrDefault()` devuelve el primer elemento de una secuencia o el valor predeterminado si no encuentra resultados.

Características:

- No lanza excepciones.
- Muy utilizado con Entity Framework.
- Compatible con consultas SQL.
- Ejecución inmediata.
- Ideal para búsquedas seguras.
