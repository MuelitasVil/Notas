---
tags: [css, flexbox, grid, responsive]
---
# CSS: layout, responsive y estilos modulares

## `display`: cómo se distribuye un elemento
- `block`: suele ocupar el ancho disponible y empezar línea nueva.
- `inline`: fluye dentro del texto y no admite todas las dimensiones como un bloque.
- `inline-block`: fluye en línea pero acepta ancho/alto.
- `flex`: organiza hijos en un eje principal.
- `grid`: organiza hijos en filas y columnas.
- `none`: no se representa ni ocupa espacio de layout.

## Flexbox
```css
.contenedor {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  gap: 16px;
}
```
- `flex-direction: row`: eje principal horizontal (por defecto).
- `flex-direction: column`: eje principal vertical.
- `justify-content`: alinea en el **eje principal**.
- `align-items`: alinea en el **eje transversal**.
- `gap`: separa hijos sin añadir márgenes individuales.
- `flex-wrap: wrap`: permite saltos de línea.

### Ejemplo: tarjeta centrada
```css
.pagina {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}
.tarjeta { width: 100%; max-width: 400px; }
```

**Importante:** si cambias `flex-direction` a `column`, los ejes cambian: `justify-content` pasa a ser vertical.

## CSS Grid
```css
.panel {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 24px;
}
```
Útil para diseños bidimensionales (sidebar + contenido, galerías, dashboards).

## Position
- `static`: flujo normal.
- `relative`: se desplaza respecto de su posición original; puede ser referencia para hijos absolutos.
- `absolute`: sale del flujo normal y se posiciona respecto del ancestro posicionado más próximo.
- `fixed`: normalmente relativo al viewport.
- `sticky`: alterna entre flujo normal y posición fijada al desplazarse, según límites de scroll.

## Responsive: media queries
```css
.panel { grid-template-columns: 240px 1fr; }
@media (max-width: 700px) {
  .panel { grid-template-columns: 1fr; }
}
```
Cambian reglas según características del dispositivo/viewport. Usa anchuras fluidas y límites (`max-width`) en lugar de tamaños rígidos siempre que sea posible.

## CSS global vs CSS local
### Global
Estilos compartidos: reset, tipografía, tokens de color, fondos generales.

```css
/* global.css */
:root { font-family: system-ui, sans-serif; }
* { box-sizing: border-box; }
```

### CSS Modules (React + Vite)
El archivo `Button.module.css` contiene nombres de clase locales al módulo.

```css
/* Button.module.css */
.primary { background: #6366f1; color: white; }
```
```tsx
import styles from './Button.module.css';
export function Button() { return <button className={styles.primary}>Guardar</button>; }
```
El import crea un mapa de nombres de clases; la compilación genera identificadores únicos. **No es lo mismo que importar un CSS global**.

## Dudas frecuentes
- ¿Personalizar un componente rompe reutilización? **No**: un componente puede aceptar `className`, variantes o tamaños. Evita que conozca reglas específicas de un dominio si es compartido.
- ¿Todas las páginas deben compartir un CSS? **No**: global para reglas transversales, local para estilos específicos.
- ¿Cuándo Flex vs Grid? Flex para alineación en un eje; Grid para estructuras en dos dimensiones (aunque ambos pueden resolver muchos layouts).

## Relacionadas
[[03 - CSS - Selectores y modelo de caja]] · [[07 - JSX - Sintaxis y renderizado condicional]]
