# Frontend - main.tsx vs App.tsx

## ¿Qué es main.tsx?

`main.tsx` es el punto de entrada técnico de la aplicación.

Es el archivo que conecta React con el HTML principal.

Normalmente busca el elemento:

```html
<div id="root"></div>
```

y monta la aplicación React ahí.

Ejemplo:

```tsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./app/App";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

## ¿Qué es App.tsx?

`App.tsx` es el componente raíz de la aplicación.

Define la estructura principal de la app.

Normalmente contiene:

- Providers.
- Router.
- Layouts principales.
- Configuración visual general.

Ejemplo:

```tsx
function App() {
  return (
    <AppProviders>
      <AppRouter />
    </AppProviders>
  );
}

export default App;
```

---

## Diferencia principal

```txt
main.tsx
  Punto de entrada técnico.
  Monta React en el DOM.

App.tsx
  Punto de entrada lógico.
  Organiza providers, rutas y estructura general.
```

---

## Analogía

`main.tsx` es como prender el motor.

`App.tsx` es como definir el tablero principal de la aplicación.

---

## Qué NO debería ir en main.tsx

Evitar poner:

```txt
Rutas
Pantallas
Lógica de negocio
Llamadas al backend
Filtros
Tablas
Permisos
```

---

## Qué NO debería ir en App.tsx

Evitar poner:

```txt
Lógica específica de usuarios
Consultas de reportes
Creación de jobs
Manejo de tablas
Validaciones específicas de formularios
```

---

## Resumen

```txt
main.tsx:
Arranca React.

App.tsx:
Organiza la aplicación.

modules:
Contienen la lógica del negocio.

shared:
Contiene piezas reutilizables.
```
