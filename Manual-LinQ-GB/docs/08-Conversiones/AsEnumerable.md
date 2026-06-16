# AsEnumerable

## 1. Introducción

`AsEnumerable()` es un operador de conversión que transforma una secuencia para que sea tratada como:

```csharp
IEnumerable<T>
```

Pertenece al namespace:

```csharp
System.Linq
```

---

# 2. ¿Qué Hace AsEnumerable?

Convierte una consulta para que LINQ continúe procesándola en memoria utilizando LINQ to Objects.

---

# 3. Sintaxis

```csharp
AsEnumerable()
```

---

# 4. Ejemplo Básico

```csharp
var consulta =
    numeros.AsEnumerable();
```

Resultado:

```text
IEnumerable<T>
```

---

# 5. ¿Por Qué Existe?

Algunos métodos no pueden traducirse a SQL.

Ejemplo:

```csharp
MiMetodoPersonalizado()
```

Entity Framework no sabe convertirlo.

---

# 6. Solución

```csharp
var resultado =
    contexto.Clientes
        .AsEnumerable()
        .Where(
            c => MiMetodo(c.Nombre)
        );
```

Después de:

```csharp
AsEnumerable()
```

el procesamiento ocurre en memoria.

---

# 7. Arquitectura Interna

```text
Base de Datos
      │
      ▼
 IQueryable
      │
      ▼
AsEnumerable()
      │
      ▼
 IEnumerable
      │
      ▼
 LINQ to Objects
```

---

# 8. IQueryable vs IEnumerable

## IQueryable

```csharp
Consulta traducible a SQL
```

---

## IEnumerable

```csharp
Procesamiento local
```

---

# 9. Entity Framework

```csharp
var clientes =
    contexto.Clientes
        .AsEnumerable();
```

A partir de este punto:

```text
LINQ trabaja en memoria
```

---

# 10. Caso Empresarial

Aplicar lógica personalizada:

```csharp
var resultado =
    contexto.Productos
        .AsEnumerable()
        .Where(
            p => ValidarProducto(p)
        );
```

---

# 11. Cuándo Utilizarlo

Cuando:

- SQL no puede traducir una operación.
- Se necesita lógica personalizada.
- Se requiere trabajar localmente.

---

# 12. Precaución

Evitar:

```csharp
contexto.Clientes
    .AsEnumerable()
```

sobre millones de registros.

Porque:

```text
Carga todos los datos en memoria.
```

---

# 13. Ventajas

- Flexibilidad.
- Compatibilidad con métodos personalizados.
- Permite lógica compleja.

---

# 14. Desventajas

- Mayor consumo de memoria.
- Menor rendimiento en grandes volúmenes.

---

# 15. Ejemplos Reales

```csharp
var resultado =
    contexto.Empleados
        .AsEnumerable()
        .Where(
            e => CalcularBonus(e)
        );
```

```csharp
var clientes =
    contexto.Clientes
        .AsEnumerable()
        .Select(
            c => FormatearNombre(c)
        );
```

---

# 16. Resumen

`AsEnumerable()` convierte una secuencia para que LINQ la procese como IEnumerable.

Características:

- Cambia de IQueryable a IEnumerable.
- Permite lógica local.
- Muy utilizado con Entity Framework.
- Debe usarse cuidadosamente con grandes volúmenes de datos.
