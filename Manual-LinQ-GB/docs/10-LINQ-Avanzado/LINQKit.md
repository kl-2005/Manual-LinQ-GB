# LINQKit

## 1. Introducción

LINQKit es una librería que amplía las capacidades de LINQ y Entity Framework.

Permite combinar expresiones dinámicamente.

---

## 2. Instalación

```powershell
Install-Package LINQKit.Microsoft.EntityFrameworkCore
```

---

## 3. Problema

Expresiones complejas:

```csharp
x => x.Activo
```

```csharp
x => x.Edad > 18
```

---

## 4. Solución

Combinar expresiones:

```csharp
var expresion =
    expr1.And(expr2);
```

---

## 5. PredicateBuilder

```csharp
var predicate =
    PredicateBuilder.New<Cliente>();
```

---

## 6. Ejemplo

```csharp
predicate =
    predicate.And(
        c => c.Activo
    );
```

```csharp
predicate =
    predicate.And(
        c => c.Edad > 18
    );
```

---

## 7. Uso

```csharp
var resultado =
    contexto.Clientes
        .AsExpandable()
        .Where(predicate);
```

---

## 8. Caso Empresarial

Filtros dinámicos.

Buscadores avanzados.

Dashboards.

---

## 9. Resumen

LINQKit facilita la creación de consultas complejas y dinámicas.
