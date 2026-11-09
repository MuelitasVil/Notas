---
tags: [react, hooks, estado, formularios]
---
# React: estado, props y formularios

## Componente funcional
Es una función que devuelve elementos de interfaz JSX. Su nombre comienza con mayúscula.
```tsx
function Titulo() { return <h1>Hola</h1>; }
```

## Props: entrada del componente
Son valores que el padre pasa al hijo. El hijo **no debe mutarlas**.
```tsx
function Mensaje({ texto }: { texto: string }) { return <p>{texto}</p>; }
<Mensaje texto="Listo" />
```

## Estado: `useState`
Guarda información entre renderizados mientras el componente sigue montado.
```tsx
import { useState } from 'react';
function Contador() {
  const [valor, setValor] = useState(0);
  return <button onClick={() => setValor(v => v + 1)}>{valor}</button>;
}
```
`valor` = estado actual; `setValor` = función que solicita actualizarlo; `0` = valor inicial. `useState` devuelve un array y usamos destructuring.

### ¿Por qué `prev`?
```tsx
setPersona(prev => ({ ...prev, nombre: 'Ana' }));
```
`prev` recibe el estado anterior más reciente para esa actualización. Spread crea un **nuevo objeto**; evita modificar el anterior directamente.

## Estado local vs datos del servidor
- **Estado local:** input escrito, modal abierto, contraseña visible.
- **Datos del servidor:** usuarios consultados, jobs, reportes; suelen gestionarse con herramientas de consultas y caché.

## Inputs controlados
React mantiene el valor del input en estado y lo actualiza al escribir.
```tsx
function Formulario() {
  const [correo, setCorreo] = useState('');
  return <input value={correo} onChange={e => setCorreo(e.target.value)} />;
}
```
Un input con `value` controlado requiere actualizar ese valor a través de `onChange` si quieres que sea editable.

### Checkbox controlado
```tsx
const [acepto, setAcepto] = useState(false);
<input type="checkbox" checked={acepto} onChange={e => setAcepto(e.target.checked)} />
```

## Formulario con envío
```tsx
import { useState } from 'react';
function Formulario() {
  const [correo, setCorreo] = useState('');
  function handleSubmit(e: React.FormEvent<HTMLFormElement>) {
    e.preventDefault();
    console.log(correo);
  }
  return (
    <form onSubmit={handleSubmit}>
      <label htmlFor="correo">Correo</label>
      <input id="correo" value={correo} onChange={e => setCorreo(e.target.value)} />
      <button type="submit">Enviar</button>
    </form>
  );
}
```
En JSX, `onSubmit` es un **prop de evento**. `preventDefault()` evita la navegación/envío nativo si queremos gestionar la operación con JS.

## Componente hijo con callback
```tsx
type FormProps = { onSubmit: (correo: string) => void };
function Form({ onSubmit }: FormProps) {
  return <button onClick={() => onSubmit('ana@ejemplo.com')}>Enviar</button>;
}
function Page() {
  function procesar(correo: string) { console.log(correo); }
  return <Form onSubmit={procesar} />;
}
```
La función viaja **padre → hijo como prop**; el hijo la ejecuta para enviar información al padre. Es el patrón de *callback props*.

## Reglas de hooks
- Invócalos en el nivel superior de un componente o un hook personalizado.
- No los llames dentro de bucles, condiciones o funciones anidadas arbitrarias.
- Los cambios de estado no se reflejan necesariamente de forma inmediata en la misma ejecución; evita leer el estado recién actualizado como si fuera una asignación síncrona.

## Relacionadas
[[07 - JSX - Sintaxis y renderizado condicional]] · [[05 - JavaScript - Objetos, arrays y spread]] · [[06 - JavaScript - Funciones, eventos y asincronía]]
