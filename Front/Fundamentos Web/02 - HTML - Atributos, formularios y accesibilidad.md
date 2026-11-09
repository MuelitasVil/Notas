---
tags: [html, formularios, accesibilidad]
---
# HTML: atributos, formularios y accesibilidad

## ¿Qué es un atributo HTML?
Es una configuración colocada en la etiqueta de apertura. Puede definir identificación, comportamiento, metadatos o relaciones con otros elementos.

```html
<input id="correo" name="email" type="email" placeholder="Correo" required>
```

## Atributos frecuentes
| Atributo | Qué hace | Ejemplo |
|---|---|---|
| `id` | Identificador único en el documento | `id="correo"` |
| `class` | Agrupa elementos para CSS/JS | `class="campo"` |
| `name` | Nombre utilizado al enviar formularios | `name="email"` |
| `type` | Tipo de control | `type="password"` |
| `value` | Valor inicial o del control (según elemento/contexto) | `value="Ana"` |
| `placeholder` | Pista visual; **no sustituye una etiqueta** | `placeholder="Correo"` |
| `required` | Marca un campo obligatorio en validación nativa | `required` |
| `disabled` | Desactiva interacción; normalmente excluye el campo de FormData | `disabled` |
| `readonly` | Solo lectura; no equivale a `disabled` | `readonly` |
| `autocomplete` | Ayuda al navegador a completar | `autocomplete="current-password"` |
| `href` | Destino de un enlace | `href="/ayuda"` |
| `src` | Fuente de imagen o recurso | `src="logo.png"` |
| `alt` | Alternativa textual de una imagen | `alt="Logotipo"` |
| `title` | Información adicional; no reemplaza el texto accesible | `title="Ayuda"` |

## Formularios
`<form>` agrupa controles y permite enviarlos. El atributo `action` define la URL de envío nativo; `method` define el método (comúnmente `GET` o `POST`). Con JavaScript o React podemos interceptar el evento de envío.

```html
<form action="/login" method="post">
  <label for="email">Correo</label>
  <input id="email" name="email" type="email" required autocomplete="username">

  <label for="password">Contraseña</label>
  <input id="password" name="password" type="password" required autocomplete="current-password">

  <button type="submit">Ingresar</button>
  <button type="reset">Limpiar</button>
</form>
```

### `<label>` y `<input>`
`for="email"` se relaciona con `id="email"`. Hace que el nombre del control esté disponible para tecnologías de asistencia y que pulsar la etiqueta enfoque el control. **En JSX se escribe `htmlFor`**.

### Tipos de input
| Tipo | Comportamiento |
|---|---|
| `text` | Muestra texto normalmente |
| `password` | Oculta visualmente los caracteres (no cifra el dato) |
| `email` | Ofrece semántica y validación básica de correo |
| `number` | Entrada numérica; no es ideal para teléfonos o códigos |
| `checkbox` | Verdadero/falso; su estado se lee con `checked` |
| `radio` | Selección de una opción de un grupo |
| `date` | Selector de fecha según navegador |
| `file` | Selección de archivos |

```html
<input type="text" value="Clave123">
<input type="password" value="Clave123">
```
Los dos valores siguen siendo texto; `password` solo cambia su presentación.

### Botones
- `<button type="submit">`: envía el formulario.
- `<button type="button">`: acción que **no** envía el formulario (por ejemplo, mostrar contraseña).
- `<button type="reset">`: restablece controles nativos; en formularios React controlados quizá debas gestionar también el estado.

### `novalidate`
```html
<form novalidate>...</form>
```
Desactiva la validación interactiva nativa del navegador al enviar. Se usa cuando la aplicación muestra sus propios errores. **El servidor siempre debe validar de nuevo**. En JSX se escribe `noValidate`.

## Atributos de accesibilidad
- `aria-label`: nombre accesible para controles sin texto visible.
- `aria-invalid="true"`: indica que el valor es inválido.
- `aria-describedby="error-correo"`: enlaza un campo con un texto explicativo.
- `role="alert"`: anuncia un mensaje importante (con moderación).

```html
<label for="correo">Correo</label>
<input id="correo" type="email" aria-invalid="true" aria-describedby="error-correo">
<p id="error-correo">Introduce una dirección válida.</p>
```

**Seguridad:** `type="password"` no cifra contraseñas; HTTPS y validación del backend son responsabilidades separadas.

## Relacionadas
[[01 - HTML - Etiquetas y estructura]] · [[07 - JSX - Sintaxis y renderizado condicional]] · [[08 - React - Estado, props y formularios]]
