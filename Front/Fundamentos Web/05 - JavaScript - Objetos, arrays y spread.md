---
tags: [javascript, objetos, sintaxis, inmutabilidad]
---
# JavaScript: objetos, arrays y spread

## Objetos
Agrupan propiedades clave–valor.

```js
const usuario = { nombre: 'Ana', correo: 'ana@ejemplo.com', activo: true };
console.log(usuario.nombre);      // 'Ana'
console.log(usuario['correo']);    // 'ana@ejemplo.com'
```

### Acceso por propiedad vs propiedad calculada
```js
const campo = 'correo';
const a = { campo: 'x' };      // propiedad literal llamada "campo"
const b = { [campo]: 'x' };    // propiedad llamada "correo"
```

## Spread (`...`) en objetos
Copia superficialmente propiedades enumerables a un **objeto nuevo**.

```js
const anterior = { email: 'a@ejemplo.com', password: '123', remember: false };
const actualizado = { ...anterior, email: 'b@ejemplo.com' };
// actualizado = { email: 'b@ejemplo.com', password: '123', remember: false }
// anterior NO cambia
```

### Caso genérico: `[field]: value`
```js
function cambiarCampo(objeto, field, value) {
  return { ...objeto, [field]: value };
}
const resultado = cambiarCampo({ x: 1, y: 2 }, 'y', 99);
// { x: 1, y: 99 }
```

### ¿Qué es `prev`?
`prev` no es una palabra reservada; es el **nombre elegido para el estado anterior** en una función de actualización React.

```tsx
setDatos(prev => ({ ...prev, email: 'nuevo@ejemplo.com' }));
```
Usa una función actualizadora cuando el nuevo estado depende del anterior. Evita mutar el estado directamente.

### Copia superficial ≠ copia profunda
```js
const a = { perfil: { ciudad: 'Bogotá' } };
const b = { ...a };
b.perfil.ciudad = 'Cali';
console.log(a.perfil.ciudad); // 'Cali': comparten el objeto anidado
```
Para actualizar objetos anidados sin mutarlos:
```js
const b = { ...a, perfil: { ...a.perfil, ciudad: 'Cali' } };
```

## Spread en arrays
```js
const numeros = [1, 2];
const extendidos = [...numeros, 3]; // [1, 2, 3]
```

## Rest (`...`) en parámetros
La misma sintaxis tiene otra función: **agrupar** argumentos sobrantes.
```js
function sumar(...numeros) {
  return numeros.reduce((total, n) => total + n, 0);
}
sumar(1, 2, 3); // 6
```

## Destructuring
Extrae valores de objetos o arrays.
```js
const { nombre, activo } = usuario;
const [primero, segundo] = [10, 20];
```
La declaración `const [valor, setValor] = useState('')` usa **desestructuración de array**.

## Operadores frecuentes
| Código | Significado |
|---|---|
| `a && b` | Devuelve `b` si `a` es truthy; si no devuelve `a` |
| `cond ? a : b` | Ternario: elige `a` o `b` |
| `!valor` | Negación booleana |
| `obj?.prop` | Accede solo si `obj` no es null/undefined |
| `fn?.()` | Llama solo si `fn` no es null/undefined (y es invocable) |
| `a ?? b` | Usa `b` solo cuando `a` es null/undefined |
| `a || b` | Usa `b` cuando `a` es falsy |

```js
const etiqueta = usuario?.nombre ?? 'Sin nombre';
const visible = true;
const tipo = visible ? 'text' : 'password';
```

## Relacionadas
[[06 - JavaScript - Funciones, eventos y asincronía]] · [[07 - JSX - Sintaxis y renderizado condicional]] · [[08 - React - Estado, props y formularios]]
