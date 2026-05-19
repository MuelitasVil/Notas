# Frontend - Índice general

## Objetivo de estas notas

Estas notas explican cómo iniciar y organizar un proyecto frontend administrativo usando React, Vite y TypeScript.

Están pensadas para recordar:

- Por qué se eligieron estas tecnologías.
- Cómo se organiza un frontend moderno.
- Qué diferencia hay entre `main.tsx` y `App.tsx`.
- Qué debe ir en `app`, `shared`, `modules`, `clients` y `config`.
- Cómo evitar mezclar lógica de negocio, componentes reutilizables y comunicación con el backend.

---

## Contexto del proyecto

El sistema tiene dos servicios principales:

- Organizacional.
- WorkspaceManager.

El frontend únicamente debe consumir el servicio Organizacional.

```txt
Frontend -> Organizacional -> WorkspaceManager
```

El frontend no debe comunicarse directamente con WorkspaceManager.

---

## Caso de uso principal

El frontend debe permitir consultar usuarios institucionales mediante filtros como:

- Tipo de usuario.
- Organización.
- Periodo.
- Estado.
- Programa.
- Facultad.
- Proveedor externo.

En el peor caso, una consulta puede involucrar aproximadamente 60.000 usuarios.

Por eso, la interfaz debe trabajar con:

- Filtros server-side.
- Paginación server-side.
- Ordenamiento server-side.
- Tablas dinámicas.
- Selección masiva por criterio.
- Ejecución de jobs asíncronos.
- Consulta del estado de jobs.

---

## Notas principales

- [[01 - Inicio de proyecto y tecnologías seleccionadas]]
- [[02 - Estructura general del proyecto]]
- [[03 - Carpeta app]]
- [[04 - Carpeta shared]]
- [[05 - Carpeta modules]]
- [[06 - main.tsx vs App.tsx]]
- [[07 - Dudas pendientes]]

---

## Resumen rápido

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
