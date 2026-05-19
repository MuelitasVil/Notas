# Frontend - Carpeta modules

## ¿Qué son los módulos?

Los módulos representan dominios funcionales del sistema.

Cada módulo agrupa todo lo relacionado con una parte del negocio.

Ejemplos:

```txt
auth
users
organizations
reports
jobs
```

---

## Ejemplo de módulo users

```txt
modules/users/
├── pages/
│   └── UsersPage.tsx
├── components/
│   ├── UsersFilters.tsx
│   ├── UsersTable.tsx
│   ├── UsersBulkActions.tsx
│   └── UserStatusBadge.tsx
├── api/
│   └── users.api.ts
├── hooks/
│   ├── useUsersSearch.ts
│   └── useUserActions.ts
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

## pages

Contiene las pantallas principales del módulo.

Ejemplo:

```txt
UsersPage
UserDetailPage
```

Una page normalmente une componentes, hooks y lógica de presentación.

---

## components

Componentes propios del módulo.

Ejemplo:

```txt
UsersFilters
UsersTable
UsersBulkActions
```

Estos componentes no necesariamente deberían usarse fuera del módulo.

---

## api

Contiene funciones que llaman al backend.

Ejemplo:

```ts
searchUsers()
getUserById()
createUser()
updateUser()
```

La carpeta `api` se comunica con el cliente HTTP.

---

## hooks

Conectan React con la lógica del módulo.

Ejemplo:

```ts
useUsersSearch()
useCreateUser()
useUpdateUser()
```

Normalmente usan TanStack Query.

---

## services

Contienen lógica de aplicación.

No siempre son necesarios.

Son útiles cuando hay que transformar datos o construir reglas antes de llamar al backend.

Ejemplo:

```ts
buildUserSyncJobPayload(filters)
```

---

## schemas

Contienen validaciones.

Ejemplo:

```txt
usersFilters.schema.ts
```

Se pueden usar con Zod para validar formularios.

---

## types

Contienen tipos específicos del módulo.

Ejemplo:

```ts
type User = {
  id: string;
  name: string;
  email: string;
}
```

---

## mappers

Transforman datos entre backend y frontend.

Ejemplo:

```txt
UserDto -> User
```

Esto es útil porque el backend puede tener nombres o estructuras diferentes a las que necesita la interfaz.

---

## Regla importante

Cada módulo debe intentar ser independiente.

El módulo de usuarios puede usar cosas de `shared`, pero `shared` no debería depender del módulo de usuarios.

Correcto:

```txt
modules/users -> shared
```

Incorrecto:

```txt
shared -> modules/users
```
