**ASP.NET Core SignalR** es una biblioteca para comunicación web en tiempo real. Mantiene una conexión entre el servidor y los clientes para que ambos puedan enviarse mensajes cuando ocurren, sin que el cliente tenga que consultar repetidamente si hay novedades.

La diferencia clave está en **quién puede iniciar el intercambio**:

- En una API REST habitual, el cliente envía una solicitud y el servidor responde. Por ejemplo, `GET /api/productos`.
- Con SignalR, cliente y servidor pueden enviarse mensajes durante una conexión activa. El servidor puede avisar espontáneamente al cliente, por ejemplo, cuando llega un mensaje nuevo al chat.

## Comparación

|Aspecto|REST habitual|SignalR|
|---|---|---|
|Interacción típica|Solicitud y respuesta|Mensajes en ambas direcciones|
|Quién inicia|Normalmente el cliente|Cliente o servidor|
|Conexión|Cada solicitud es independiente|Se mantiene una conexión lógica activa|
|Uso típico|Consultar, crear o actualizar recursos|Chats, alertas, seguimiento en vivo y colaboración|
|Ejemplo|`GET /api/orders/15`|“Notificar a los clientes conectados que cambió el pedido 15”|

REST y SignalR no son incompatibles. Una misma aplicación puede usar REST para operaciones de consulta y actualización, y SignalR para notificar cambios en tiempo real.

## ¿Qué transportes utiliza?

SignalR no es sinónimo de WebSockets. **SignalR es una capa de más alto nivel** que elige un transporte compatible entre cliente y servidor. Puede usar WebSockets, eventos enviados por el servidor o _long polling_; WebSockets suele ser la opción preferida cuando está disponible.

Esto significa que, al programar con SignalR, normalmente trabajas con sus **hubs** y clientes, mientras que la biblioteca se encarga de gestionar detalles de conexión y transporte.

## ¿Qué es un Hub?

Un **Hub** es el punto de comunicación de SignalR. El cliente puede invocar métodos del servidor, y el servidor puede invocar métodos en los clientes conectados. SignalR también permite enviar a todos los clientes, a uno específico o a grupos.

Ejemplo sencillo de un Hub en C#:

```
using Microsoft.AspNetCore.SignalR;

public class ChatHub : Hub
{
    public async Task EnviarMensaje(string usuario, string mensaje)
    {
        await Clients.All.SendAsync("RecibirMensaje", usuario, mensaje);
    }
}
```

Aquí:

- `EnviarMensaje` es un método que el cliente puede invocar en el servidor.
- `Clients.All.SendAsync(...)` envía un mensaje a todos los clientes conectados.
- `"RecibirMensaje"` es el nombre del método que el cliente debe manejar.

El Hub permite expresar la interacción como llamadas a métodos, pero la comunicación sigue ocurriendo mediante mensajes sobre una conexión de red. SignalR admite un protocolo de Hub basado en JSON de forma predeterminada y también uno basado en MessagePack.

## Analogía con REST

Con REST, un cliente pregunta:

> “¿Hay mensajes nuevos?”

Si quiere enterarse de cambios, puede volver a consultar después de un intervalo. Eso se llama _polling_ y genera solicitudes repetidas, incluso cuando no hay novedades.

Con SignalR, el cliente establece la conexión y el servidor puede avisar:

> “Llegó un mensaje nuevo.”

SignalR es útil cuando importa recibir actualizaciones rápidamente. Para operaciones típicas de una API —como consultar un usuario, crear un producto o actualizar un pedido— REST suele ser más directo.

## ¿Cómo se configura en ASP.NET Core?

Se registra SignalR y se asigna una ruta al Hub:

```
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddSignalR();

var app = builder.Build();

app.MapHub<ChatHub>("/chat");

app.Run();
```

La ruta `/chat` es el punto al que se conecta el cliente SignalR. Es distinta de una ruta REST como `/api/productos`.

**En resumen:** REST expone recursos y suele funcionar mediante solicitudes y respuestas. SignalR mantiene una conexión lógica para permitir mensajes en tiempo real entre el servidor y los clientes. Para una aplicación de pedidos, por ejemplo, podrías usar REST para consultar o modificar el pedido y SignalR para avisar a la pantalla cuando cambie su estado.