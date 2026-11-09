---
tags: [glosario, desarrollo-web, referencia]
---
# Glosario y patrones frecuentes

| Término | Definición breve | Ejemplo |
|---|---|---|
| DOM | Árbol de nodos que el navegador expone para representar el documento | `document.querySelector('p')` |
| Atributo HTML | Información adicional de un elemento | `type="email"` |
| Selector CSS | Patrón para escoger elementos | `.card`, `button:hover` |
| Pseudoclase CSS | Estado o condición de un elemento | `:focus`, `:disabled` |
| CSS Module | CSS con nombres de clase locales generados en compilación | `styles.card` |
| Callback | Función pasada a otra función | `onClick={guardar}` |
| Spread | Expande propiedades o elementos | `{...obj}`, `[...lista]` |
| Rest | Reúne argumentos o propiedades sobrantes | `function f(...args)` |
| Destructuring | Extrae datos de objetos/arrays | `const {id} = user` |
| Promise | Representa un resultado asíncrono futuro | `fetch('/api')` |
| JSX | Sintaxis para describir interfaz en JS | `<h1>{titulo}</h1>` |
| Prop | Entrada pasada a un componente | `<Card title="Hola" />` |
| Estado | Dato conservado por React entre renders | `useState(false)` |
| Render | Evaluación del componente para producir UI | cambio de estado → nuevo render |
| Controlled input | Input cuyo valor depende de un estado de React | `value={email}` |
| Conditional rendering | Mostrar UI dependiendo de una condición | `{error && <p>{error}</p>}` |
| `aria-*` | Atributos para apoyar accesibilidad | `aria-invalid="true"` |

## Preguntas rápidas para autoevaluación
1. ¿En qué se diferencian `id`, `class` y `name`?
2. ¿Por qué `type="password"` no es una medida de cifrado?
3. ¿Qué cambia al utilizar `box-sizing: border-box`?
4. ¿Cómo se comportan `justify-content` y `align-items` si `flex-direction` cambia?
5. ¿Qué copia `...prev` y qué **no** copia?
6. ¿Qué diferencia hay entre `{prop: valor}` y `{[campo]: valor}`?
7. ¿Qué hace `finally` si el `await` falla?
8. ¿Por qué en JSX se escribe `className`?
9. ¿Cómo se distingue una prop de un estado en React?
10. ¿Qué diferencia existe entre `onClick={fn}` y `onClick={() => fn(id)}`?

## Referencias generales
- [MDN Web Docs: HTML](https://developer.mozilla.org/es/docs/Web/HTML)
- [MDN Web Docs: CSS](https://developer.mozilla.org/es/docs/Web/CSS)
- [MDN Web Docs: JavaScript](https://developer.mozilla.org/es/docs/Web/JavaScript)
- [React: Learn](https://react.dev/learn)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
