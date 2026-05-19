# Frontend - Inicio de proyecto y tecnologías seleccionadas

## Objetivo

Crear una aplicación frontend administrativa para consultar usuarios, aplicar filtros, ejecutar procesos masivos y consultar estados de jobs.

La aplicación debe ser mantenible, modular y fácil de extender.

---

# 1. Tecnologías seleccionadas

## Stack recomendado

```txt
React
TypeScript
Vite
React Router
TanStack Query
Axios
React Hook Form
Zod
AG Grid o MUI X Data Grid
Tailwind + shadcn/ui o Material UI
```

---

# 2. ¿Por qué React?

Se selecciona React porque:

- Ya existe experiencia previa con JavaScript y React.
- Permite construir interfaces dinámicas basadas en componentes.
- Tiene un ecosistema amplio para tablas, formularios, rutas, validaciones y consumo de APIs.
- Es adecuado para aplicaciones administrativas internas.
- Facilita dividir la interfaz en componentes reutilizables.

Este proyecto no es principalmente una página estática o informativa, sino una aplicación con interacción constante:

```txt
Filtros
Tablas
Paginación
Selección de usuarios
Acciones masivas
Consulta de jobs
Estados de carga
Manejo de errores
```

Por eso React encaja bien.

---

# 3. ¿Por qué TypeScript?

TypeScript se usa para darle tipado al frontend.

Aunque en backend es común pensar en entidades, DTOs y contratos, en frontend también es importante controlar las estructuras de datos.

Ejemplo:

```ts
type User = {
  id: string;
  fullName: string;
  email: string;
  userType: string;
  organizationName: string;
  status: string;
};
```

Ventajas de TypeScript:

- Reduce errores al consumir APIs.
- Ayuda a documentar los datos esperados.
- Facilita trabajar con DTOs del backend.
- Mejora el autocompletado.
- Permite detectar errores antes de ejecutar la aplicación.
- Hace más mantenible el proyecto cuando crece.

En este proyecto es importante porque habrá datos como:

```txt
Usuarios
Organizaciones
Periodos
Jobs
Filtros
Paginación
Ordenamiento
Permisos
```

---

# 4. ¿Por qué Vite?

Vite se usa como herramienta para crear y compilar el proyecto frontend.

Vite no es un framework completo como Next.js o Astro.

Vite sirve para:

- Crear el proyecto.
- Ejecutar el servidor de desarrollo.
- Compilar el frontend para producción.
- Trabajar rápido durante el desarrollo.

Se selecciona Vite porque:

- Es simple.
- Es rápido.
- Tiene buena integración con React.
- No agrega una arquitectura innecesaria.
- Es adecuado para aplicaciones tipo SPA administrativas.

---

# 5. ¿Por qué no Next.js como primera opción?

Next.js es muy potente, pero en este caso puede ser más de lo necesario.

Next.js es muy útil cuando se necesita:

- SEO.
- Renderizado del lado del servidor.
- Páginas públicas indexables.
- Sitios con contenido público.
- Backend for Frontend dentro del mismo framework.

Pero este proyecto parece más una aplicación administrativa interna:

```txt
Login
Dashboard
Usuarios
Reportes
Jobs
Filtros
Tablas
Acciones internas
```

Por eso, para iniciar de forma simple y mantenible:

```txt
React + Vite
```

es una mejor decisión inicial.

---

# 6. ¿Por qué no Astro como framework principal?

Astro es muy bueno para sitios donde la mayoría del contenido es estático o informativo.

Ejemplos donde Astro funciona muy bien:

```txt
Landing pages
Blogs
Documentación
Portales institucionales
Sitios de contenido
```

Pero este proyecto necesita una interfaz altamente interactiva.

El usuario va a estar haciendo:

```txt
Filtrar datos
Cambiar tablas
Seleccionar registros
Crear procesos masivos
Consultar estados
Ver resultados
Interactuar con modales
```

En ese contexto, Astro podría terminar siendo solo un contenedor para componentes React.

Por eso, Astro podría servir para:

```txt
Manual público
Documentación
Portal informativo
Landing institucional
```

Pero no sería la mejor opción para el panel administrativo principal.

---

# 7. ¿Por qué React Router?

React Router permite manejar rutas internas de la aplicación.

Ejemplos:

```txt
/login
/users
/reports
/jobs
/organizations
```

Se selecciona porque la aplicación tendrá varias vistas protegidas.

Ejemplo:

```txt
/login             -> vista pública
/users             -> vista privada
/reports/users     -> vista privada
/jobs              -> vista privada
```

---

# 8. ¿Por qué TanStack Query?

TanStack Query sirve para manejar datos que vienen del backend.

En backend normalmente uno piensa:

```txt
Controller -> Service -> Repository
```

En frontend, muchas veces el problema es:

```txt
¿Cómo consulto datos?
¿Cómo sé si están cargando?
¿Cómo manejo errores?
¿Cómo refresco datos?
¿Cómo cacheo respuestas?
¿Cómo vuelvo a consultar el estado de un job?
```

TanStack Query ayuda con eso.

Sirve para:

- Consultar APIs.
- Manejar loading.
- Manejar errores.
- Cachear datos.
- Refrescar información.
- Hacer polling.
- Invalidar consultas.
- Sincronizar datos del backend con la interfaz.

En este proyecto es especialmente útil para consultar jobs.

Ejemplo conceptual:

```ts
useQuery({
  queryKey: ["job-status", jobId],
  queryFn: () => getJobStatus(jobId),
  refetchInterval: 3000
});
```

Esto permite consultar periódicamente el estado de un job sin implementar lógica manual compleja.

---

# 9. ¿Por qué Axios?

Axios se usa como cliente HTTP.

Sirve para centralizar la comunicación con el backend Organizacional.

Permite configurar:

- URL base.
- Headers.
- Token JWT.
- Interceptores.
- Manejo de errores.
- Timeout.

Ejemplo conceptual:

```ts
const httpClient = axios.create({
  baseURL: import.meta.env.VITE_ORGANIZATIONAL_API_URL
});
```

---

# 10. ¿Por qué React Hook Form y Zod?

React Hook Form ayuda a manejar formularios.

Zod ayuda a validar datos.

En este proyecto habrá formularios de filtros:

```txt
Tipo de usuario
Organización
Periodo
Estado
Programa
Facultad
```

React Hook Form permite manejar el formulario de forma eficiente.

Zod permite validar que los filtros tengan la estructura correcta.

Ejemplo:

```ts
const filtersSchema = z.object({
  userType: z.string().optional(),
  organizationId: z.string().optional(),
  periodId: z.string().optional(),
  status: z.string().optional()
});
```

---

# 11. ¿Por qué AG Grid o MUI X Data Grid?

La tabla es uno de los componentes más importantes del proyecto.

El sistema puede llegar a manejar consultas de hasta 60.000 usuarios.

Por eso la tabla debe soportar:

- Paginación.
- Filtros.
- Ordenamiento.
- Selección.
- Columnas configurables.
- Loading states.
- Acciones por fila.
- Acciones masivas.
- Integración con backend.

AG Grid es una opción fuerte si la tabla es el centro del sistema.

MUI X Data Grid es una buena opción si se quiere una interfaz más integrada con Material UI.

---

# 12. Caso de uso específico del proyecto

El usuario entra al frontend y filtra estudiantes por:

```txt
Tipo de usuario: Estudiante
Organización: Facultad X
Periodo: 2026-1
Estado: Activo
```

El frontend no debe traer los 60.000 usuarios en una sola petición.

Debe enviar los filtros al backend Organizacional:

```json
{
  "filters": {
    "userType": "STUDENT",
    "organizationId": "FAC_001",
    "periodId": "2026-1",
    "status": "ACTIVE"
  },
  "pagination": {
    "page": 0,
    "pageSize": 100
  },
  "sorting": {
    "field": "lastName",
    "direction": "asc"
  }
}
```

El backend responde una página de resultados:

```json
{
  "items": [],
  "page": 0,
  "pageSize": 100,
  "total": 60000
}
```

---

# 13. Acciones masivas

Si el usuario quiere sincronizar todos los usuarios filtrados, el frontend no debe enviar 60.000 IDs.

Debe enviar el criterio de selección:

```json
{
  "processType": "SYNC_GOOGLE_GROUPS",
  "selectionMode": "FILTER_CRITERIA",
  "filters": {
    "userType": "STUDENT",
    "organizationId": "FAC_001",
    "periodId": "2026-1",
    "status": "ACTIVE"
  }
}
```

Luego Organizacional crea el job y WorkspaceManager lo procesa por lotes.

---

# 14. Orden recomendado para iniciar el proyecto

```txt
1. Crear proyecto con Vite + React + TypeScript.
2. Configurar estructura de carpetas.
3. Configurar rutas.
4. Configurar providers.
5. Configurar cliente HTTP.
6. Crear layout público.
7. Crear layout privado.
8. Crear login básico.
9. Crear módulo users.
10. Crear filtros de usuarios con datos mock.
11. Crear tabla con datos mock.
12. Conectar tabla con backend.
13. Agregar paginación server-side.
14. Agregar filtros server-side.
15. Agregar ordenamiento server-side.
16. Agregar acciones masivas.
17. Crear módulo jobs.
18. Consultar estado de jobs.
19. Agregar manejo de errores.
20. Agregar permisos.
```

---

# 15. Resumen mental

```txt
React:
Construir interfaz dinámica.

TypeScript:
Evitar errores de datos y contratos.

Vite:
Crear y compilar el proyecto de forma simple y rápida.

React Router:
Manejar rutas.

TanStack Query:
Manejar datos del backend.

Axios:
Cliente HTTP centralizado.

React Hook Form:
Formularios.

Zod:
Validación.

AG Grid / MUI X Data Grid:
Tablas dinámicas grandes.
```
