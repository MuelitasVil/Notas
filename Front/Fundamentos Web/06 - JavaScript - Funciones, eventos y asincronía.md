---
tags: [javascript, funciones, eventos, asincronia]
---
# JavaScript: funciones, eventos y asincronía

## Declaración de función y función flecha
```js
function sumar(a, b) { return a + b; }
const duplicar = n => n * 2;
```
Una función se puede pasar como argumento a otra (callback):
```js
function ejecutar(callback) { callback('Hola'); }
ejecutar(mensaje => console.log(mensaje));
```

## Referencia a función vs ejecución
```js
boton.addEventListener('click', guardar);   // se llama al hacer clic
boton.addEventListener('click', guardar()); // la ejecuta inmediatamente: normalmente incorrecto
```
En JSX suele escribirse `onClick={guardar}` o `onClick={() => guardar(id)}`.

## Eventos del DOM
El navegador genera eventos cuando una persona escribe, hace clic o envía un formulario.

```js
const input = document.querySelector('#nombre');
input.addEventListener('input', event => {
  console.log(event.target.value);
});
```
- `event`: objeto del evento.
- `event.target`: elemento que originó el evento.
- `event.target.value`: texto del control.
- `event.target.checked`: booleano útil para checkboxes.
- `event.preventDefault()`: evita la acción predeterminada (por ejemplo, el envío nativo de un formulario).

## Promesas, `async` y `await`
Una **Promise** representa el eventual resultado de una operación asíncrona.

```js
async function cargarDatos() {
  const respuesta = await fetch('/api/datos');
  if (!respuesta.ok) throw new Error('Error HTTP');
  return await respuesta.json();
}
```
- `async` hace que la función devuelva una promesa.
- `await` suspende **esa función asíncrona** hasta que la promesa se resuelva o rechace; no bloquea todo el navegador.
- `fetch` no rechaza automáticamente por un estado HTTP 404/500; hay que revisar `response.ok`.

## `try`, `catch` y `finally`
```js
async function enviar() {
  mostrarCarga(true);
  try {
    await guardarDatos();
  } catch (error) {
    mostrarError('No fue posible guardar');
  } finally {
    mostrarCarga(false);
  }
}
```
- `try`: ejecuta el código que puede fallar.
- `catch`: maneja errores lanzados o promesas rechazadas.
- `finally`: se ejecuta al terminar el bloque tanto por éxito como por error (dentro del flujo normal de ejecución).

### ¿Por qué usar `finally`?
Asegura acciones de limpieza, como quitar un indicador de carga, sin duplicar instrucciones en `try` y `catch`.

## Errores frecuentes
- Olvidar `await` en una operación cuyo resultado necesitas antes de continuar.
- Asumir que `async` significa “segundo hilo”: no es así.
- Ejecutar un callback al asignarlo, en vez de pasar una referencia.
- Confundir `event.target.value` (texto) y `event.target.checked` (booleano).
- Tratar la validación del navegador como sustituto de la validación del servidor.

## Relacionadas
[[05 - JavaScript - Objetos, arrays y spread]] · [[07 - JSX - Sintaxis y renderizado condicional]] · [[08 - React - Estado, props y formularios]]
