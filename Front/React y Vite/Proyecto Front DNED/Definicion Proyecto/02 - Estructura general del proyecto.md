# Frontend - Estructura general del proyecto

## Objetivo

Definir una estructura de carpetas clara para una aplicación frontend administrativa.

La estructura debe ayudar a:

- Separar responsabilidades.
- Evitar archivos gigantes.
- Evitar mezclar lógica de negocio con componentes genéricos.
- Facilitar el crecimiento del sistema.
- Facilitar que un desarrollador backend pueda ubicarse rápidamente.

---

# 1. Estructura recomendada

```txt
src/
├── app/
│   ├── router/
│   ├── providers/
│   ├── layouts/
│   └── App.tsx
│
├── config/
│   ├── env.ts
│   └── constants.ts
│
├── clients/
│   ├── httpClient.ts
│   └── organizationalClient.ts
│
├── shared/
│   ├── components/
│   ├── hooks/
│   ├── utils/
│   ├── types/
│   └── styles/
│
├── modules/
│   ├── auth/
│   ├── users/
│   ├── organizations/
│   ├── reports/
│   └── jobs/
│
└── main.tsx
```

---

# 2. Idea principal de la estructura

Esta estructura mezcla dos enfoques:

```txt
Estructura por responsabilidad global
+
Estructura por dominio funcional
```

Es decir:

```txt
app      -> configuración principal de la aplicación
config   -> configuración general
clients  -> comunicación base con APIs
shared   -> elementos reutilizables
modules  -> dominios del negocio
main.tsx -> punto de entrada técnico
```

---

# 3. ¿Por qué no organizar todo por capas como backend?

En backend es común organizar así:

```txt
controllers/
services/
repositories/
models/
dtos/
```

Ese enfoque funciona muy bien para backend porque normalmente la aplicación se organiza alrededor de entrada, lógica y persistencia.

Pero en frontend el problema principal es diferente.

En frontend se trabaja con:

```txt
Pantallas
Componentes
Estados
Formularios
Datos del backend
Eventos del usuario
Layouts
Rutas
```

Por eso, una estructura solo por capas puede terminar así:

```txt
components/
pages/
services/
hooks/
types/
```

Y con el tiempo todo se mezcla.

Ejemplo de problema:

```txt
components/
├── UsersTable.tsx
├── JobsTable.tsx
├── LoginForm.tsx
├── UserStatusBadge.tsx
├── ReportFilters.tsx
├── OrganizationSelect.tsx
```

Después de un tiempo no es claro qué pertenece a usuarios, qué pertenece a reportes y qué es realmente reutilizable.

---

# 4. Beneficio de usar modules

La carpeta `modules` permite agrupar por dominio.

Ejemplo:

```txt
modules/users/
├── pages/
├── components/
├── api/
├── hooks/
├── services/
├── types/
├── schemas/
└── mappers/
```

Todo lo relacionado con usuarios vive dentro de `modules/users`.

Esto permite ubicar rápidamente:

```txt
Vista de usuarios
Filtros de usuarios
Tabla de usuarios
Tipos de usuarios
Llamadas HTTP de usuarios
Hooks de usuarios
Validaciones de filtros de usuarios
```

---

# 5. Analogía con backend

En backend se podría pensar así:

```txt
UserController
UserService
UserRepository
UserDto
UserEntity
```

En frontend, el módulo users puede tener:

```txt
UsersPage
UsersFilters
UsersTable
users.api.ts
useUsersSearch
user.types.ts
usersFilters.schema.ts
user.mapper.ts
```

No es exactamente igual, pero la idea de separar el dominio se mantiene.

---

# 6. ¿Qué va en app?

La carpeta `app` contiene la configuración central de la aplicación.

No es un módulo de negocio.

Aquí van:

```txt
Rutas
Providers
Layouts
App.tsx
```

Ejemplo:

```txt
app/
├── router/
│   └── AppRouter.tsx
├── providers/
│   ├── AppProviders.tsx
│   ├── QueryProvider.tsx
│   └── AuthProvider.tsx
├── layouts/
│   ├── PublicLayout.tsx
│   └── PrivateLayout.tsx
└── App.tsx
```

## Regla

`app` organiza la aplicación, pero no contiene lógica específica de usuarios, reportes o jobs.

---

# 7. ¿Qué va en config?

La carpeta `config` contiene configuración general.

Ejemplos:

```txt
Variables de entorno
Constantes globales
Configuración de permisos
Configuración de rutas
```

Ejemplo:

```txt
config/
├── env.ts
├── constants.ts
└── permissions.ts
```

Ejemplo conceptual:

```ts
export const env = {
  organizationalApiUrl: import.meta.env.VITE_ORGANIZATIONAL_API_URL
};
```

---

# 8. ¿Qué va en clients?

La carpeta `clients` contiene la comunicación base con servicios externos.

En este proyecto, el frontend solo debería tener cliente para Organizacional.

```txt
clients/
├── httpClient.ts
└── organizationalClient.ts
```

## Importante

El frontend no debe tener:

```txt
workspaceManagerClient.ts
```

Porque el frontend no debe comunicarse directamente con WorkspaceManager.

La comunicación correcta es:

```txt
Frontend -> Organizacional -> WorkspaceManager
```

---

# 9. ¿Qué va en shared?

La carpeta `shared` contiene elementos reutilizables en todo el sistema.

Ejemplos:

```txt
Botones
Inputs
Selects
Modales
Tablas genéricas
Hooks reutilizables
Funciones auxiliares
Tipos globales
Estilos globales
```

Estructura:

```txt
shared/
├── components/
│   ├── ui/
│   ├── forms/
│   ├── tables/
│   └── feedback/
├── hooks/
├── utils/
├── types/
└── styles/
```

## Regla

`shared` no debe depender de módulos específicos.

Correcto:

```txt
modules/users -> shared
```

Incorrecto:

```txt
shared -> modules/users
```

---

# 10. ¿Qué va en modules?

La carpeta `modules` contiene los dominios funcionales del sistema.

Ejemplos:

```txt
auth
users
organizations
reports
jobs
```

Cada módulo puede tener:

```txt
pages
components
api
hooks
services
types
schemas
mappers
```

Ejemplo:

```txt
modules/users/
├── pages/
│   └── UsersPage.tsx
├── components/
│   ├── UsersFilters.tsx
│   ├── UsersTable.tsx
│   └── UsersBulkActions.tsx
├── api/
│   └── users.api.ts
├── hooks/
│   └── useUsersSearch.ts
├── services/
│   └── usersJob.service.ts
├── schemas/
│   └── usersFilters.schema.ts
├── types/
│   └── user.types.ts
└── mappers/
    └── user.mapper.ts
```

---

# 11. ¿Por qué las capas están diferentes?

Porque en frontend no todo se organiza igual que en backend.

En esta estructura hay dos tipos de separación:

## Separación horizontal

Son elementos usados por toda la aplicación.

Ejemplos:

```txt
app
config
clients
shared
```

Estos elementos son transversales.

## Separación vertical

Son dominios de negocio.

Ejemplos:

```txt
modules/auth
modules/users
modules/jobs
modules/reports
```

Cada módulo agrupa su propia lógica.

---

# 12. Beneficios de esta estructura

## 1. Escala mejor

Si mañana crece el módulo de usuarios, no contamina todo el proyecto.

```txt
modules/users
```

puede crecer internamente sin afectar `reports` o `jobs`.

## 2. Es fácil encontrar archivos

Si necesito modificar filtros de usuarios:

```txt
modules/users/components/UsersFilters.tsx
```

Si necesito modificar la consulta al backend:

```txt
modules/users/api/users.api.ts
```

Si necesito modificar el hook de búsqueda:

```txt
modules/users/hooks/useUsersSearch.ts
```

## 3. Evita mezclar componentes genéricos con componentes de negocio

Un botón es genérico:

```txt
shared/components/ui/Button.tsx
```

Una tabla de usuarios es de negocio:

```txt
modules/users/components/UsersTable.tsx
```

## 4. Facilita reutilizar sin duplicar

Se puede tener una tabla genérica:

```txt
shared/components/tables/DataTable.tsx
```

Y luego especializarla en un módulo:

```txt
modules/users/components/UsersTable.tsx
```

## 5. Permite pensar por dominio

En vez de tener todos los hooks juntos:

```txt
hooks/
├── useUsersSearch.ts
├── useJobsStatus.ts
├── useOrganizations.ts
```

Se organizan por módulo:

```txt
modules/users/hooks/useUsersSearch.ts
modules/jobs/hooks/useJobStatus.ts
modules/organizations/hooks/useOrganizations.ts
```

Esto evita que el proyecto se vuelva difícil de navegar.

---

# 13. Otras posibles estructuras

## Opción A: estructura simple por tipo de archivo

```txt
src/
├── components/
├── pages/
├── services/
├── hooks/
├── types/
└── utils/
```

### Ventajas

- Fácil de entender al inicio.
- Buena para proyectos pequeños.
- Rápida de crear.

### Desventajas

- Escala mal.
- Todo queda mezclado.
- No es claro qué pertenece a cada dominio.
- Puede crecer de forma desordenada.

Esta opción sirve para proyectos pequeños, pero no es ideal para este sistema.

---

## Opción B: estructura por capas tipo backend

```txt
src/
├── controllers/
├── services/
├── repositories/
├── models/
└── views/
```

### Ventajas

- Familiar para alguien de backend.
- Separa responsabilidades técnicas.

### Desventajas

- No se adapta tan bien a React.
- Puede volver artificial la organización del frontend.
- React trabaja más naturalmente con componentes, hooks y páginas.
- No refleja bien la relación entre pantalla, estado y componente.

No es la mejor opción para este caso.

---

## Opción C: estructura por Atomic Design

```txt
src/
├── atoms/
├── molecules/
├── organisms/
├── templates/
└── pages/
```

### Ventajas

- Muy buena para sistemas de diseño.
- Ayuda a construir componentes reutilizables.
- Ordena componentes visuales por complejidad.

### Desventajas

- Puede ser confusa si el proyecto tiene mucha lógica de negocio.
- No siempre es claro dónde poner componentes específicos.
- Puede generar debates innecesarios sobre si algo es átomo, molécula u organismo.
- No organiza bien APIs, hooks, tipos y servicios de dominio.

Atomic Design puede servir como inspiración para `shared/components`, pero no lo usaría como estructura principal del proyecto.

---

## Opción D: estructura por features o módulos

```txt
src/
├── features/
│   ├── auth/
│   ├── users/
│   ├── jobs/
│   └── reports/
├── shared/
└── app/
```

### Ventajas

- Escala bien.
- Agrupa por dominio.
- Es clara para aplicaciones grandes.
- Evita mezclar lógica.

### Desventajas

- Puede parecer más compleja al inicio.
- Requiere disciplina para decidir qué va en shared y qué va en cada módulo.

Esta es muy parecida a la estructura recomendada.

---

# 14. Estructura seleccionada

La estructura seleccionada es una variante de arquitectura por módulos:

```txt
app
config
clients
shared
modules
main.tsx
```

Se selecciona porque el proyecto tiene:

- Varios dominios.
- Usuarios.
- Organizaciones.
- Jobs.
- Reportes.
- Tablas dinámicas.
- Filtros.
- Procesos masivos.
- Comunicación con backend.
- Crecimiento esperado.

---

# 15. Preguntas para saber dónde ubicar un archivo

## ¿Es una pantalla completa?

Va en:

```txt
modules/<dominio>/pages
```

Ejemplo:

```txt
modules/users/pages/UsersPage.tsx
```

## ¿Es un componente usado en varios módulos?

Va en:

```txt
shared/components
```

Ejemplo:

```txt
shared/components/ui/Button.tsx
```

## ¿Es un componente específico de usuarios?

Va en:

```txt
modules/users/components
```

Ejemplo:

```txt
modules/users/components/UsersFilters.tsx
```

## ¿Es una llamada HTTP específica de usuarios?

Va en:

```txt
modules/users/api
```

Ejemplo:

```txt
modules/users/api/users.api.ts
```

## ¿Es el cliente HTTP base?

Va en:

```txt
clients
```

Ejemplo:

```txt
clients/httpClient.ts
```

## ¿Es una función genérica?

Va en:

```txt
shared/utils
```

Ejemplo:

```txt
shared/utils/formatDate.ts
```

## ¿Es un hook genérico?

Va en:

```txt
shared/hooks
```

Ejemplo:

```txt
shared/hooks/useDebounce.ts
```

## ¿Es un hook específico de usuarios?

Va en:

```txt
modules/users/hooks
```

Ejemplo:

```txt
modules/users/hooks/useUsersSearch.ts
```

## ¿Es un tipo global?

Va en:

```txt
shared/types
```

Ejemplo:

```txt
shared/types/pagination.types.ts
```

## ¿Es un tipo específico de usuarios?

Va en:

```txt
modules/users/types
```

Ejemplo:

```txt
modules/users/types/user.types.ts
```

---

# 16. Resumen mental

```txt
main.tsx
  Arranca React.

app
  Organiza la aplicación.

config
  Guarda configuración global.

clients
  Centraliza comunicación HTTP.

shared
  Guarda cosas reutilizables.

modules
  Guarda lógica por dominio.
```

---

# 17. Regla más importante

Si algo pertenece claramente a un dominio, debe ir en ese módulo.

Si algo puede ser usado por varios dominios, debe ir en shared.

Si algo configura toda la aplicación, debe ir en app o config.

Si algo comunica con el backend de forma base, debe ir en clients.
