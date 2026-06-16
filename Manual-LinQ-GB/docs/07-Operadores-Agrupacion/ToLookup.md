# ToLookup

## 1. Introducción

`ToLookup()` es un operador de agrupación de LINQ que crea una estructura de datos indexada basada en una clave.

Es similar a:

```csharp
GroupBy()
```

pero genera inmediatamente una colección especializada de tipo:

```csharp
ILookup<TKey,TElement>
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. ¿Qué Hace ToLookup?

Agrupa elementos según una clave y crea un índice para acceder rápidamente a cada grupo.

Ejemplo:

```text
Ventas
│
├─ Quito
├─ Quito
├─ Cuenca
├─ Guayaquil
```

Resultado:

```text
Quito
 ├─ Venta1
 └─ Venta2

Cuenca
 └─ Venta3

Guayaquil
 └─ Venta4
```

---

# 3. Sintaxis

```csharp
ToLookup(
    keySelector
)
```

---

# 4. Ejemplo Básico

```csharp
var lookup =
    empleados.ToLookup(
        e => e.Departamento
    );
```

---

# 5. Acceso Directo

```csharp
foreach(var empleado in lookup["Ventas"])
{
    Console.WriteLine(
        empleado.Nombre
    );
}
```

---

# 6. Arquitectura Interna

```text
Colección
      │
      ▼
 ToLookup()
      │
      ▼
 Crear Índice
      │
      ▼
 ILookup
```

---

# 7. GroupBy vs ToLookup

## GroupBy

```csharp
GroupBy()
```

Ejecución diferida.

---

## ToLookup

```csharp
ToLookup()
```

Ejecución inmediata.

---

# 8. Clase de Ejemplo

```csharp
public class Empleado
{
    public string Nombre { get; set; }
    public string Departamento { get; set; }
}
```

---

# 9. Crear Lookup

```csharp
var lookup =
    empleados.ToLookup(
        e => e.Departamento
    );
```

---

# 10. Buscar Grupo

```csharp
var ventas =
    lookup["Ventas"];
```

---

# 11. Verificar Existencia

```csharp
bool existe =
    lookup.Contains(
        "Ventas"
    );
```

---

# 12. Caso Empresarial

Productos por categoría:

```csharp
var categorias =
    productos.ToLookup(
        p => p.Categoria
    );
```

Clientes por ciudad:

```csharp
var ciudades =
    clientes.ToLookup(
        c => c.Ciudad
    );
```

---

# 13. Ventajas

- Acceso rápido.
- Búsqueda eficiente.
- Ideal para cachés.
- Agrupaciones reutilizables.

---

# 14. Diferencia con Dictionary

Dictionary:

```csharp
1 clave = 1 valor
```

Lookup:

```csharp
1 clave = muchos valores
```

---

# 15. Resumen

`ToLookup()` crea una colección indexada y agrupada.

Características:

- Similar a GroupBy.
- Genera ILookup.
- Ejecución inmediata.
- Acceso rápido por clave.
- Muy útil para búsquedas frecuentes.
