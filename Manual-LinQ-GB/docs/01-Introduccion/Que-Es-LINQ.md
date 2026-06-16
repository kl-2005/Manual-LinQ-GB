# ¿Qué es LINQ (Language Integrated Query)?

**LINQ** es el acrónimo de **Language Integrated Query** (Consulta Integrada en el Lenguaje). Es un componente tecnológico del framework .NET que introduce capacidades de consulta de datos directamente en la sintaxis de lenguajes de programación como C# y Visual Basic.

En términos sencillos: LINQ te permite escribir consultas de tipo "SQL" directamente dentro de tu código de C# para buscar, filtrar, ordenar y agrupar información, sin importar de dónde vengan los datos.

---

## 1. El Concepto Clave: Unificación

Antes de LINQ, si tu aplicación necesitaba consultar datos de diferentes lugares, tenías que aprender y mezclar múltiples lenguajes y APIs en un solo archivo de código:

* **Base de Datos:** Escribías cadenas de texto con SQL nativo.
* **Archivo XML:** Escribías rutas complejas de XPath o manejabas la API DOM.
* **Listas en RAM:** Escribías bucles `foreach` e `if` manuales.

LINQ unificó todo este ecosistema bajo una **única sintaxis homogénea**:
## 2. Las Dos Sintaxis de LINQ

LINQ ofrece dos formas de escribir la misma consulta. El compilador las procesa exactamente igual, por lo que la elección depende del gusto del desarrollador o de los estándares del proyecto.

### A. Sintaxis de Consulta (Query Syntax)
Utiliza un estilo declarativo muy similar al SQL tradicional. Es ideal para consultas complejas que involucran uniones (`join`) o agrupaciones (`group by`).

Obtener los nombres de estudiantes mayores de 18 años ordenados alfabéticamente

```
var resultado = listaEstudiantes
                .Where(e => e.Edad >= 18)
                .OrderBy(e => e.Nombre)
                .Select(e => e.Nombre);
```
