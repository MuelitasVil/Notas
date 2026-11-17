## 3.1 Inmutabilidad

En Java quizá declaras un campo `final` para evitar que se reasigne:

```
private final String nombre;
```

En C#, una propiedad de solo lectura puede escribirse así:

```
public class Usuario
{
    public string Nombre { get; }

    public Usuario(string nombre)
    {
        Nombre = nombre;
    }
}
```

Después de construir el objeto, `Nombre` no se puede reasignar desde fuera. Esto protege la propiedad, aunque no necesariamente hace inmutable al objeto entero: la clase podría tener otras propiedades modificables.

### Propiedades con `init`

Si quieres permitir que una propiedad se establezca durante la creación del objeto, pero no cambiarla después, puedes usar `init`:

```
public class Usuario
{
    public string Nombre { get; init; }
    public string Correo { get; init; }
}
```

Se puede inicializar con:

```
var usuario = new Usuario
{
    Nombre = "Ana",
    Correo = "ana@example.com"
};
```

Pero no se permite modificarla después:

```
// usuario.Nombre = "Laura"; // No compila
```

`init` se parece a asignar campos `final` durante la construcción, aunque no son exactamente el mismo mecanismo.

Para una clase de datos compacta, también puedes usar un `record`:

```
public record Usuario(string Nombre, string Correo);
```

Puedes crear una instancia modificada mediante `with`, que produce otra instancia:

```
var original = new Usuario("Ana", "ana@example.com");
var actualizado = original with { Correo = "ana.nuevo@example.com" };
```

`original` conserva su valor. Sin embargo, `record` no vuelve profundamente inmutables a los objetos que contiene: si una propiedad apunta a una lista mutable, esa lista todavía podría modificarse.

## 3.2 Colecciones de solo lectura e inmutabilidad

`readonly` en un campo impide reasignar la referencia, pero no modificar el objeto al que apunta:

```
private readonly List<string> _roles = new();

public void AgregarRol(string rol)
{
    _roles.Add(rol); // Sí se permite: la lista sigue siendo mutable
}
```

Una opción simple es exponer una interfaz de solo lectura:

```
private readonly List<string> _roles = new();

public IReadOnlyList<string> Roles => _roles;
```

Así, quien use la propiedad no puede llamar a `Add` a través de `Roles`. Pero la clase todavía puede modificar la lista interna.

Si necesitas colecciones inmutables, .NET dispone del paquete y los tipos `System.Collections.Immutable`, como `ImmutableList<T>`. No hace falta incorporarlos en todos los casos; son útiles cuando la colección debe garantizar que sus versiones no se modifiquen.

## 3.3 Nullabilidad

En Java, cualquier referencia puede ser `null`, salvo que uses anotaciones y herramientas adicionales. En C#, puedes activar las **referencias anulables** para que el compilador advierta dónde un valor podría ser `null`.

```
#nullable enable

string nombre = "Ana";
string? segundoNombre = null;
```

- `string` expresa que se espera un valor no nulo.
- `string?` expresa que el valor puede ser nulo.

Si intentas usar `segundoNombre` sin comprobarlo, el compilador puede advertirte:

```
Console.WriteLine(segundoNombre.Length); // Advertencia posible
```

Compruébalo antes de acceder:

```
if (segundoNombre is not null)
{
    Console.WriteLine(segundoNombre.Length);
}
```

La anotación `?` ayuda al análisis estático del compilador; no crea por sí misma una clase distinta ni impide que `null` aparezca durante la ejecución.

### Operador de acceso condicional

Si solo quieres acceder cuando el valor no es nulo:

```
int? longitud = segundoNombre?.Length;
```

Si `segundoNombre` es `null`, el resultado también será `null`. Por eso `longitud` es `int?`, es decir, un entero que puede ser nulo.

### Valor alternativo: `??`

El operador `??` proporciona un valor alternativo cuando la expresión izquierda es nula:

```
string nombreMostrado = segundoNombre ?? "Sin segundo nombre";
```

También existe `??=` para asignar un valor solo si la variable actualmente es nula:

```
string? etiqueta = null;
etiqueta ??= "General";
```

## 3.4 Manejo de errores con excepciones

C# usa `try`, `catch` y `finally`, igual que Java:

```
try
{
    int cantidad = int.Parse("abc");
}
catch (FormatException ex)
{
    Console.WriteLine($"El valor no tiene un formato válido: {ex.Message}");
}
finally
{
    Console.WriteLine("Esta parte se ejecuta al terminar.");
}
```

`int.Parse` lanza una excepción si la cadena no representa un entero. Si el dato podría no ser válido y eso es esperable, `TryParse` suele ser mejor:

```
string entrada = "42";

if (int.TryParse(entrada, out int cantidad))
{
    Console.WriteLine($"Cantidad: {cantidad}");
}
else
{
    Console.WriteLine("La entrada no es un entero válido.");
}
```

`TryParse` comunica el resultado mediante un valor `bool` y entrega el entero a través de `out`. Así se evita usar excepciones para controlar una situación normal, como una entrada incorrecta.

### Excepciones propias

Puedes definir una excepción propia cuando quieras representar un error concreto del dominio:

```
public class SaldoInsuficienteException : Exception
{
    public SaldoInsuficienteException(string mensaje)
        : base(mensaje)
    {
    }
}
```

Y lanzarla así:

```
if (monto > saldo)
{
    throw new SaldoInsuficienteException("El saldo no alcanza para realizar el pago.");
}
```

## Resumen de equivalencias

|Necesidad|Java|C#|
|---|---|---|
|Evitar reasignar un campo|`final`|`readonly` para campos; propiedades `{ get; }`|
|Inicializar y luego impedir cambios|Constructor con `final`|`{ get; init; }`|
|Advertir sobre valores nulos|Anotaciones como `@Nullable` y analizadores|Referencias anulables, por ejemplo `string?`|
|Valor alternativo si hay `null`|`Optional` u operador ternario|`??`|
|Acceder solo si no es nulo|Comprobación o `Optional`|`?.`|
|Convertir sin excepción por entrada inválida|Validar antes o usar una API específica|`TryParse`|
|Manejar errores excepcionales|`try/catch`|`try/catch`|

La idea principal es distinguir entre **un valor ausente**, que conviene representar explícitamente con tipos anulables y comprobaciones, y **un error excepcional**, para el que usarías una excepción.

En el siguiente paso de este punto podemos practicar con un método que recibe un monto nullable, valida la entrada y devuelve un resultado sin permitir que una operación normal dependa de excepciones.