# Data Model

**Date**: 2026-09-10

## Overview

Modelo de datos para la gestión de capacidades, asignaciones a roles y usuarios, control de scope por compañía y auditoría en un sistema basado en PostgreSQL y TypeScript.

## Entities

### Company

Representa una compañía dentro del sistema.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único de la compañía. |
| name | `string` | Yes | Nombre de la compañía. |
| created_at | `timestamp` | Yes | Fecha de creación. |
| updated_at | `timestamp` | Yes | Fecha de última actualización. |

### Capability

Representa una acción o permiso que puede ser asignado a roles o usuarios.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único de la capacidad. |
| name | `string` | Yes | Nombre único de la capacidad. |
| description | `string` | No | Descripción de la capacidad. |
| is_active | `boolean` | Yes | Indica si la capacidad está activa. |
| company_id | `uuid` | Yes | Empresa a la que pertenece la capacidad. |
| created_at | `timestamp` | Yes | Fecha de creación. |
| updated_at | `timestamp` | Yes | Fecha de última actualización. |

### Role

Grupo de usuarios con permisos agrupados.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único del rol. |
| name | `string` | Yes | Nombre del rol. |
| description | `string` | No | Descripción del rol. |
| is_active | `boolean` | Yes | Indica si el rol está activo. |
| company_id | `uuid` | Yes | Empresa a la que pertenece el rol. |
| created_at | `timestamp` | Yes | Fecha de creación. |
| updated_at | `timestamp` | Yes | Fecha de última actualización. |

### User

Representa un usuario del sistema.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único del usuario. |
| username | `string` | Yes | Nombre de usuario. |
| email | `string` | Yes | Correo electrónico. |
| password_hash | `string` | Yes | Hash de la contraseña. |
| is_active | `boolean` | Yes | Indica si el usuario está activo. |
| company_id | `uuid` | Yes | Empresa a la que pertenece el usuario. |
| created_at | `timestamp` | Yes | Fecha de creación. |
| updated_at | `timestamp` | Yes | Fecha de última actualización. |

### RoleCapability

Tabla de unión que asigna capacidades a roles.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único de la asignación. |
| role_id | `uuid` | Yes | Identificador del rol. |
| capability_id | `uuid` | Yes | Identificador de la capacidad. |
| created_at | `timestamp` | Yes | Fecha de creación. |
| updated_at | `timestamp` | Yes | Fecha de última actualización. |

### UserCapability

Tabla de unión que asigna capacidades directamente a usuarios.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único de la asignación. |
| user_id | `uuid` | Yes | Identificador del usuario. |
| capability_id | `uuid` | Yes | Identificador de la capacidad. |
| created_at | `timestamp` | Yes | Fecha de creación. |
| updated_at | `timestamp` | Yes | Fecha de última actualización. |

### AuditLog

Registro de auditoría de eventos del sistema.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único del log. |
| event_type | `string` | Yes | Tipo de evento (CRUD, asignación, uso, fallo de scope, etc.). |
| user_id | `uuid` | No | Usuario que originó el evento. |
| target_id | `uuid` | No | Entidad objetivo del evento. |
| target_type | `string` | No | Tipo de entidad objetivo (capability, role, user, etc.). |
| payload | `json` | No | Datos adicionales del evento. |
| timestamp | `timestamp` | Yes | Momento en que ocurrió el evento. |
| ip_address | `string` | No | IP desde la que se realizó la acción. |
| user_agent | `string` | No | User‑Agent del cliente. |

## Relationships

| From | To | Type | Description |
|------|-----|------|-------------|
| Capability | Company | many-to-one | Cada capacidad pertenece a una compañía. |
| Role | Company | many-to-one | Cada rol pertenece a una compañía. |
| User | Company | many-to-one | Cada usuario pertenece a una compañía. |
| RoleCapability | Role | many-to-one | Una asignación de capacidad pertenece a un rol. |
| RoleCapability | Capability | many-to-one | Una asignación de capacidad pertenece a una capacidad. |
| UserCapability | User | many-to-one | Una asignación directa pertenece a un usuario. |
| UserCapability | Capability | many-to-one | Una asignación directa pertenece a una capacidad. |
| AuditLog | User | many-to-one | El log referencia al usuario que realizó la acción. |
| AuditLog | Capability | many-to-one | El log puede referir a una capacidad objetivo. |
| AuditLog | Role | many-to-one | El log puede referir a un rol objetivo. |
| AuditLog | User | many-to-one | El log puede referir a un usuario objetivo. |

