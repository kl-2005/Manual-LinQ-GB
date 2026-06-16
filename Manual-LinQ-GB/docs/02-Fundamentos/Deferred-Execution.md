# Ejecución Aplazada (Deferred Execution)

La **Ejecución Aplazada** es un principio fundamental en la arquitectura de LINQ. Significa que el paso de definición de la consulta y el paso de ejecución de la misma están completamente separados.

---

## ¿Cómo Funciona?

Cuando creas una consulta utilizando operadores de LINQ como `Where`, `Select`, `Take` o `OrderBy`, no estás procesando los datos inmediatamente. En su lugar, estás construyendo una **estructura conceptual o plan de ejecución** de lo que deseas obtener.

La evaluación real de la información ocurre únicamente cuando intentas leer o iterar activamente sobre los resultados por primera vez.

---

## Mecanismos que Activan la Ejecución

Una consulta guardada en una variable se ejecutará bajo los siguientes escenarios:

### A. Iteración Explícita
Al recorrer la variable dentro de un ciclo condicional o bucle de repetición:
```csharp
// La consulta se define aquí, pero NO se ejecuta todavía
var consulta = lista.Where(x => x.Activo); 

// La ejecución real ocurre exactamente en esta línea al iniciar el ciclo
foreach (var elemento in consulta) 
{
    Console.WriteLine(elemento.Nombre);
}
