# All

## 1. Introducción

`All()` es un operador cuantificador de LINQ que permite determinar si todos los elementos de una secuencia cumplen una condición determinada.

Pertenece al namespace:

```csharp
System.Linq
```

A diferencia de:

```csharp
Any()
```

que pregunta:

```text
¿Existe al menos uno?
```

`All()` pregunta:

```text
¿Todos cumplen la condición?
```

---

# 2. Sintaxis

```csharp
All(
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
    40
];
```

Consulta:

```csharp
bool resultado =
    numeros.All(
        n => n > 5
    );
```

Resultado:

```text
True
```

Porque todos los números son mayores que 5.

---

# 4. Arquitectura Interna

```text
Colección
      │
      ▼
    All()
      │
      ▼
¿Cumple Condición?
      │
 ┌────┴────┐
 │         │
 ▼         ▼
Todos   Alguno No
 │         │
 ▼         ▼
True     False
```

---

# 5. Ejemplo Básico

```csharp
List<int> edades =
[
    18,
    21,
    25,
    30
];

bool mayoresEdad =
    edades.All(
        e => e >= 18
    );
```

Resultado:

```text
True
```

---

# 6. Cuando Alguno No Cumple

```csharp
List<int> edades =
[
    18,
    21,
    15,
    30
];

bool mayoresEdad =
    edades.All(
        e => e >= 18
    );
```

Resultado:

```text
False
```

Porque:

```text
15 < 18
```

---

# 7. All con Objetos

Clase:

```csharp
public class Producto
{
    public string Nombre { get; set; }
    public int Stock { get; set; }
}
```

Consulta:

```csharp
bool disponibles =
    productos.All(
        p => p.Stock > 0
    );
```

Resultado:

```text
True si todos tienen stock
```

---

# 8. All con Strings

```csharp
List<string> nombres =
[
    "Ana",
    "Carlos",
    "Luis"
];

bool longitud =
    nombres.All(
        n => n.Length >= 3
    );
```

Resultado:

```text
True
```

---

# 9. Diferencia entre Any y All

## Any

```csharp
clientes.Any(
    c => c.Activo
);
```

Pregunta:

```text
¿Existe alguno activo?
```

---

## All

```csharp
clientes.All(
    c => c.Activo
);
```

Pregunta:

```text
¿Todos están activos?
```

---

# 10. Entity Framework

Consulta:

```csharp
bool todosActivos =
    contexto.Clientes
        .All(
            c => c.Activo
        );
```

EF Core traduce esta operación a SQL equivalente.

---

# 11. Caso Empresarial

Validar inventario:

```csharp
bool inventarioCompleto =
    productos.All(
        p => p.Stock > 0
    );
```

Validar pagos:

```csharp
bool pagosCorrectos =
    facturas.All(
        f => f.Pagada
    );
```

Validar asistencia:

```csharp
bool asistenciaCompleta =
    estudiantes.All(
        e => e.Asistio
    );
```

---

# 12. Rendimiento

All es eficiente porque:

- Se detiene al encontrar el primer elemento que no cumple.
- No necesita recorrer toda la colección si ya encontró un resultado negativo.

Ejemplo:

```csharp
numeros.All(
    n => n > 0
);
```

Si encuentra:

```text
-5
```

finaliza inmediatamente.

---

# 13. Colecciones Vacías

Caso especial:

```csharp
List<int> numeros = [];

bool resultado =
    numeros.All(
        n => n > 0
    );
```

Resultado:

```text
True
```

Esto se conoce como:

```text
Verdad Vacía
(Vacuous Truth)
```

---

# 14. Ejemplos Reales

Todos los empleados activos:

```csharp
bool activos =
    empleados.All(
        e => e.Activo
    );
```

Todos los productos disponibles:

```csharp
bool disponibles =
    productos.All(
        p => p.Stock > 0
    );
```

Todos aprobaron:

```csharp
bool aprobaron =
    estudiantes.All(
        e => e.Nota >= 7
    );
```

---

# 15. Resumen

`All()` verifica si todos los elementos de una secuencia cumplen una condición.

Características:

- Operador cuantificador.
- Complemento lógico de Any.
- Compatible con Entity Framework.
- Ejecución inmediata.
- Muy eficiente.
- Ideal para validaciones globales.
