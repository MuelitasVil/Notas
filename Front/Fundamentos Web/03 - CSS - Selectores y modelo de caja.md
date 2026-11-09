---
tags: [css, fundamentos, referencia]
---
# CSS: selectores y modelo de caja

## ¿Qué es CSS?
**Cascading Style Sheets** define la presentación de HTML: colores, tipografía, bordes, espaciado, tamaño y distribución.

```css
.tarjeta {
  background-color: white;
  color: #172033;
  padding: 24px;
  border: 1px solid #ddd;
  border-radius: 8px;
}
```

- `.tarjeta` = selector.
- `padding` = propiedad.
- `24px` = valor.
- Las declaraciones se separan con `;`.

## Selectores comunes
| Selector | Significado |
|---|---|
| `p` | Todos los párrafos |
| `.tarjeta` | Elementos con clase `tarjeta` |
| `#titulo` | Elemento con `id="titulo"` |
| `.tarjeta p` | Párrafos descendientes de `.tarjeta` |
| `.tarjeta > p` | Párrafos hijos directos |
| `input[type="email"]` | Inputs cuyo tipo sea email |
| `button:hover` | Botón bajo el puntero |
| `input:focus` | Input con foco |
| `button:disabled` | Botón deshabilitado |
| `:focus-visible` | Foco que requiere indicador visible (p. ej. teclado) |

```css
input:focus { border-color: #6366f1; }
button:disabled { opacity: 0.5; cursor: not-allowed; }
```

## Cascada, herencia y especificidad
- **Cascada:** determina qué declaración se aplica cuando varias coinciden.
- **Herencia:** ciertas propiedades (como `color`) se transmiten a descendientes, otras (como `margin`) no.
- **Especificidad:** algunos selectores prevalecen sobre otros; también importan el origen, la importancia y el orden.

```css
p { color: blue; }
.aviso { color: red; } /* gana para <p class="aviso">, si nada más interviene */
```

## Modelo de caja (box model)
De afuera hacia adentro: **margin → border → padding → content**.

| Propiedad | Efecto |
|---|---|
| `margin` | Espacio fuera del borde |
| `border` | Contorno de la caja |
| `padding` | Espacio entre borde y contenido |
| `width`, `height` | Tamaño del contenido con `content-box`, o tamaño total con `border-box` |

```css
* { box-sizing: border-box; }
.card { width: 300px; padding: 20px; border: 2px solid; }
```
Con `border-box`, el ancho total es 300 px. Con `content-box`, sería 344 px (300 + 20 + 20 + 2 + 2).

## Unidades comunes
- `px`: píxeles CSS.
- `%`: relativo al contexto, según la propiedad.
- `rem`: relativo al tamaño de fuente del elemento raíz.
- `em`: relativo al tamaño de fuente del elemento pertinente, según la propiedad.
- `vh` / `vw`: fracción de dimensiones del viewport.
- `dvh`: altura dinámica del viewport, útil en móviles.

## Variables CSS (custom properties)
```css
:root { --primary: #6366f1; --radius: 8px; }
.button { background: var(--primary); border-radius: var(--radius); }
```
Permiten reutilizar valores o crear temas visuales.

## Relacionadas
[[04 - CSS - Layout, responsive y estilos modulares]] · [[01 - HTML - Etiquetas y estructura]]
