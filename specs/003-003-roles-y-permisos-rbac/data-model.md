# Data Model

**Date**: 2026-09-10

## Overview

Modelo relacional para la gestión de roles, permisos y asignaciones en un sistema RBAC, con auditoría y control de estado activo/inactivo.

## Entities

### Role

Representa un rol asignable a usuarios.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único del rol. |
| name | `string` | Yes | Nombre único del rol. |
| description | `string` | No | Descripción del rol. |
| is_active | `boolean` | Yes | Indica si el rol está activo. |
| created_at | `timestamp` | Yes | Fecha y hora de creación. |
| updated_at | `timestamp` | Yes | Fecha y hora de la última actualización. |
| created_by | `uuid` | Yes | Id del usuario que creó el rol. |
| updated_by | `uuid` | Yes | Id del usuario que actualizó el rol. |

### Permission

Representa un permiso individual que puede asignarse a roles.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único del permiso. |
| identifier | `string` | Yes | Identificador único del permiso. |
| description | `string` | No | Descripción del permiso. |
| created_at | `timestamp` | Yes | Fecha y hora de creación. |
| updated_at | `timestamp` | Yes | Fecha y hora de la última actualización. |
| created_by | `uuid` | Yes | Id del usuario que creó el permiso. |
| updated_by | `uuid` | Yes | Id del usuario que actualizó el permiso. |

### RolePermission

Tabla puente que asocia permisos a roles, evitando duplicaciones.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| role_id | `uuid` | Yes | Id del rol. |
| permission_id | `uuid` | Yes | Id del permiso. |
| created_at | `timestamp` | Yes | Fecha y hora de creación de la asociación. |

### UserRole

Tabla puente que asigna roles a usuarios.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| user_id | `uuid` | Yes | Id del usuario. |
| role_id | `uuid` | Yes | Id del rol. |
| assigned_at | `timestamp` | Yes | Fecha y hora de asignación. |

### AuditLog

Registro de auditoría de cambios en entidades críticas.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único del registro de auditoría. |
| action | `string` | Yes | Acción realizada (CREATE, UPDATE, DELETE). |
| entity | `string` | Yes | Nombre de la entidad afectada. |
| entity_id | `uuid` | Yes | Id de la entidad modificada. |
| old_value | `jsonb` | No | Estado anterior de la entidad. |
| new_value | `jsonb` | No | Estado posterior de la entidad. |
| performed_by | `uuid` | Yes | Id del usuario que realizó la acción. |
| performed_at | `timestamp` | Yes | Fecha y hora de la acción. |

## Relationships

| From | To | Type | Description |
|------|-----|------|-------------|
| RolePermission | Role | many-to-one | Cada asociación pertenece a un rol. |
| RolePermission | Permission | many-to-one | Cada asociación pertenece a un permiso. |
| UserRole | Role | many-to-one | Cada asignación pertenece a un rol. |
| UserRole | User | many-to-one | Cada asignación pertenece a un usuario. |

