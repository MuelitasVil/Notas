## 1. MVC en ASP.NET Core

MVC significa **Model–View–Controller**. En una API REST, normalmente no hay vistas HTML: el controller recibe la solicitud y devuelve datos, por ejemplo, en JSON.

Una organización común para una API es:

```
ProductosApi/
├── Controllers/
│   └── ProductosController.cs
├── Services/
│   ├── IProductoService.cs
│   └── ProductoService.cs
├── Repositories/
│   ├── IProductoRepository.cs
│   └── ProductoRepository.cs
├── Models/
│   └── Producto.cs
└── Program.cs
```

La separación de capas es parecida a la que usarías con Spring:

|ASP.NET Core|Spring Boot|
|---|---|
|`Controller`|`@RestController`|
|`Service`|`@Service`|
|`Repository`|`@Repository` o interfaz de Spring Data|
|Modelos o DTOs|Entidades y DTOs|
|`Program.cs`|Clase principal y configuración|

Un controller debería ocuparse principalmente de HTTP: interpretar la solicitud y convertir el resultado de la aplicación en una respuesta. La lógica de negocio queda en el servicio, y el acceso a datos en el repositorio.

## 2. Ejemplo de capas

El dominio:

```
public record Producto(int Id, string Nombre, decimal Precio);
```

El contrato del repositorio:

```
public interface IProductoRepository
{
    IReadOnlyList<Producto> ObtenerTodos();
    Producto? ObtenerPorId(int id);
}
```

El servicio:

```
public interface IProductoService
{
    IReadOnlyList<Producto> ObtenerTodos();
    Producto? ObtenerPorId(int id);
}

public class ProductoService : IProductoService
{
    private readonly IProductoRepository _repository;

    public ProductoService(IProductoRepository repository)
    {
        _repository = repository;
    }

    public IReadOnlyList<Producto> ObtenerTodos()
    {
        return _repository.ObtenerTodos();
    }

    public Producto? ObtenerPorId(int id)
    {
        return _repository.ObtenerPorId(id);
    }
}
```

El controller:

```
[ApiController]
[Route("api/[controller]")]
public class ProductosController : ControllerBase
{
    private readonly IProductoService _service;

    public ProductosController(IProductoService service)
    {
        _service = service;
    }

    [HttpGet]
    public ActionResult<IReadOnlyList<Producto>> ObtenerTodos()
    {
        return Ok(_service.ObtenerTodos());
    }

    [HttpGet("{id:int}")]
    public ActionResult<Producto> ObtenerPorId(int id)
    {
        var producto = _service.ObtenerPorId(id);

        return producto is null
            ? NotFound()
            : Ok(producto);
    }
}
```

El `?` en `Producto?` indica que el método puede devolver un producto o `null`. El controller convierte ese resultado en `404 Not Found` o `200 OK`.

## 3. Inyección de dependencias

ASP.NET Core tiene un contenedor de dependencias integrado. Se registran las implementaciones en `Program.cs`:

```
builder.Services.AddScoped<IProductoRepository, ProductoRepository>();
builder.Services.AddScoped<IProductoService, ProductoService>();
```

Luego, el framework crea los objetos y los pasa por el constructor, recorriendo la cadena:

```
Controller → Service → Repository
```

Esto corresponde a la inyección por constructor que probablemente ya has usado con Spring. La principal diferencia práctica es que en ASP.NET Core suele ser explícito el registro de cada interfaz y clase.

Los ciclos de vida más comunes son:

- `AddTransient`: una instancia nueva cada vez que se solicita.
- `AddScoped`: una instancia por solicitud HTTP.
- `AddSingleton`: una instancia compartida durante toda la vida de la aplicación.

En general, repositorios que usan un contexto de base de datos y servicios asociados suelen registrarse como `Scoped`.

## 4. Arquitectura hexagonal

La arquitectura hexagonal busca que la lógica central de la aplicación no dependa de detalles externos como HTTP, bases de datos o frameworks.

Una estructura posible:

```
src/
├── Productos.Domain/
│   └── Producto.cs
├── Productos.Application/
│   ├── IProductoRepository.cs
│   └── ProductoService.cs
├── Productos.Infrastructure/
│   └── ProductoRepositorySql.cs
└── Productos.Api/
    └── ProductosController.cs
```

La dirección de dependencias importa:

```
API ───────────────→ Application ───────────────→ Domain
Infrastructure ────→ Application / Domain
```

- **Domain**: entidades y reglas del negocio.
- **Application**: casos de uso y puertos (interfaces) que necesita la aplicación.
- **Infrastructure**: adaptadores concretos, como SQL, archivos o servicios externos.
- **API**: adaptador de entrada que traduce HTTP a llamadas de aplicación.

Por ejemplo, `IProductoRepository` es un **puerto** declarado hacia el núcleo. `ProductoRepositorySql` es un **adaptador** que implementa ese puerto usando una base de datos.

Así, el servicio de aplicación depende de una abstracción:

```
public class ProductoService : IProductoService
{
    private readonly IProductoRepository _repository;

    public ProductoService(IProductoRepository repository)
    {
        _repository = repository;
    }

    // Casos de uso...
}
```

La implementación concreta se conecta mediante inyección de dependencias:

```
builder.Services.AddScoped<IProductoRepository, ProductoRepositorySql>();
```

El núcleo no necesita saber que la implementación usa SQL. Para una prueba, se podría registrar un repositorio falso o en memoria.

## 5. ¿MVC o hexagonal?

No son necesariamente alternativas excluyentes. MVC organiza principalmente la interacción de presentación; la arquitectura hexagonal organiza las dependencias alrededor del dominio y los casos de uso. Una API puede usar controllers MVC en la capa exterior y mantener el interior organizado de forma hexagonal.

**En esta sección vimos:** separación Controller–Service–Repository, inyección por constructor y cómo las interfaces permiten desacoplar el núcleo de infraestructura. El siguiente punto del temario es **logs y pruebas unitarias**.