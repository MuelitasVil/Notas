# Frontend - Carpeta app

## ¿Qué es la carpeta app?

La carpeta `app` contiene la configuración principal de la aplicación React.

No representa un módulo de negocio, sino el núcleo general de la app.

Aquí van elementos como:

- Rutas globales.
- Providers.
- Layouts principales.
- Componente App.

---

## Estructura sugerida

```txt
app/
├── router/
│   └── AppRouter.tsx
├── providers/
│   ├── QueryProvider.tsx
│   └── AuthProvider.tsx
├── layouts/
│   ├── PublicLayout.tsx
│   └── PrivateLayout.tsx
└── App.tsx
```

---

## ¿Qué son los routers?

Los routers definen las rutas de navegación de la aplicación.

Ejemplo:

```txt
/login
/users
/reports
/jobs
```

En React normalmente se usa `react-router-dom`.

Ejemplo conceptual:

```tsx
<Route path="/login" element={<LoginPage />} />
<Route path="/users" element={<UsersPage />} />
```

---

## ¿Qué son los providers?

Los providers son componentes que envuelven la aplicación para dar acceso global a cierta funcionalidad.

Ejemplos:

- `QueryProvider`: permite usar TanStack Query en toda la app.
- `AuthProvider`: permite acceder al usuario autenticado.
- `ThemeProvider`: permite manejar tema visual.
- `RouterProvider`: permite manejar rutas.

Un provider normalmente envuelve toda la aplicación.

---

## ¿Qué son los layouts?

Los layouts son estructuras visuales generales.

Ejemplo:

### PublicLayout

Para pantallas públicas:

- Login.
- Recuperar contraseña.
- Registro, si aplica.

### PrivateLayout

Para pantallas internas:

- Sidebar.
- Header.
- Menú de usuario.
- Contenedor principal.
- Módulos internos.

Ejemplo:

```txt
PrivateLayout
├── Sidebar
├── Header
└── PageContent
```

---

## ¿Qué debería ir en App.tsx?

`App.tsx` debería ser un componente muy limpio.

Normalmente solo conecta providers y rutas.

Ejemplo conceptual:

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

## Regla importante

`App.tsx` no debería tener lógica de negocio.

No debería consultar usuarios, crear jobs ni manejar filtros.

Esa lógica debe estar en los módulos correspondientes.
