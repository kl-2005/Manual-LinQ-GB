# ¿Qué es LINQ?

LINQ significa Language Integrated Query.

Es una tecnología introducida por Microsoft en C# 3.0 que permite consultar datos mediante una sintaxis unificada.

## Ventajas

- Menos código
- Mayor legibilidad
- Tipado fuerte
- IntelliSense
- Consultas unificadas

## Ejemplo

```csharp
var resultado =
    productos.Where(p => p.Precio > 100);
```

## Fuentes de datos soportadas

- Colecciones
- Bases de datos
- XML
- Entity Framework
- Objetos personalizados
