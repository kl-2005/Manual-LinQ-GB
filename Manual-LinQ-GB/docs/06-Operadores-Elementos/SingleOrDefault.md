# SingleOrDefault

## 1. Introducción

`SingleOrDefault()` es un operador de elementos de LINQ que devuelve el único elemento de una secuencia que cumple una condición determinada.

Si no encuentra ningún elemento, devuelve el valor predeterminado del tipo.

Sin embargo, si encuentra más de un elemento, genera una excepción.

Pertenece al namespace:

```csharp
System.Linq
```

Es la versión segura de:

```csharp
Single()
```

---

# 2. ¿Qué Hace SingleOrDefault?

Devuelve:

```text
El único elemento encontrado
```

Si no existe:

```text
Valor predeterminado (Default)
```

Si existen varios:

```text
Excepción
```

---

# 3. Casos Posibles

| Coincidencias | Resultado |
|---------------|------------|
| 0 | Default(T) |
| 1 | Elemento |
| Más de 1 | Excepción |

---

# 4. Sintaxis

## Único elemento

```csharp
SingleOrDefault()
```

## Único elemento con condición

```csharp
SingleOrDefault(
    Func<TSource,bool> predicate
)
```

---

# 5. Ejemplo Básico

```csharp
List<int> numeros =
[
    10
];

int resultado =
    numeros.SingleOrDefault();
```

Resultado:

```text
10
```

---

# 6. Sin Coincidencias

```csharp
int resultado =
    numeros.SingleOrDefault(
        n => n == 100
    );
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

# 7. Arquitectura Interna

```text
Colección
      │
      ▼
SingleOrDefault()
      │
      ▼
Contar Coincidencias
      │
 ┌────┼────┐
 │    │    │
 ▼    ▼    ▼
0    1    >1
 │    │    │
 ▼    ▼    ▼
Default Elemento Error
```

---

# 8. Valores Predeterminados

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

# 9. Múltiples Coincidencias

```csharp
List<int> numeros =
[
    20,
    20,
    20
];

var resultado =
    numeros.SingleOrDefault(
        n => n == 20
    );
```

Resultado:

```text
InvalidOperationException
```

Mensaje:

```text
Sequence contains more than one matching element
```

---

# 10. SingleOrDefault con Objetos

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
    clientes.SingleOrDefault(
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
    clientes.SingleOrDefault(
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
var usuario =
    contexto.Usuarios
        .SingleOrDefault(
            u => u.Email == email
        );
```

SQL aproximado:

```sql
SELECT *
FROM Usuarios
WHERE Email = @Email
```

Posteriormente EF valida que no existan múltiples registros.

---

# 13. Caso Empresarial

Buscar usuario por correo:

```csharp
var usuario =
    contexto.Usuarios
        .SingleOrDefault(
            u => u.Email == email
        );
```

Buscar empleado:

```csharp
var empleado =
    empleados.SingleOrDefault(
        e => e.Codigo == codigo
    );
```

Buscar factura:

```csharp
var factura =
    facturas.SingleOrDefault(
        f => f.Numero ==
             numeroFactura
    );
```

---

# 14. Diferencia entre FirstOrDefault y SingleOrDefault

## FirstOrDefault

```csharp
usuarios.FirstOrDefault(
    u => u.Activo
);
```

Devuelve el primero.

No importa si existen varios.

---

## SingleOrDefault

```csharp
usuarios.SingleOrDefault(
    u => u.Activo
);
```

Exige que exista solamente uno.

---

# 15. Diferencia entre Single y SingleOrDefault

## Single

```csharp
usuarios.Single(
    u => u.Id == id
);
```

Sin resultados:

```text
Excepción
```

---

## SingleOrDefault

```csharp
usuarios.SingleOrDefault(
    u => u.Id == id
);
```

Sin resultados:

```text
Default(T)
```

---

# 16. Cuándo Utilizarlo

Cuando:

- El dato debería ser único.
- Puede no existir.
- Se desea evitar excepciones por ausencia de registros.

Ejemplos:

- Usuario por correo.
- Cliente por cédula.
- Factura por número.
- Producto por código.

---

# 17. Rendimiento

Similar a:

```csharp
Single()
```

Debe verificar que no existan múltiples coincidencias.

---

# 18. Resumen

`SingleOrDefault()` devuelve un único elemento o el valor predeterminado si no existe.

Características:

- Garantiza unicidad.
- No falla cuando no hay resultados.
- Falla si existen varios resultados.
- Compatible con Entity Framework.
- Ejecución inmediata.
- Ideal para búsquedas únicas opcionales.
