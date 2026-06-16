# Contains

## 1. Introducción

`Contains()` es un operador cuantificador de LINQ que permite determinar si una secuencia contiene un valor específico.

Es equivalente conceptualmente a:

```sql
IN
```

o a una búsqueda de existencia de un valor dentro de una colección.

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. ¿Qué Hace Contains?

Responde la pregunta:

```text
¿La colección contiene este valor?
```

Devuelve:

```csharp
true
```

si el valor existe.

Devuelve:

```csharp
false
```

si el valor no existe.

---

# 3. Sintaxis

```csharp
Contains(valor)
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

bool existe =
    numeros.Contains(20);
```

Resultado:

```text
True
```

---

# 5. Valor No Encontrado

```csharp
bool existe =
    numeros.Contains(100);
```

Resultado:

```text
False
```

---

# 6. Arquitectura Interna

```text
Colección
      │
      ▼
Contains(Valor)
      │
      ▼
Buscar Valor
      │
 ┌────┴────┐
 │         │
 ▼         ▼
Existe   No Existe
 │         │
 ▼         ▼
True     False
```

---

# 7. Contains con Strings

```csharp
List<string> nombres =
[
    "Ana",
    "Carlos",
    "Luis"
];

bool existe =
    nombres.Contains(
        "Carlos"
    );
```

Resultado:

```text
True
```

---

# 8. Contains con Objetos Simples

```csharp
List<int> edades =
[
    18,
    25,
    30
];

bool existe =
    edades.Contains(25);
```

Resultado:

```text
True
```

---

# 9. Contains con Objetos Complejos

Clase:

```csharp
public class Cliente
{
    public int Id { get; set; }
    public string Nombre { get; set; }
}
```

Colección:

```csharp
Cliente cliente =
    new Cliente
    {
        Id = 1,
        Nombre = "Juan"
    };

bool existe =
    clientes.Contains(cliente);
```

Por defecto se compara la referencia del objeto.

---

# 10. Contains y Equals

Para comparar objetos correctamente suele sobrescribirse:

```csharp
Equals()
```

y

```csharp
GetHashCode()
```

o implementar:

```csharp
IEquatable<T>
```

---

# 11. Contains con Any

Muchas veces se puede reemplazar por:

```csharp
clientes.Any(
    c => c.Id == 1
);
```

---

## Contains

```csharp
ids.Contains(1);
```

---

## Any

```csharp
clientes.Any(
    c => c.Id == 1
);
```

---

# 12. Entity Framework

Consulta:

```csharp
bool existe =
    contexto.Clientes
        .Select(
            c => c.Id
        )
        .Contains(5);
```

SQL aproximado:

```sql
SELECT CASE
WHEN EXISTS
(
    SELECT *
    FROM Clientes
    WHERE Id = 5
)
THEN 1
ELSE 0
END
```

---

# 13. Contains como IN de SQL

Lista:

```csharp
List<int> ids =
[
    1,
    3,
    5
];
```

Consulta:

```csharp
var clientes =
    contexto.Clientes
        .Where(
            c => ids.Contains(c.Id)
        );
```

SQL aproximado:

```sql
SELECT *
FROM Clientes
WHERE Id IN (1,3,5)
```

Esta es una de las aplicaciones más utilizadas en Entity Framework.

---

# 14. Caso Empresarial

Validar rol:

```csharp
bool administrador =
    roles.Contains(
        "Administrador"
    );
```

Validar producto:

```csharp
bool existe =
    codigos.Contains(
        codigoBuscado
    );
```

Validar usuario:

```csharp
bool autorizado =
    usuariosPermitidos.Contains(
        usuario
    );
```

---

# 15. Rendimiento

Depende de la colección.

## List

```csharp
List<T>
```

Búsqueda lineal:

```text
O(n)
```

---

## HashSet

```csharp
HashSet<T>
```

Búsqueda optimizada:

```text
O(1)
```

---

# 16. Diferencia entre Any y Contains

## Any

```csharp
clientes.Any(
    c => c.Id == 1
);
```

Utiliza una condición.

---

## Contains

```csharp
ids.Contains(1);
```

Busca un valor específico.

---

# 17. Ejemplos Reales

Verificar ciudad:

```csharp
bool existe =
    ciudades.Contains(
        "Quito"
    );
```

Verificar categoría:

```csharp
bool categoriaValida =
    categorias.Contains(
        categoria
    );
```

Verificar permisos:

```csharp
bool acceso =
    permisos.Contains(
        permisoActual
    );
```

---

# 18. Resumen

`Contains()` verifica si una secuencia contiene un valor determinado.

Características:

- Operador cuantificador.
- Devuelve true o false.
- Similar a IN de SQL.
- Compatible con Entity Framework.
- Ejecución inmediata.
- Muy utilizado en filtros y validaciones.
- Funciona especialmente bien con HashSet para búsquedas rápidas.
