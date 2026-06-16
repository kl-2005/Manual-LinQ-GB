# Expression Visitors

## 1. Introducción

Expression Visitor es un patrón utilizado para recorrer y modificar árboles de expresiones.

Clase base:

```csharp
ExpressionVisitor
```

Namespace:

```csharp
System.Linq.Expressions
```

---

## 2. ¿Por Qué Existe?

Permite analizar:

```csharp
x => x.Edad > 18
```

y modificarla dinámicamente.

---

## 3. Arquitectura

```text
Expression Tree
       │
       ▼
ExpressionVisitor
       │
       ▼
Modificar Nodos
```

---

## 4. Ejemplo Básico

```csharp
public class MiVisitor
    : ExpressionVisitor
{
}
```

---

## 5. Sobrescribir VisitBinary

```csharp
protected override Expression VisitBinary(
    BinaryExpression node)
{
    return base.VisitBinary(node);
}
```

---

## 6. Caso Empresarial

Agregar filtros automáticos.

Auditoría.

Multi-Tenant.

Soft Delete.

---

## 7. Uso en EF Core

Entity Framework utiliza internamente Expression Visitors para traducir LINQ a SQL.

---

## 8. Ventajas

- Muy flexible.
- Permite modificar consultas.
- Base de muchos frameworks.

---

## 9. Resumen

Expression Visitor permite recorrer y transformar árboles de expresiones.
