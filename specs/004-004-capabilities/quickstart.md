# Quickstart Guide

**Date**: 2026-09-10

## Overview

Guía rápida para implementar y probar la gestión de capacidades, asignaciones a roles y usuarios, y la validación de scope por compañía en una aplicación TypeScript con Express y PostgreSQL.

## Prerequisites

- Node.js 20.x instalado
- PostgreSQL 15 con una base de datos disponible
- Acceso a un terminal con `npm` o `pnpm`
- Conocimientos básicos de Express, TypeORM y Jest/Vitest

## Steps

### 1. Paso 1: Clonar el repositorio y configurar variables de entorno

Clona el proyecto y copia el archivo `.env.example` a `.env`. Asegúrate de que las variables `DATABASE_URL`, `JWT_SECRET` y `LOG_LEVEL` estén definidas correctamente.

```bash
git clone https://github.com/tu-org/proyecto-capabilities.git
cd proyecto-capabilities
cp .env.example .env
# Edita .env con tus valores

```

### 2. Paso 2: Instalar dependencias y ejecutar migraciones

Instala las dependencias y ejecuta las migraciones de TypeORM para crear las tablas del modelo de datos.

```bash
npm install
npx typeorm migration:run
```

### 3. Paso 3: Iniciar el servidor en modo desarrollo

Arranca el servidor Express con hot‑reload para pruebas rápidas.

```bash
npm run dev
```

### 4. Paso 4: Crear una nueva capacidad vía API

Utiliza `curl` o Postman para enviar una solicitud POST a `/api/capabilities`. La capacidad debe pertenecer a la misma compañía que el usuario autenticado.

```bash
curl -X POST http://localhost:3000/api/capabilities \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <TOKEN_ADMIN>" \
  -d '{"name":"MANAGE_USERS","description":"Permite crear y eliminar usuarios"}'
```

### 5. Paso 5: Asignar la capacidad a un rol

Envía una solicitud POST a `/api/roles/{roleId}/capabilities` con el ID de la capacidad recién creada. El API evita duplicados automáticamente.

```bash
curl -X POST http://localhost:3000/api/roles/123e4567-e89b-12d3-a456-426614174000/capabilities \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <TOKEN_ADMIN>" \
  -d '{"capabilityId":"abcd1234-5678-90ef-1234-567890abcdef"}'
```

### 6. Paso 6: Asignar la capacidad directamente a un usuario

Para otorgar un permiso sin modificar el rol, usa `/api/users/{userId}/capabilities`. El sistema verificará que el usuario no ya tenga la capacidad a través de su rol.

```bash
curl -X POST http://localhost:3000/api/users/987e6543-e21b-12d3-a456-426614174999/capabilities \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <TOKEN_ADMIN>" \
  -d '{"capabilityId":"abcd1234-5678-90ef-1234-567890abcdef"}'
```

### 7. Paso 7: Consultar capacidades efectivas de un usuario

Accede a `/api/users/{userId}/effective-capabilities` para ver todas las capacidades activas, indicando su origen (rol o directa).

```bash
curl -X GET http://localhost:3000/api/users/987e6543-e21b-12d3-a456-426614174999/effective-capabilities \
  -H "Authorization: Bearer <TOKEN_USER>"
```

### 8. Paso 8: Probar la restricción de scope por compañía

Intenta ejecutar una acción que requiera una capacidad en una entidad que pertenece a otra compañía. Deberías recibir un error 403 con mensaje claro y un registro en `AuditLog`.

```bash
curl -X POST http://localhost:3000/api/entities/456e7890-e12b-34c5-a678-123456789abc/disable \
  -H "Authorization: Bearer <TOKEN_USER>" \
  -d '{}' \
  -H "Content-Type: application/json"
```

### 9. Paso 9: Verificar auditoría de eventos

Consulta la tabla `AuditLog` o usa la API `/api/audit-logs` para comprobar que cada asignación, eliminación y uso de capacidades se haya registrado con el usuario, la entidad objetivo y la razón.

```bash
curl -X GET http://localhost:3000/api/audit-logs \
  -H "Authorization: Bearer <TOKEN_ADMIN>"
```

