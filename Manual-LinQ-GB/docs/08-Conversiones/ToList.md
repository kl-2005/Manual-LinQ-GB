# ToList

## 1. Introducción

`ToList()` es un operador de conversión de LINQ que transforma una secuencia en una colección de tipo:

```csharp
List<T>
```

Pertenece al namespace:

```csharp
System.Linq
```

Es probablemente el método de materialización más utilizado en LINQ y Entity Framework.

---

# 2. ¿Qué Hace ToList?

Convierte:

```csharp
IEnumerable<T>
```

en:

```csharp
List<T>
```

Ejemplo:

```csharp
var consulta =
    numeros.Where(
        n => n > 10
    );

List<int> resultado =
    consulta.ToList();
```

---

# 3. Sintaxis

```csharp
ToList()
```

---

# 4. Ejemplo Básico

```csharp
int[] numeros =
[
    10,
    20,
    30
];

List<int> lista =
    numeros.ToList();
```

Resultado:

```text
List<int>
```

---

# 5. Arquitectura Interna

```text
IEnumerable
      │
      ▼
  ToList()
      │
      ▼
Recorrer Datos
      │
      ▼
 Crear List<T>
```

---

# 6. Materialización

Antes:

```csharp
var consulta =
    clientes.Where(
        c => c.Activo
    );
```

Después:

```csharp
var lista =
    consulta.ToList();
```

La consulta se ejecuta inmediatamente.

---

# 7. Entity Framework

```csharp
var clientes =
    contexto.Clientes
        .Where(
            c => c.Activo
        )
        .ToList();
```

SQL aproximado:

```sql
SELECT *
FROM Clientes
WHERE Activo = 1
```

---

# 8. Deferred Execution

Sin ToList:

```csharp
var consulta =
    productos.Where(
        p => p.Stock > 0
    );
```

No ejecuta.

Con ToList:

```csharp
var lista =
    consulta.ToList();
```

Ejecuta inmediatamente.

---

# 9. Caso Empresarial

```csharp
var empleados =
    contexto.Empleados
        .ToList();
```

```csharp
var ventas =
    contexto.Ventas
        .ToList();
```

---

# 10. Ventajas

- Fácil de usar.
- Muy común.
- Compatible con EF Core.
- Permite recorrer varias veces la colección.

---

# 11. Desventajas

- Consume memoria.
- Carga todos los registros.

---

# 12. Resumen

`ToList()` convierte una secuencia en una lista.

Características:

- Devuelve List<T>.
- Ejecución inmediata.
- Materializa consultas.
- Muy utilizado en Entity Framework.
