# ToDictionary

## 1. Introducción

`ToDictionary()` es un operador de conversión de LINQ que transforma una secuencia en una colección de tipo:

```csharp
Dictionary<TKey,TValue>
```

Permite almacenar datos mediante pares:

```text
Clave → Valor
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. ¿Qué Hace ToDictionary?

Convierte:

```csharp
IEnumerable<T>
```

en:

```csharp
Dictionary<TKey,TValue>
```

---

# 3. Sintaxis

```csharp
ToDictionary(
    keySelector
)
```

o

```csharp
ToDictionary(
    keySelector,
    elementSelector
)
```

---

# 4. Ejemplo Básico

```csharp
List<string> nombres =
[
    "Ana",
    "Luis",
    "Carlos"
];

var diccionario =
    nombres.ToDictionary(
        n => n
    );
```

Resultado:

```text
Clave -> Valor

Ana -> Ana
Luis -> Luis
Carlos -> Carlos
```

---

# 5. Ejemplo con Objetos

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
var clientesDic =
    clientes.ToDictionary(
        c => c.Id,
        c => c.Nombre
    );
```

Resultado:

```text
1 -> Juan
2 -> María
3 -> Pedro
```

---

# 6. Arquitectura Interna

```text
Colección
      │
      ▼
ToDictionary()
      │
      ▼
Generar Claves
      │
      ▼
Dictionary<TKey,TValue>
```

---

# 7. Acceso por Clave

```csharp
string nombre =
    clientesDic[1];
```

Resultado:

```text
Juan
```

---

# 8. Búsqueda Eficiente

```csharp
clientesDic.ContainsKey(1);
```

Complejidad:

```text
O(1)
```

---

# 9. Claves Duplicadas

```csharp
var diccionario =
    clientes.ToDictionary(
        c => c.Ciudad
    );
```

Si existen ciudades repetidas:

```text
Excepción
```

Resultado:

```text
ArgumentException
```

---

# 10. Entity Framework

```csharp
var clientes =
    contexto.Clientes
        .ToDictionary(
            c => c.Id
        );
```

La consulta se ejecuta inmediatamente.

---

# 11. Caso Empresarial

Clientes por Id:

```csharp
var clientes =
    contexto.Clientes
        .ToDictionary(
            c => c.Id
        );
```

Productos por código:

```csharp
var productos =
    contexto.Productos
        .ToDictionary(
            p => p.Codigo
        );
```

---

# 12. Ventajas

- Acceso rápido.
- Ideal para búsquedas.
- Muy utilizado en caché.
- Excelente rendimiento.

---

# 13. Desventajas

- No admite claves duplicadas.
- Consume memoria adicional.

---

# 14. Ejemplos Reales

```csharp
var usuarios =
    usuariosSistema
        .ToDictionary(
            u => u.Email
        );
```

```csharp
var empleados =
    empleadosSistema
        .ToDictionary(
            e => e.Cedula
        );
```

---

# 15. Resumen

`ToDictionary()` convierte una secuencia en un diccionario.

Características:

- Devuelve Dictionary<TKey,TValue>.
- Búsquedas rápidas O(1).
- Ejecución inmediata.
- Compatible con Entity Framework.
- Muy utilizado en aplicaciones empresariales.
