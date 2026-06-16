# Extension Methods (Métodos de Extensión)

Los **Extension Methods** (Métodos de Extensión) son una característica de C# que permite agregar nuevos métodos a tipos existentes sin modificar su código fuente, sin heredar de ellos y sin recompilar las bibliotecas originales.

Esta funcionalidad fue introducida en **C# 3.0** y es uno de los pilares fundamentales sobre los que se construye **LINQ**.

---

# 1. El Fundamento Teórico: ¿Por qué existen?

Normalmente, para agregar funcionalidad a una clase existen dos opciones:

* Modificar la clase original.
* Crear una clase derivada mediante herencia.

Sin embargo, muchas veces estas opciones no son posibles porque:

* La clase pertenece al Framework de .NET.
* La clase pertenece a una biblioteca de terceros.
* No se desea alterar el diseño original.

Los Extension Methods resuelven este problema permitiendo "extender" tipos existentes desde una clase externa.

Por ejemplo:

```csharp
string nombre = "gabriel";

nombre.Capitalizar();
```

Aunque la clase `string` no posee realmente un método llamado `Capitalizar()`, gracias a un Extension Method el compilador lo interpreta como una llamada válida.

---

# 2. ¿Cómo Funcionan Internamente?

Un Extension Method es realmente un método estático.

Cuando escribimos:

```csharp
texto.MiMetodo();
```

El compilador lo transforma internamente en:

```csharp
MiClaseExtensions.MiMetodo(texto);
```

Por tanto, los Extension Methods son simplemente una sintaxis más amigable para llamar métodos estáticos.

---

# 3. Requisitos para Crear un Extension Method

Para que un método sea reconocido como método de extensión debe cumplir tres condiciones:

### 1. Estar dentro de una clase estática

```csharp
public static class StringExtensions
{
}
```

---

### 2. Ser un método estático

```csharp
public static string Capitalizar(...)
{
}
```

---

### 3. Utilizar la palabra clave `this`

```csharp
public static string Capitalizar(this string texto)
{
}
```

El parámetro precedido por `this` indica cuál será el tipo extendido.

---

# 4. Primer Extension Method

## Clase de Extensión

```csharp
public static class StringExtensions
{
    public static string Capitalizar(this string texto)
    {
        if (string.IsNullOrEmpty(texto))
            return texto;

        return char.ToUpper(texto[0]) + texto.Substring(1);
    }
}
```

---

## Uso

```csharp
string nombre = "gabriel";

string resultado = nombre.Capitalizar();

Console.WriteLine(resultado);
```

### Salida

```text
Gabriel
```

---

# 5. Anatomía de un Extension Method

```csharp
public static string Capitalizar(this string texto)
```

Desglose:

| Elemento          | Descripción                        |
| ----------------- | ---------------------------------- |
| public            | Accesible desde cualquier proyecto |
| static            | Debe ser estático                  |
| string            | Tipo de retorno                    |
| Capitalizar       | Nombre del método                  |
| this string texto | Tipo que se extiende               |

---

# 6. Ejemplos con Tipos Primitivos

## Extender int

```csharp
public static class IntExtensions
{
    public static bool EsPar(this int numero)
    {
        return numero % 2 == 0;
    }
}
```

Uso:

```csharp
int valor = 10;

bool resultado = valor.EsPar();

Console.WriteLine(resultado);
```

Salida:

```text
True
```

---

## Extender decimal

```csharp
public static class DecimalExtensions
{
    public static decimal AplicarIVA(
        this decimal precio,
        decimal iva = 0.15m)
    {
        return precio + (precio * iva);
    }
}
```

Uso:

```csharp
decimal precio = 100;

Console.WriteLine(precio.AplicarIVA());
```

Salida:

```text
115
```

---

# 7. Extension Methods y Colecciones

Los métodos LINQ son Extension Methods.

Ejemplo:

```csharp
lista.Where(x => x > 10);
```

Internamente:

```csharp
Enumerable.Where(lista, x => x > 10);
```

---

## Crear un Extension Method para List

```csharp
public static class ListExtensions
{
    public static void MostrarTodos<T>(
        this List<T> lista)
    {
        foreach (var item in lista)
        {
            Console.WriteLine(item);
        }
    }
}
```

Uso:

```csharp
List<string> nombres =
[
    "Gabriel",
    "Ana",
    "Luis"
];

nombres.MostrarTodos();
```

Salida:

```text
Gabriel
Ana
Luis
```

---

# 8. Extension Methods Genéricos

Pueden trabajar con cualquier tipo.

```csharp
public static class GenericExtensions
{
    public static string ObtenerTipo<T>(
        this T objeto)
    {
        return typeof(T).Name;
    }
}
```

Uso:

```csharp
int numero = 10;

Console.WriteLine(numero.ObtenerTipo());
```

Salida:

```text
Int32
```

---

# 9. Uso Empresarial

Supongamos una entidad:

```csharp
public class Cliente
{
    public string Nombre { get; set; }
    public bool Activo { get; set; }
}
```

Creamos extensiones específicas:

```csharp
public static class ClienteExtensions
{
    public static string Estado(
        this Cliente cliente)
    {
        return cliente.Activo
            ? "Activo"
            : "Inactivo";
    }
}
```

Uso:

```csharp
Cliente cliente = new Cliente
{
    Nombre = "Gabriel",
    Activo = true
};

Console.WriteLine(cliente.Estado());
```

Salida:

```text
Activo
```

---

# 10. Extension Methods en LINQ

Los métodos más usados de LINQ son extensiones de `IEnumerable<T>`.

Ejemplos:

```csharp
Where()
Select()
OrderBy()
ThenBy()
GroupBy()
Any()
All()
Count()
First()
FirstOrDefault()
Single()
SingleOrDefault()
Distinct()
Take()
Skip()
```

Todos pertenecen a:

```csharp
System.Linq.Enumerable
```

Ejemplo:

```csharp
var resultado =
    numeros
        .Where(x => x > 10)
        .OrderBy(x => x)
        .Take(5);
```

---

# 11. Ventajas

## Código Más Legible

Sin extensión:

```csharp
StringHelper.Capitalizar(nombre);
```

Con extensión:

```csharp
nombre.Capitalizar();
```

---

## Reutilización

La lógica puede reutilizarse en múltiples proyectos.

---

## Mantenimiento

Las funcionalidades quedan organizadas por tipo.

---

## Compatibilidad con LINQ

Toda la sintaxis fluida de LINQ depende de Extension Methods.

---

# 12. Desventajas

## Posibles Conflictos

Dos extensiones pueden tener el mismo nombre.

```csharp
Capitalizar()
```

en diferentes namespaces.

---

## No Acceden a Miembros Privados

Solo pueden acceder a miembros públicos.

```csharp
public
protected
internal
```

No pueden acceder a:

```csharp
private
```

---

# 13. Buenas Prácticas

### Correcto

```csharp
ClienteExtensions
StringExtensions
DateTimeExtensions
EnumerableExtensions
```

---

### Evitar

Métodos demasiado grandes:

```csharp
ProcesarFacturaCompleta()
```

Los Extension Methods deben realizar tareas específicas.

---

# 14. Diferencia Entre Herencia y Extension Methods

| Característica                   | Herencia | Extension Method |
| -------------------------------- | -------- | ---------------- |
| Modifica comportamiento          | Sí       | No               |
| Agrega métodos                   | Sí       | Sí               |
| Requiere heredar                 | Sí       | No               |
| Funciona con clases selladas     | No       | Sí               |
| Funciona con tipos del Framework | No       | Sí               |

---

# 15. Resumen

Los Extension Methods permiten agregar funcionalidades a tipos existentes sin modificar su implementación original.

Características principales:

* Son métodos estáticos.
* Se declaran dentro de clases estáticas.
* Utilizan la palabra clave `this`.
* Mejoran la legibilidad del código.
* Son la base de LINQ.
* Permiten reutilización y mantenimiento sencillo.
* Funcionan con cualquier tipo, incluso tipos del Framework.

Prácticamente toda la sintaxis fluida de LINQ está construida sobre Extension Methods, convirtiéndolos en una de las características más importantes de C# moderno.
