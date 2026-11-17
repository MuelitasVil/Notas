
## Crear el proyecto:

```
dotnet new webapi -n motor_control_system_simulation_api --use-controllers
```

Ejecutar el proyecto:

```
dotnet run
```

Esa lista muestra los archivos y carpetas creados por la plantilla de **ASP.NET Core Web API con controllers**. Es el punto de partida del proyecto; varios elementos vienen de la plantilla, aunque todavía no los uses.

| Elemento                                     | Para qué sirve                                                                                                                                                              |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Controllers/`                               | Contiene los controllers que reciben solicitudes HTTP y generan respuestas. La plantilla suele incluir `WeatherForecastController.cs`.                                      |
| `obj/`                                       | Archivos generados durante la restauración y compilación, como dependencias resueltas y datos intermedios. No se suele editar manualmente ni subir al repositorio.          |
| `Properties/`                                | Configuración del perfil de ejecución local, normalmente en `launchSettings.json`. Define puertos, entorno y cómo iniciar la aplicación desde Visual Studio o `dotnet run`. |
| `appsettings.json`                           | Configuración general de la aplicación: por ejemplo, cadenas de conexión, niveles de logging u opciones propias.                                                            |
| `appsettings.Development.json`               | Configuración que se aplica específicamente cuando la aplicación corre en entorno `Development`; puede reemplazar valores de `appsettings.json`.                            |
| `motor_control_system_simulation_api.csproj` | Archivo del proyecto .NET. Define el framework objetivo, paquetes y propiedades de compilación. Es comparable, en propósito general, al `pom.xml` o `build.gradle` de Java. |
| `motor_control_system_simulation_api.http`   | Archivo para enviar solicitudes HTTP de prueba desde herramientas compatibles, como Visual Studio o la extensión REST Client de VS Code.                                    |
| `Program.cs`                                 | Punto de inicio y configuración de la aplicación: registra servicios, configura middleware y mapea rutas.                                                                   |
| `WeatherForecast.cs`                         | Modelo de ejemplo que trae la plantilla; probablemente se relaciona con el controller de clima. Puedes eliminarlo cuando ya no uses ese ejemplo.                            |

En la consola de PowerShell, las columnas significan:

- `Mode`: tipo de elemento; `d-----` indica directorio y `-a----` archivo.
- `LastWriteTime`: última fecha y hora de modificación.
- `Length`: tamaño en bytes. En directorios normalmente no muestra un tamaño útil.
- `Name`: nombre.