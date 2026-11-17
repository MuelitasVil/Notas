## 1. Logs con `ILogger`

ASP.NET Core incluye la abstracción `ILogger<T>`, que cumple un papel parecido a la interfaz de logging de SLF4J. El framework configura los proveedores que escriben los mensajes: por ejemplo, en la consola o en otros destinos.

Puedes inyectar el logger por constructor:

```
using Microsoft.Extensions.Logging;

public class ProductoService
{
    private readonly ILogger<ProductoService> _logger;

    public ProductoService(ILogger<ProductoService> logger)
    {
        _logger = logger;
    }

    public void ConsultarProducto(int id)
    {
        _logger.LogInformation(
            "Consultando el producto con id {ProductoId}",
            id
        );
    }
}
```

La plantilla `"{ProductoId}"` crea un campo estructurado llamado `ProductoId`. Es preferible a construir el mensaje con interpolación:

```
// Evita esto para logs estructurados:
_logger.LogInformation($"Consultando el producto con id {id}");
```

Con parámetros, el proveedor puede guardar por separado el mensaje y sus valores. La idea es similar a usar placeholders `{}` en SLF4J.

Niveles comunes:

|C# `ILogger`|SLF4J|
|---|---|
|`LogTrace`|`trace`|
|`LogDebug`|`debug`|
|`LogInformation`|`info`|
|`LogWarning`|`warn`|
|`LogError`|`error`|
|`LogCritical`|`error` de máxima severidad|

Para registrar una excepción, pásala como primer argumento:

```
try
{
    // Operación que podría fallar
}
catch (Exception ex)
{
    _logger.LogError(ex, "No se pudo consultar el producto {ProductoId}", id);
    throw;
}
```

Así el log puede incluir los detalles de la excepción y su stack trace. Si la excepción se debe propagar, `throw;` conserva el stack trace original; evita `throw ex;` para volver a lanzarla.

## 2. Pruebas unitarias con xUnit

En el ecosistema .NET hay varios frameworks de pruebas. **xUnit** es una opción común; NUnit y MSTest también se usan. El concepto es el mismo que con JUnit: preparar datos, ejecutar el método y comprobar el resultado.

Crea un proyecto de pruebas:

```
dotnet new xunit -n Productos.Tests
dotnet add Productos.Tests reference src/Productos.Application
```

Si la solución aún no contiene el proyecto:

```
dotnet sln add Productos.Tests
```

Ejecuta las pruebas desde la carpeta de la solución:

```
dotnet test
```

## 3. Probar un servicio sin base de datos

Supongamos que el servicio calcula el precio promedio:

```
public record Producto(int Id, string Nombre, decimal Precio);

public interface IProductoRepository
{
    IReadOnlyList<Producto> ObtenerTodos();
}

public class ProductoService
{
    private readonly IProductoRepository _repository;

    public ProductoService(IProductoRepository repository)
    {
        _repository = repository;
    }

    public decimal? ObtenerPrecioPromedio()
    {
        var productos = _repository.ObtenerTodos();

        return productos.Count == 0
            ? null
            : productos.Average(p => p.Precio);
    }
}
```

Para probarlo, podemos crear un repositorio falso en memoria:

```
public class ProductoRepositoryFalso : IProductoRepository
{
    private readonly IReadOnlyList<Producto> _productos;

    public ProductoRepositoryFalso(IReadOnlyList<Producto> productos)
    {
        _productos = productos;
    }

    public IReadOnlyList<Producto> ObtenerTodos() => _productos;
}
```

Una prueba con xUnit:

```
using Xunit;

public class ProductoServiceTests
{
    [Fact]
    public void ObtenerPrecioPromedio_ConProductos_DevuelvePromedio()
    {
        // Arrange
        var productos = new List<Producto>
        {
            new(1, "Teclado", 100_000m),
            new(2, "Mouse", 50_000m)
        };

        var repository = new ProductoRepositoryFalso(productos);
        var service = new ProductoService(repository);

        // Act
        var promedio = service.ObtenerPrecioPromedio();

        // Assert
        Assert.Equal(75_000m, promedio);
    }
}
```

`[Fact]` marca un caso de prueba, como `@Test` en JUnit. `Assert.Equal` comprueba el resultado esperado.

También conviene cubrir el caso de lista vacía:

```
[Fact]
public void ObtenerPrecioPromedio_SinProductos_DevuelveNull()
{
    var repository = new ProductoRepositoryFalso(new List<Producto>());
    var service = new ProductoService(repository);

    var promedio = service.ObtenerPrecioPromedio();

    Assert.Null(promedio);
}
```

## 4. Equivalencias principales

| xUnit                       | JUnit                      |
| --------------------------- | -------------------------- |
| `[Fact]`                    | `@Test`                    |
| `[Theory]` y `[InlineData]` | Pruebas parametrizadas     |
| `Assert.Equal(...)`         | `assertEquals(...)`        |
| `Assert.Throws<T>(...)`     | `assertThrows(...)`        |
| `dotnet test`               | `mvn test` o `gradle test` |

Una prueba unitaria debe probar el comportamiento de una unidad —por ejemplo, un servicio— sin depender de una base de datos o de servicios externos. Para eso usamos una implementación falsa del repositorio; en proyectos mayores también se pueden usar bibliotecas de mocks.

Con esto terminamos el temario de siete puntos: sintaxis y tipos, LINQ, inmutabilidad y errores, proyectos y NuGet, APIs REST, arquitecturas e inyección de dependencias, y finalmente logs y pruebas.