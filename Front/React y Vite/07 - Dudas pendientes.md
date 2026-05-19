# Frontend - Dudas pendientes

## 1. ¿Cómo iniciar correctamente un proyecto frontend?

Pendiente revisar:

- Creación con Vite.
- Instalación de librerías base.
- Configuración de TypeScript.
- Configuración de rutas.
- Configuración de cliente HTTP.
- Variables de entorno.
- Layout público y privado.
- Manejo de autenticación.

---

## 2. ¿Qué debe ir en app?

Pendiente profundizar:

- Router.
- Providers.
- Layouts.
- App.tsx.
- Separación entre configuración global y lógica de negocio.

---

## 3. ¿Qué debe ir en shared?

Pendiente profundizar:

- Componentes reutilizables.
- Hooks reutilizables.
- Utilidades.
- Tipos globales.
- Estilos globales.
- Diferencia entre componente global y componente de negocio.

---

## 4. ¿Los estilos deben estar en shared o por módulo?

Respuesta inicial:

Pueden existir ambos.

- Estilos globales en `shared/styles`.
- Estilos específicos dentro de cada módulo.

No se rompe la atomicidad si una vista ajusta tamaño, posición o distribución de un componente reusable.

---

## 5. ¿Qué son los módulos?

Respuesta inicial:

Son carpetas que agrupan todo lo relacionado con un dominio funcional.

Ejemplos:

- auth
- users
- organizations
- reports
- jobs

---

## 6. ¿Cuál es la diferencia entre main.tsx y App.tsx?

Respuesta inicial:

```txt
main.tsx -> monta React en el DOM.
App.tsx  -> organiza la aplicación.
```

`main.tsx` es técnico.

`App.tsx` es lógico.

---

## 7. Dudas futuras sobre la tabla

Pendiente definir:

- AG Grid o MUI X Data Grid.
- Columnas base.
- Filtros iniciales.
- Paginación server-side.
- Ordenamiento server-side.
- Selección por página.
- Selección por criterio.
- Exportación CSV desde backend.
- Ejecución de jobs masivos.
