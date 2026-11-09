---
tags: [jsx, react, sintaxis]
---
# JSX: sintaxis y comportamiento dinámico

## ¿Qué es JSX?
**JSX** es una extensión de sintaxis de JavaScript que permite describir elementos de interfaz con etiquetas parecidas a HTML. En archivos `.tsx` se combina con TypeScript. JSX se transforma mediante herramientas de compilación en llamadas JavaScript.

```tsx
function Saludo() {
  const nombre = 'Ana';
  return <h1>Hola, {nombre}</h1>;
}
```

## HTML vs JSX
| HTML | JSX / TSX |
|---|---|
| `class="caja"` | `className="caja"` |
| `for="correo"` | `htmlFor="correo"` |
| `onclick="..."` | `onClick={funcion}` |
| `novalidate` | `noValidate` |
| Varios elementos hermanos libres | Devolver un solo elemento raíz o un fragmento `<>...</>` |

## Llaves `{}`: expresiones JavaScript
```tsx
const contador = 3;
return <p>Hay {contador} mensajes</p>;
```
Las llaves permiten expresiones, como variables, ternarios y llamadas a funciones, no sentencias arbitrarias `if` escritas directamente dentro del JSX.

## Props
Un componente recibe propiedades desde su padre:
```tsx
function Boton({ texto }: { texto: string }) {
  return <button>{texto}</button>;
}
<Boton texto="Guardar" />
```

## Eventos
```tsx
<button onClick={() => alert('Hola')}>Saludar</button>
```
- `onClick={funcion}` pasa la función.
- `onClick={funcion()}` la ejecuta al evaluar el JSX (generalmente error).
- `onClick={() => funcion(id)}` difiere la llamada y pasa argumentos.

## Renderizado condicional
### Ternario
```tsx
<input type={mostrar ? 'text' : 'password'} />
```
### `&&`
```tsx
{error && <p role="alert">{error}</p>}
```
Si `error` es un string vacío, no aparece el párrafo. Cuidado: `{0 && <p>...</p>}` puede renderizar `0`; usa una condición booleana cuando corresponda.

### Listas y `key`
```tsx
const nombres = ['Ana', 'Luis'];
return <ul>{nombres.map(nombre => <li key={nombre}>{nombre}</li>)}</ul>;
```
`key` permite a React identificar elementos al actualizar la lista. Debe ser estable y único entre hermanos; evita índices cuando la lista se reordena.

## Fragment
```tsx
return <><h1>Título</h1><p>Descripción</p></>;
```
Agrupa sin introducir un `div` adicional.

## React actualiza la interfaz
Al cambiar props o estado, React vuelve a llamar al componente para calcular el siguiente árbol de elementos y aplicar al DOM los cambios necesarios. No significa que toda la página se recargue.

## Relacionadas
[[05 - JavaScript - Objetos, arrays y spread]] · [[08 - React - Estado, props y formularios]] · [[02 - HTML - Atributos, formularios y accesibilidad]]
