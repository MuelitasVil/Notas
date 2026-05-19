# Frontend - Carpeta shared

## ¿Qué es shared?

La carpeta `shared` contiene elementos reutilizables en varios módulos.

No debe contener lógica específica de usuarios, reportes, organizaciones o jobs.

Debe contener piezas generales que puedan usarse en cualquier parte del sistema.

---

## Estructura sugerida

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

---

## components

Aquí van componentes reutilizables.

Ejemplos:

```txt
Button
Input
Select
Modal
Card
Badge
DataTable
ConfirmDialog
LoadingSpinner
ErrorMessage
```

### Diferencia importante

Un componente genérico va en `shared`.

Ejemplo:

```txt
shared/components/tables/DataTable.tsx
```

Un componente específico de negocio va en un módulo.

Ejemplo:

```txt
modules/users/components/UsersTable.tsx
```

---

## hooks

Los hooks compartidos son funciones reutilizables que encapsulan lógica de React.

Ejemplos:

```txt
useDebounce
usePagination
useModal
useLocalStorage
usePermissions
```

No deberían tener lógica demasiado específica de un módulo.

Por ejemplo:

```txt
shared/hooks/useDebounce.ts
```

Sí es compartido.

```txt
modules/users/hooks/useUsersSearch.ts
```

Pertenece al módulo de usuarios.

---

## utils

Los utils son funciones auxiliares puras.

Ejemplos:

```txt
formatDate
formatCurrency
buildQueryParams
downloadFile
capitalizeText
```

Normalmente no dependen de React.

Ejemplo:

```ts
formatDate("2026-05-18")
```

---

## types

Aquí van tipos globales o compartidos.

Ejemplos:

```txt
Pagination
ApiResponse
SelectOption
SortDirection
UserRole
```

Pero los tipos propios de usuarios deben ir en el módulo de usuarios.

Ejemplo:

```txt
modules/users/types/user.types.ts
```

---

## styles

Aquí pueden ir estilos globales o tokens de diseño.

Ejemplos:

```txt
global.css
variables.css
theme.ts
tailwind.css
```

---

## Duda: ¿los styles se comparten o deberían estar por módulos?

Ambas cosas pueden existir.

### Estilos globales

Van en `shared/styles` o directamente en `src/styles`.

Ejemplos:

```txt
Tipografía general
Colores base
Reset CSS
Variables globales
Configuración de Tailwind
Tema general
```

### Estilos por módulo

Van dentro del módulo cuando son específicos.

Ejemplo:

```txt
modules/users/styles/users-page.css
```

o usando clases dentro del componente.

---

## ¿Editar un módulo rompe la atomicidad?

No necesariamente.

No se rompe la atomicidad si una vista ajusta el tamaño, posición o distribución de un componente.

Ejemplo válido:

```tsx
<DataTable className="h-[650px]" />
```

La tabla sigue siendo reusable.

La vista solo decide cuánto espacio le da.

---

## Regla práctica

El componente reusable define:

```txt
Comportamiento general
Estructura base
Estilo base
Propiedades configurables
```

La vista o módulo define:

```txt
Tamaño
Ubicación
Contexto
Datos específicos
Columnas específicas
Acciones específicas
```

---

## Ejemplo correcto

```tsx
<DataTable
  columns={userColumns}
  data={users}
  height="large"
/>
```

Aquí `DataTable` es genérica, pero `userColumns` pertenece al módulo de usuarios.

---

## Ejemplo a evitar

```txt
DataTableUsers
DataTableReports
DataTableStudents
DataTableTeachers
DataTableBig
DataTableSmall
```

Esto puede generar duplicación innecesaria.

Mejor crear un solo componente configurable.
