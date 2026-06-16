# Async LINQ

## 1. Introducción

Async LINQ permite ejecutar consultas asincrónicas.

Muy utilizado con:

```text
Entity Framework Core
```

---

## 2. Problema

Consulta tradicional:

```csharp
var clientes =
    contexto.Clientes.ToList();
```

Bloquea el hilo.

---

## 3. Solución

```csharp
var clientes =
    await contexto.Clientes
        .ToListAsync();
```

---

## 4. Namespace

```csharp
Microsoft.EntityFrameworkCore
```

---

## 5. Métodos Asíncronos

```csharp
ToListAsync()
```

```csharp
FirstAsync()
```

```csharp
FirstOrDefaultAsync()
```

```csharp
SingleAsync()
```

```csharp
AnyAsync()
```

```csharp
CountAsync()
```

---

## 6. Ejemplo

```csharp
var cliente =
    await contexto.Clientes
        .FirstOrDefaultAsync(
            c => c.Id == 1
        );
```

---

## 7. Beneficios

- Escalabilidad.
- Mejor rendimiento.
- Menor bloqueo.

---

## 8. Caso Empresarial

APIs Web.

Microservicios.

Aplicaciones Cloud.

---

## 9. Resumen

Async LINQ permite consultas no bloqueantes y es esencial en aplicaciones modernas.
