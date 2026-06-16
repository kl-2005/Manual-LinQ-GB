# Performance Tips

## 1. Introducción

LINQ puede ser muy eficiente, pero también puede generar problemas de rendimiento si se utiliza incorrectamente.

---

## 2. Evitar ToList Temprano

Incorrecto:

```csharp
var datos =
    contexto.Clientes
        .ToList()
        .Where(c => c.Activo);
```

Correcto:

```csharp
var datos =
    contexto.Clientes
        .Where(c => c.Activo)
        .ToList();
```

---

## 3. Utilizar Proyecciones

Incorrecto:

```csharp
var clientes =
    contexto.Clientes.ToList();
```

Correcto:

```csharp
var clientes =
    contexto.Clientes
        .Select(
            c => new
            {
                c.Id,
                c.Nombre
            }
        );
```

---

## 4. Utilizar Any en Lugar de Count

Incorrecto:

```csharp
if(clientes.Count() > 0)
```

Correcto:

```csharp
if(clientes.Any())
```

---

## 5. Evitar Múltiples Enumeraciones

Incorrecto:

```csharp
consulta.Count();
consulta.First();
```

---

## 6. Utilizar AsNoTracking

```csharp
contexto.Clientes
    .AsNoTracking();
```

---

## 7. Paginación

```csharp
Skip()
```

```csharp
Take()
```

---

## 8. Evitar Consultas N+1

Utilizar:

```csharp
Include()
```

---

## 9. Consultas Asíncronas

```csharp
ToListAsync()
```

---

## 10. Resumen

Buenas prácticas:

- Usar Select.
- Usar Any.
- Usar AsNoTracking.
- Usar paginación.
- Evitar ToList prematuro.
- Utilizar consultas asíncronas.
