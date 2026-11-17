Sigamos con un ejemplo un poco más completo. Vamos a definir una clase `Producto`, crear varios productos y calcular el precio promedio. Así repasamos clases, propiedades, métodos, listas y algunas diferencias de sintaxis con Java.

## Clase y propiedades

```c#
public class Producto
{
    public string Nombre { get; }
    public decimal Precio { get; }

    public Producto(string nombre, decimal precio)
    {
        Nombre = nombre;
        Precio = precio;
    }
}
```

Comparado con Java:

- `public class Producto` se ve prácticamente igual.
- El constructor tiene el mismo nombre de la clase y no declara un tipo de retorno.
- `Nombre` y `Precio` son propiedades de solo lectura desde fuera de la clase.
- `decimal` se usa con frecuencia para valores monetarios. En Java, probablemente usarías `BigDecimal` si necesitas precisión decimal.

## Crear objetos y usar una lista

```c#
var productos = new List<Producto>
{
    new Producto("Teclado", 120_000m),
    new Producto("Mouse", 65_000m),
    new Producto("Monitor", 850_000m)
};
```

Necesitas importar la colección:

```c#
using System.Collections.Generic;
```

El sufijo `m` indica que los literales son de tipo `decimal`. El guion bajo solo facilita la lectura del número: `120_000` equivale a `120000`.

## Recorrer y calcular

```c#
decimal total = 0;

foreach (var producto in productos)
{
    total += producto.Precio;
}

decimal promedio = total / productos.Count;

Console.WriteLine($"Promedio: {promedio:C}");
```

La interpolación `$"..."` permite insertar valores directamente en una cadena. El formato `:C` muestra el número como moneda según la configuración regional del programa; el símbolo y la presentación pueden variar según esa configuración.

## Versión usando LINQ

LINQ es la herramienta de C# más comparable con los Streams de Java:

```c#
using System.Linq;

decimal promedio = productos.Average(producto => producto.Precio);
```

La operación cumple un propósito similar a:

```c#
double promedio = productos.stream()
    .mapToDouble(Producto::getPrecio)
    .average()
    .orElse(0);
```

Hay una diferencia importante: `Average()` sobre una lista vacía lanza una excepción. Si la lista podría estar vacía, compruébalo antes:

```
decimal? promedio = productos.Count > 0
    ? productos.Average(producto => producto.Precio)
    : null;
```

El tipo `decimal?` significa que el resultado puede contener un `decimal` o ser `null`. Es una forma de expresar que quizá no exista un promedio.

**Para comprobar tu comprensión:** ¿qué cambiarías para obtener una lista con los nombres de los productos cuyo precio sea mayor a 100 000? Vas a necesitar `Where` y `Select`, equivalentes a `filter` y `map` en Streams.

## La idea central

En Java, normalmente partes de una colección, llamas a `.stream()` y encadenas operaciones:

```c#
List<Producto> productosFiltrados = productos.stream()
    .filter(producto -> producto.getPrecio().compareTo(new BigDecimal("100000")) > 0)
    .toList();
```

En C#, los métodos de LINQ se aplican directamente a la colección:

```c#
List<Producto> productosFiltrados = productos
    .Where(producto => producto.Precio > 100_000m)
    .ToList();
```

La diferencia visual principal es que en C# no necesitas llamar a `.stream()`. `Where` es la operación equivalente a `filter`, y `ToList()` materializa el resultado en una lista.

Supongamos esta clase:

```c#
public record Producto(string Nombre, decimal Precio);

var productos = new List<Producto>
{
    new("Teclado", 120_000m),
    new("Mouse", 65_000m),
    new("Monitor", 850_000m)
};
```

El sufijo `m` indica que los valores son `decimal`, tipo habitual para precios.

## Operaciones comunes

|Objetivo|Java Streams|LINQ en C#|
|---|---|---|
|Filtrar|`.filter(...)`|`.Where(...)`|
|Transformar|`.map(...)`|`.Select(...)`|
|Ordenar|`.sorted(...)`|`.OrderBy(...)`|
|Ordenar descendente|`.sorted(comparator)`|`.OrderByDescending(...)`|
|Tomar primeros elementos|`.limit(n)`|`.Take(n)`|
|Quitar primeros elementos|`.skip(n)`|`.Skip(n)`|
|Comprobar si alguno cumple|`.anyMatch(...)`|`.Any(...)`|
|Comprobar si todos cumplen|`.allMatch(...)`|`.All(...)`|
|Contar coincidencias|`.filter(...).count()`|`.Count(...)`|

Por ejemplo, obtener los nombres de los productos que cuestan más de 100 000:

```c#
List<string> nombres = productos
    .Where(producto => producto.Precio > 100_000m)
    .Select(producto => producto.Nombre)
    .ToList();
```

Equivalente aproximado en Java:

```c#
List<String> nombres = productos.stream()
    .filter(producto -> producto.getPrecio().compareTo(new BigDecimal("100000")) > 0)
    .map(Producto::getNombre)
    .toList();
```

Y ordenar esos resultados por precio descendente:

```c#
List<Producto> ordenados = productos
    .Where(producto => producto.Precio > 100_000m)
    .OrderByDescending(producto => producto.Precio)
    .ToList();
```

## Ejecución diferida

Muchas operaciones de LINQ, como `Where` y `Select`, son diferidas: describen una consulta que se ejecuta cuando recorres el resultado. `ToList()` ejecuta la consulta y guarda los resultados en una lista.

```c#
var consulta = productos.Where(p => p.Precio > 100_000m);
// La consulta todavía no se ha materializado.

List<Producto> resultado = consulta.ToList();
// Aquí se ejecuta y se crea la lista.
```

Esto se parece a los Streams de Java en que las operaciones intermedias se ejecutan al iniciar una operación terminal. Pero hay una diferencia: un Stream de Java normalmente se consume una sola vez; una consulta LINQ sobre una colección puede enumerarse de nuevo.