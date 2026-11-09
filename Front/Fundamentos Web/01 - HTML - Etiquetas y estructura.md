---
tags: [html, fundamentos, referencia]
---
# HTML: etiquetas y estructura

## ¿Qué es HTML?
**HTML (HyperText Markup Language)** es un lenguaje de marcado que describe el contenido y su estructura semántica. **No es un lenguaje de programación.** Sus etiquetas no determinan por sí solas la apariencia final.

```html
<!doctype html>
<html lang="es">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Mi página</title>
  </head>
  <body>
    <header><h1>Bienvenido</h1></header>
    <main><p>Contenido principal.</p></main>
    <footer>Pie de página</footer>
  </body>
</html>
```

## Etiqueta, elemento y contenido
```html
<p class="mensaje">Hola</p>
```
- `<p>`: etiqueta de apertura.
- `class="mensaje"`: atributo.
- `Hola`: contenido.
- `</p>`: etiqueta de cierre.
- El conjunto completo es un **elemento HTML**.

Algunas etiquetas son **vacías** y no requieren cierre: `<input>`, `<img>`, `<br>`, `<meta>`.

## Etiquetas estructurales y semánticas
| Etiqueta | Significado y uso |
|---|---|
| `<html>` | Raíz del documento |
| `<head>` | Metadatos y enlaces a recursos; no es el encabezado visible |
| `<body>` | Contenido visible de la página |
| `<header>` | Encabezado de una página o sección |
| `<nav>` | Bloque principal de navegación |
| `<main>` | Contenido principal de la página; normalmente uno por documento |
| `<section>` | Sección temática, normalmente con título |
| `<article>` | Contenido independiente (artículo, tarjeta de publicación) |
| `<aside>` | Contenido complementario |
| `<footer>` | Pie de página o sección |
| `<div>` | Contenedor genérico sin significado semántico particular |
| `<span>` | Contenedor genérico **en línea** |

> **Regla práctica:** prefiere etiquetas semánticas (`main`, `nav`, `section`) cuando describen el contenido; usa `div`/`span` si solo necesitas agrupar sin aportar significado.

## Texto y enlaces
| Etiqueta | Para qué sirve |
|---|---|
| `<h1>` ... `<h6>` | Jerarquía de encabezados |
| `<p>` | Párrafo |
| `<strong>` | Importancia fuerte (normalmente se muestra en negrita) |
| `<em>` | Énfasis (normalmente cursiva) |
| `<a>` | Hipervínculo (`href`) |
| `<ul>` | Lista no ordenada |
| `<ol>` | Lista ordenada |
| `<li>` | Elemento de lista |
| `<code>` | Código en línea |
| `<pre>` | Texto preformateado |
| `<button>` | Acción interactiva |

```html
<section aria-labelledby="titulo">
  <h2 id="titulo">Recursos</h2>
  <p>Visita la <a href="https://developer.mozilla.org/">documentación</a>.</p>
  <ul><li>HTML</li><li>CSS</li><li>JavaScript</li></ul>
</section>
```

## Elementos de bloque y de línea
En el comportamiento CSS inicial, un `div` normalmente ocupa toda una línea (`display: block`) y `span` fluye dentro del texto (`display: inline`). **CSS puede cambiar ese comportamiento**.

## Ejemplo de estructura de una página
```html
<header>Marca y navegación</header>
<main>
  <section>
    <h1>Título</h1>
    <article><h2>Noticia</h2><p>Descripción.</p></article>
  </section>
</main>
<footer>Información legal</footer>
```

## Errores habituales
- Usar `<div>` para todo: se pierde semántica y accesibilidad.
- Usar un `<a href="#">` como acción: para acciones suele corresponder `<button>`.
- Elegir `<h3>` por su tamaño visual en vez de por jerarquía; el tamaño se define con CSS.
- Confundir `<head>` con `<header>`.

## Relacionadas
[[02 - HTML - Atributos, formularios y accesibilidad]] · [[03 - CSS - Selectores y modelo de caja]] · [[07 - JSX - Sintaxis y renderizado condicional]]
