# Dynamic LINQ

## 1. Introducción

Dynamic LINQ permite construir consultas LINQ dinámicamente en tiempo de ejecución utilizando cadenas de texto.

Es especialmente útil cuando los filtros son definidos por el usuario.

---

## 2. Problema que Resuelve

Consulta tradicional:

```csharp
var resultado =
    clientes.Where(
        c => c.Edad > 18
    );
```

La condición está escrita en código.

---

## 3. Dynamic LINQ

Permite:

```csharp
var resultado =
    clientes.Where(
        "Edad > 18"
    );
```

---

## 4. Librería

Generalmente se utiliza:

```text
System.Linq.Dynamic.Core
```

---

## 5. Instalación

```powershell
Install-Package System.Linq.Dynamic.Core
```

---

## 6. Ejemplo

```csharp
var clientesAdultos =
    clientes.Where(
        "Edad >= 18"
    );
```

---

## 7. Ordenamiento Dinámico

```csharp
clientes.OrderBy(
    "Nombre"
);
```

---

## 8. Filtros Dinámicos

```csharp
clientes.Where(
    "Ciudad == @0",
    "Quito"
);
```

---

## 9. Caso Empresarial

Buscadores avanzados.

Filtros configurables.

Reportes personalizados.

---

## 10. Resumen

Dynamic LINQ permite generar consultas en tiempo de ejecución sin escribir expresiones fijas.
