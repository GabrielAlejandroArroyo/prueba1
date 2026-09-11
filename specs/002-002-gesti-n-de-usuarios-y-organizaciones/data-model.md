# Data Model

**Date**: 2026-09-10

## Overview

Modelo de datos para la gestión de usuarios y organizaciones, incluyendo entidades de organizaciones, perfiles de usuario, roles, capacidades y las relaciones que permiten la autorización basada en organización y en roles.

## Entities

### Organización

Representa una entidad empresarial que agrupa a usuarios bajo un contexto de negocio.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `bigint` | Yes | Identificador único de la organización. |
| code | `string` | Yes | Código alfanumérico único que identifica la organización. |
| name | `string` | Yes | Nombre legible de la organización. |
| description | `string` | No | Descripción adicional de la organización. |
| status | `enum` | Yes | Estado de la organización: ACTIVE o INACTIVE. |
| created_at | `timestamp` | Yes | Fecha y hora de creación. |
| updated_at | `timestamp` | Yes | Fecha y hora de la última actualización. |

### PerfilUsuario

Representa la cuenta de un usuario con credenciales y asociación a una organización.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `bigint` | Yes | Identificador único del usuario. |
| email | `string` | Yes | Correo electrónico del usuario, único. |
| name | `string` | No | Nombre completo del usuario. |
| password_hash | `string` | Yes | Hash de la contraseña. |
| status | `enum` | Yes | Estado del perfil: ACTIVE o INACTIVE. |
| organization_id | `bigint` | No | Identificador de la organización a la que pertenece. |
| is_super_admin | `boolean` | Yes | Indica si el usuario es super administrador. |
| created_at | `timestamp` | Yes | Fecha y hora de creación. |
| updated_at | `timestamp` | Yes | Fecha y hora de la última actualización. |

### Rol

Define un conjunto de permisos que pueden ser asignados a usuarios.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `bigint` | Yes | Identificador único del rol. |
| name | `string` | Yes | Nombre del rol. |
| description | `string` | No | Descripción del rol. |
| status | `enum` | Yes | Estado del rol: ACTIVE o INACTIVE. |
| created_at | `timestamp` | Yes | Fecha y hora de creación. |
| updated_at | `timestamp` | Yes | Fecha y hora de la última actualización. |

### Capacidad

Representa un permiso granular que puede ser asignado a roles.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `bigint` | Yes | Identificador único de la capacidad. |
| name | `string` | Yes | Nombre de la capacidad. |
| description | `string` | No | Descripción de la capacidad. |
| created_at | `timestamp` | Yes | Fecha y hora de creación. |
| updated_at | `timestamp` | Yes | Fecha y hora de la última actualización. |

### PerfilRol

Tabla de unión entre perfiles de usuario y roles.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| user_id | `bigint` | Yes | Identificador del perfil de usuario. |
| role_id | `bigint` | Yes | Identificador del rol. |

### RolCapacidad

Tabla de unión entre roles y capacidades.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| role_id | `bigint` | Yes | Identificador del rol. |
| capability_id | `bigint` | Yes | Identificador de la capacidad. |

### Notificación

Registra eventos de notificación enviados a usuarios.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `bigint` | Yes | Identificador único de la notificación. |
| user_id | `bigint` | Yes | Identificador del usuario receptor. |
| subject | `string` | Yes | Asunto de la notificación. |
| body | `text` | Yes | Cuerpo del mensaje. |
| sent_at | `timestamp` | Yes | Fecha y hora de envío. |

## Relationships

| From | To | Type | Description |
|------|-----|------|-------------|
| Organización | PerfilUsuario | one-to-many | Una organización puede tener múltiples perfiles de usuario asociados. |
| PerfilUsuario | Organización | many-to-one | Cada perfil de usuario pertenece a una organización (puede ser nulo para super admins). |
| PerfilUsuario | Rol | many-to-many | Un perfil de usuario puede tener múltiples roles a través de la tabla PerfilRol. |
| Rol | Capacidad | many-to-many | Un rol puede incluir múltiples capacidades a través de la tabla RolCapacidad. |
| Rol | PerfilUsuario | many-to-many | Un rol puede ser asignado a múltiples perfiles de usuario. |

