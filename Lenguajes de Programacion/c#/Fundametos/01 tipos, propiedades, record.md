## 1. Tipos por valor y tipos por referencia

En C#, los tipos se agrupan, en términos generales, en tipos por valor y tipos por referencia.

**Tipos por valor** guardan directamente su valor. Algunos ejemplos son `int`, `double`, `bool`, `decimal` y los `struct`.

```
int a = 10;
int b = a;

b = 20;

Console.WriteLine(a); // 10
Console.WriteLine(b); // 20
```

Al asignar `a` a `b`, se copia el valor.

**Tipos por referencia**, como las clases y los arreglos, contienen una referencia al objeto:

```
var listaA = new List<int> { 1, 2, 3 };
var listaB = listaA;

listaB.Add(4);

Console.WriteLine(listaA.Count); // 4
```

`listaA` y `listaB` apuntan a la misma lista. Esto se parece al comportamiento de los objetos en Java.

> La comparación es conceptual: los detalles de memoria difieren entre ambos lenguajes. Lo útil aquí es recordar que asignar una clase a otra variable no crea automáticamente una copia del objeto.

## 2. Propiedades con validación

Las propiedades pueden incluir lógica en sus accesores. Esto sirve, por ejemplo, para validar el valor asignado:

```
public class Producto
{
    private decimal _precio;

    public string Nombre { get; set; }

    public decimal Precio
    {
        get => _precio;
        set
        {
            if (value < 0)
            {
                throw new ArgumentOutOfRangeException(
                    nameof(value),
                    "El precio no puede ser negativo."
                );
            }

            _precio = value;
        }
    }
}
```

`value` es el valor que se intenta asignar a `Precio`. Una asignación como esta ejecuta la validación:

```
var producto = new Producto
{
    Nombre = "Teclado",
    Precio = 120_000m
};
```

Si se intenta asignar un precio negativo, la propiedad lanza una excepción.

## 3. Propiedades inicializadas en el constructor

Para objetos cuyos datos deben ser válidos desde el momento de creación, puedes exigirlos en el constructor:

```
public class Producto
{
    public string Nombre { get; }
    public decimal Precio { get; }

    public Producto(string nombre, decimal precio)
    {
        if (precio < 0)
        {
            throw new ArgumentOutOfRangeException(nameof(precio));
        }

        Nombre = nombre;
        Precio = precio;
    }
}
```

En Java escribirías algo similar con campos `private final` y métodos de acceso. En C#, la propiedad `{ get; }` permite leer el valor desde fuera, pero solo se puede asignar dentro del constructor o durante la inicialización del objeto.

## 4. `record`: datos con comparación por valor

Los `record` de C# están pensados para representar datos. A diferencia de una clase normal, comparan sus instancias usando los valores de sus miembros.

```
public record Producto(string Nombre, decimal Precio);
```

Puedes crear instancias así:

```
var primero = new Producto("Teclado", 120_000m);
var segundo = new Producto("Teclado", 120_000m);

Console.WriteLine(primero == segundo); // True
```

Con una clase normal, `==` suele comparar si ambas variables apuntan a la misma instancia. En este `record`, compara los valores de `Nombre` y `Precio`.

Java 17 también tiene `record`:

```
public record Producto(String nombre, BigDecimal precio) {}
```

La idea principal es parecida: declarar compactamente un tipo orientado a datos y obtener igualdad basada en sus componentes.

## 5. Crear una copia modificada con `with`

Los `record` de C# permiten crear una copia cambiando algunos valores:

```
var original = new Producto("Teclado", 120_000m);
var actualizado = original with { Precio = 110_000m };

Console.WriteLine(original.Precio);    // 120000
Console.WriteLine(actualizado.Precio); // 110000
```

`original` no cambia: se crea otra instancia con los valores indicados.

En C# moderno, las propiedades posicionales de un `record` suelen ser `init`: se pueden establecer al crear el objeto o con `with`, pero no reasignar después normalmente.

### Recapitulación

- Una `class` suele representar objetos con identidad y comportamiento.
- Un `record` resulta práctico para datos y ofrece igualdad por valor.
- Las propiedades pueden ser de lectura y escritura (`get; set;`), solo lectura (`get;`) o inicializables (`init`).
- `with` crea una copia de un `record` con los cambios indicados.

Con esto completamos el primer punto del temario. El siguiente es **colecciones y LINQ**, comparando `List<T>`, `Dictionary<TKey, TValue>`, `Where`, `Select`, `GroupBy` y otras operaciones con Java Streams.