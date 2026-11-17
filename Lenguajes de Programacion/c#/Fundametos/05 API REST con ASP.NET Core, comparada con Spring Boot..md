## 1. Crear y ejecutar el proyecto

Con el SDK de .NET instalado:

```
dotnet new webapi -n ProductosApi --use-controllers
cd ProductosApi
dotnet run
```

`--use-controllers` solicita la plantilla basada en controladores. Sin esa opción, según la plantilla y versión del SDK, podrías obtener una API basada en _Minimal APIs_.

La plantilla crea el proyecto y sus archivos, incluido el `.csproj`. `dotnet run` lo compila y levanta el servidor. La terminal muestra las direcciones disponibles; la API normalmente escucha en `localhost` con un puerto asignado.

## 2. Un endpoint con controller

Este controller recibe una solicitud HTTP y devuelve una respuesta JSON:

```
using Microsoft.AspNetCore.Mvc;

namespace ProductosApi.Controllers;

[ApiController]
[Route("api/[controller]")]
public class ProductosController : ControllerBase
{
    [HttpGet]
    public ActionResult<IEnumerable<string>> ObtenerTodos()
    {
        var productos = new[] { "Teclado", "Mouse" };
        return Ok(productos);
    }

    [HttpGet("{id:int}")]
    public ActionResult<string> ObtenerPorId(int id)
    {
        if (id <= 0)
        {
            return BadRequest("El id debe ser mayor que cero.");
        }

        return Ok($"Producto {id}");
    }
}
```

La ruta `api/[controller]` se convierte en `api/productos`: ASP.NET Core toma el nombre del controller y elimina el sufijo `Controller`.

Algunos elementos conocidos de Spring:

|ASP.NET Core|Spring Boot|
|---|---|
|`[ApiController]`|`@RestController`|
|`[Route("api/[controller]")]`|`@RequestMapping("/api/productos")`|
|`[HttpGet]`|`@GetMapping`|
|`[HttpGet("{id:int}")]`|`@GetMapping("/{id}")`|
|`ControllerBase`|Funciones de un controller REST|
|`Ok(...)`|`ResponseEntity.ok(...)`|
|`BadRequest(...)`|`ResponseEntity.badRequest(...)`|

Los atributos de C# (`[HttpGet]`) cumplen un papel similar a las anotaciones de Java (`@GetMapping`): añaden metadatos que el framework utiliza para conectar rutas HTTP con métodos.

## 3. Inyección de dependencias: Controller → Service

En lugar de poner la lógica de negocio en el controller, la movemos a un servicio.

```
public record Producto(int Id, string Nombre, decimal Precio);

public interface IProductoService
{
    IReadOnlyList<Producto> ObtenerTodos();
}
```

Una implementación en memoria para el ejemplo:

```
public class ProductoService : IProductoService
{
    private readonly List<Producto> _productos =
    [
        new(1, "Teclado", 120_000m),
        new(2, "Mouse", 65_000m)
    ];

    public IReadOnlyList<Producto> ObtenerTodos()
    {
        return _productos.AsReadOnly();
    }
}
```

El controller recibe la interfaz por constructor:

```
[ApiController]
[Route("api/[controller]")]
public class ProductosController : ControllerBase
{
    private readonly IProductoService _productoService;

    public ProductosController(IProductoService productoService)
    {
        _productoService = productoService;
    }

    [HttpGet]
    public ActionResult<IReadOnlyList<Producto>> ObtenerTodos()
    {
        return Ok(_productoService.ObtenerTodos());
    }
}
```

Esto es equivalente en intención a usar `@Autowired` o inyección por constructor en Spring. La diferencia práctica es que en ASP.NET Core normalmente registras explícitamente cada implementación en el contenedor de dependencias.

## 4. Registrar el servicio en `Program.cs`

En la plantilla moderna, `Program.cs` configura y arranca la aplicación:

```
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddScoped<IProductoService, ProductoService>();

var app = builder.Build();

app.MapControllers();

app.Run();
```

La línea:

```
builder.Services.AddScoped<IProductoService, ProductoService>();
```

indica que, por cada solicitud web, el contenedor puede crear y compartir una instancia del servicio durante esa solicitud.

Otros ciclos de vida comunes:

- `AddTransient`: crea una instancia cada vez que se solicita.
- `AddScoped`: una instancia por solicitud HTTP.
- `AddSingleton`: una instancia para toda la vida de la aplicación.

En Spring, el framework suele detectar componentes mediante anotaciones como `@Service`. En ASP.NET Core, es habitual ver el registro explícito en `Program.cs`.

## 5. Mapa mental con Spring Boot

El flujo que acabamos de construir es:

```
Solicitud HTTP
    ↓
ProductosController
    ↓
IProductoService
    ↓
ProductoService
    ↓
Respuesta HTTP en JSON
```

El controller maneja HTTP, el servicio concentra la lógica de aplicación y la interfaz permite desacoplar el controller de la implementación concreta. Más adelante podemos introducir el repository y una base de datos.

**En esta sección vimos:** creación y ejecución de un proyecto, rutas HTTP con controllers, respuestas, inyección de dependencias y registro de servicios.

El próximo paso dentro de este tema será completar las operaciones REST (`GET`, `POST`, `PUT`, `DELETE`) y ver cómo ASP.NET Core recibe y valida datos del cuerpo de una solicitud.