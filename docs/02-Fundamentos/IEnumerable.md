# IEnumerable

Representa una secuencia de elementos que puede recorrerse mediante foreach.

## Ejemplo

```csharp
List<int> numeros =
[
    1,2,3,4,5
];

var mayores =
    numeros.Where(n => n > 3);

foreach(var numero in mayores)
{
    Console.WriteLine(numero);
}
```

## Características

- Trabaja en memoria.
- Implementa foreach.
- Soporta ejecución diferida.
