# Quickstart Guide

**Date**: 2026-09-10

## Overview

Guía rápida para implementar la gestión de usuarios y organizaciones según la especificación 002.

## Prerequisites

- Conocimientos básicos de JavaScript y Node.js
- Acceso a la API REST del backend
- Credenciales de administrador con permisos de gestión de organizaciones

## Steps

### 1. Paso 1: Crear una organización

Para crear una nueva organización, envía una solicitud POST al endpoint "/organizations" con los campos requeridos. El código debe ser único y el estado inicial será activo.

```javascript
fetch('https://api.ejemplo.com/organizations', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer <token>'
  },
  body: JSON.stringify({
    code: 'ORG001',
    name: 'Empresa XYZ',
    description: 'Organización de ejemplo',
    status: 'ACTIVE'
  })
})
.then(response => response.json())
.then(data => console.log('Organización creada:', data));
```

### 2. Paso 2: Editar una organización

Para modificar los detalles de una organización existente, utiliza el endpoint PUT con el identificador de la organización. Solo se permiten cambios en name, description y status.

```javascript
fetch('https://api.ejemplo.com/organizations/1', {
  method: 'PUT',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer <token>'
  },
  body: JSON.stringify({
    name: 'Empresa XYZ Actualizada',
    description: 'Descripción actualizada',
    status: 'ACTIVE'
  })
})
.then(response => response.json())
.then(data => console.log('Organización actualizada:', data));
```

### 3. Paso 3: Activar y desactivar una organización

Para cambiar el estado de una organización entre activo e inactivo, envía una solicitud PATCH. Cuando una organización está inactiva, no se pueden crear nuevos perfiles y los perfiles existentes no podrán iniciar sesión.

```javascript
fetch('https://api.ejemplo.com/organizations/1', {
  method: 'PATCH',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer <token>'
  },
  body: JSON.stringify({
    status: 'INACTIVE'
  })
})
.then(response => response.json())
.then(data => console.log('Estado actualizado:', data));
```

### 4. Paso 4: Asociar un usuario a una organización

Para asignar un usuario existente a una organización, envía una solicitud POST al endpoint que vincula usuario y organización. Se enviará una notificación al usuario.

```javascript
fetch('https://api.ejemplo.com/users/42/associate', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer <token>'
  },
  body: JSON.stringify({
    organization_id: 1
  })
})
.then(response => response.json())
.then(data => console.log('Usuario asociado:', data));
```

### 5. Paso 5: Ver roles y capacidades de un usuario

Para auditar los roles y capacidades de un usuario, consulta el endpoint GET que devuelve la información detallada, incluyendo los estados activos e inactivos.

```javascript
fetch('https://api.ejemplo.com/users/42/roles', {
  method: 'GET',
  headers: {
    'Authorization': 'Bearer <token>'
  }
})
.then(response => response.json())
.then(data => console.log('Roles y capacidades:', data));
```

### 6. Paso 6: Restricción de acceso por organización

Los administradores solo pueden gestionar usuarios dentro de su propia organización. Si intentan acceder a usuarios de otra organización, la API devolverá un error 403. No es necesario ningún código adicional; la lógica se implementa en el backend.

### 7. Paso 7: Super Admin gestionando todas las organizaciones

Un super administrador tiene privilegios ilimitados y puede crear, editar y borrar cualquier organización, así como asociar usuarios a cualquier organización. Al usar el token de super admin, las restricciones de organización se omiten.

