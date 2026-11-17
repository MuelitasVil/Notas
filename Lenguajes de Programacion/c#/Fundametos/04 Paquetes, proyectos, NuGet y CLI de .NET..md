
Ahora estamos en el **punto 4 del temario**. La comparación principal es: **proyecto de .NET ↔ proyecto Maven/Gradle**, **NuGet ↔ Maven Central**, y **CLI de `dotnet` ↔ comandos de Maven o Gradle**.

## 4.1 ¿Qué es un proyecto de .NET?

Un proyecto de C# suele definirse en un archivo `.csproj`. Ahí se especifican el framework objetivo, las dependencias y otras opciones de compilación.

Por ejemplo, un proyecto de aplicación de consola puede tener:

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net8.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>
```

Equivale, de manera general, al `pom.xml` de Maven o al `build.gradle` de Gradle, aunque cada formato tiene responsabilidades y opciones propias.

- `TargetFramework` indica para qué versión de .NET se compila.
- `OutputType` indica el tipo de salida: `Exe` para una aplicación ejecutable.
- `ImplicitUsings` habilita ciertos `using` comunes automáticamente.
- `Nullable` activa las advertencias de análisis de nulabilidad.

La versión `net8.0` es solo un ejemplo; puedes encontrar proyectos dirigidos a otras versiones.

## 4.2 Solución y proyectos

Una **solución** (`.sln`) puede agrupar varios proyectos: por ejemplo, una API, una biblioteca de dominio y un proyecto de pruebas. Se parece a un workspace o proyecto multi-módulo de Java.

```
MiAplicacion/
├── MiAplicacion.sln
├── src/
│   ├── MiAplicacion.Api/
│   │   └── MiAplicacion.Api.csproj
│   └── MiAplicacion.Dominio/
│       └── MiAplicacion.Dominio.csproj
└── tests/
    └── MiAplicacion.Tests/
        └── MiAplicacion.Tests.csproj
```

Una solución puede contener proyectos que se referencian entre sí. Por ejemplo, la API puede depender del proyecto de dominio, y el proyecto de pruebas puede depender de ambos.

## 4.3 NuGet y dependencias

NuGet es el gestor de paquetes de .NET. Puedes agregar un paquete desde la terminal:

```
dotnet add package Nombre.Del.Paquete
```

Por ejemplo, para incorporar una biblioteca de serialización JSON externa:

```
dotnet add package Newtonsoft.Json
```

El paquete se declara en el `.csproj`, normalmente así:

```
<ItemGroup>
  <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
</ItemGroup>
```

NuGet también mantiene un archivo `obj/project.assets.json` con información de dependencias resueltas. Es generado por las herramientas; normalmente no se edita a mano.

En Java, las dependencias se declaran en `pom.xml` o `build.gradle` y se resuelven desde repositorios como Maven Central. La idea de agregar una dependencia y fijar su versión es familiar.

## 4.4 Comandos básicos de `dotnet`

Desde la terminal:

```
dotnet new console -n MiAplicacion
```

Crea un proyecto de consola.

```
cd MiAplicacion
dotnet run
```

Compila y ejecuta el proyecto.

```
dotnet build
```

Compila sin ejecutarlo.

```
dotnet test
```

Ejecuta los proyectos de pruebas que encuentre en el contexto de la solución o proyecto.

```
dotnet restore
```

Restaura las dependencias declaradas. A menudo `build` y `run` hacen la restauración necesaria automáticamente.

```
dotnet add package Nombre.Del.Paquete
```

Agrega un paquete NuGet al proyecto.

## 4.5 Crear una solución con varios proyectos

Una secuencia típica para una API y una biblioteca de dominio podría ser:

```
dotnet new sln -n MiAplicacion
dotnet new webapi -n MiAplicacion.Api -o src/MiAplicacion.Api
dotnet new classlib -n MiAplicacion.Dominio -o src/MiAplicacion.Dominio
dotnet sln add src/MiAplicacion.Api
dotnet sln add src/MiAplicacion.Dominio
dotnet add src/MiAplicacion.Api reference src/MiAplicacion.Dominio
```

- `webapi` crea un proyecto de API.
- `classlib` crea una biblioteca de clases.
- `sln add` incorpora proyectos a la solución.
- `reference` agrega una referencia entre proyectos.

Esto es similar a tener módulos separados en un proyecto Maven/Gradle. La organización concreta dependerá de la arquitectura que elijas.

**Nota sobre versiones:** las plantillas y versiones de .NET disponibles dependen del SDK instalado. Puedes revisar los SDK instalados con:

```
dotnet --list-sdks
```

Y consultar las opciones de una plantilla con:

```
dotnet new webapi --help
```

En este punto estamos viendo **cómo se crean y organizan proyectos C#, cómo se agregan dependencias NuGet y cómo se usan los comandos básicos de `dotnet`**. El siguiente punto del temario será **crear una API REST con ASP.NET Core y compararla con Spring Boot**.