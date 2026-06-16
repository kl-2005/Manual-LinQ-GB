# Single

## 1. Introducción

`Single()` es un operador de elementos de LINQ que devuelve el único elemento de una secuencia que cumple una condición determinada.

Su característica principal es que exige que exista exactamente un resultado.

Pertenece al namespace:

```csharp
System.Linq
```

Es ampliamente utilizado cuando una regla de negocio garantiza que solo puede existir un registro.

---

# 2. ¿Qué Hace Single?

Devuelve:

```text
Un único elemento
```

Pero únicamente si existe exactamente uno.

---

# 3. Casos Posibles

## Existe uno

```text
Retorna el elemento
```

---

## No existe ninguno

```text
Lanza excepción
```

---

## Existe más de uno

```text
Lanza excepción
```

---

# 4. Sintaxis

## Único elemento

```csharp
Single()
```

## Único elemento con condición

```csharp
Single(
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
    numeros.Single();
```

Resultado:

```text
10
```

---

# 6. Ejemplo con Condición

```csharp
List<int> numeros =
[
    10,
    20,
    30
];

int resultado =
    numeros.Single(
        n => n == 20
    );
```

Resultado:

```text
20
```

---

# 7. Arquitectura Interna

```text
Colección
      │
      ▼
   Single()
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
Error Resultado Error
```

---

# 8. Sin Resultados

```csharp
var resultado =
    numeros.Single(
        n => n == 100
    );
```

Resultado:

```text
InvalidOperationException
```

Mensaje:

```text
Sequence contains no matching element
```

---

# 9. Múltiples Resultados

```csharp
List<int> numeros =
[
    20,
    20,
    20
];

var resultado =
    numeros.Single(
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

# 10. Single con Objetos

Clase:

```csharp
public class Usuario
{
    public int Id { get; set; }
    public string Correo { get; set; }
}
```

Consulta:

```csharp
var usuario =
    usuarios.Single(
        u => u.Correo ==
             "admin@empresa.com"
    );
```

---

# 11. Entity Framework

Consulta:

```csharp
var usuario =
    contexto.Usuarios
        .Single(
            u => u.Id == 5
        );
```

SQL aproximado:

```sql
SELECT *
FROM Usuarios
WHERE Id = 5
```

Posteriormente EF verifica que solo exista un registro.

---

# 12. Caso Empresarial

Buscar usuario por correo:

```csharp
var usuario =
    contexto.Usuarios
        .Single(
            u => u.Email == email
        );
```

Buscar empleado por código:

```csharp
var empleado =
    empleados.Single(
        e => e.Codigo == codigo
    );
```

---

# 13. Diferencia entre First y Single

## First

```csharp
clientes.First(
    c => c.Activo
);
```

Obtiene el primero.

No importa si existen varios.

---

## Single

```csharp
clientes.Single(
    c => c.Activo
);
```

Exige que exista uno y solamente uno.

---

# 14. Diferencia entre Single y SingleOrDefault

## Single

```csharp
usuarios.Single(
    u => u.Id == id
);
```

Si no existe:

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

Si no existe:

```text
Valor por defecto
```

---

# 15. Cuándo Utilizar Single

Cuando la lógica del sistema garantiza unicidad.

Ejemplos:

- Cédula.
- Correo electrónico.
- Código de empleado.
- Identificador único.
- Clave primaria.

---

# 16. Rendimiento

Puede ser ligeramente más costoso que:

```csharp
First()
```

porque necesita verificar que no existan coincidencias adicionales.

---

# 17. Ejemplos Reales

Buscar usuario:

```csharp
var usuario =
    usuarios.Single(
        u => u.Id == 1
    );
```

Buscar factura:

```csharp
var factura =
    facturas.Single(
        f => f.Numero ==
             numeroFactura
    );
```

Buscar estudiante:

```csharp
var estudiante =
    estudiantes.Single(
        e => e.Cedula ==
             cedula
    );
```

---

# 18. Resumen

`Single()` devuelve un único elemento y exige que exista exactamente uno.

Características:

- Garantiza unicidad.
- Lanza excepción si hay cero resultados.
- Lanza excepción si hay más de uno.
- Compatible con Entity Framework.
- Ejecución inmediata.
- Ideal para claves únicas y registros exclusivos.
