# Data Model

**Date**: 2026-09-10

## Overview

Modelo de datos basado en metadata para la gestión de maestros, con tablas genéricas que permiten crear, editar, consultar y desactivar maestros sin generar nuevas tablas.

## Entities

### master_definitions

Representa cada maestro de negocio.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único del maestro. |
| organization_id | `uuid` | Yes | Organización a la que pertenece. |
| code | `string` | Yes | Código único dentro de la organización. |
| name | `string` | Yes | Nombre del maestro. |
| description | `string` | No | Descripción del maestro. |
| status | `string` | Yes | Estado del maestro (active/inactive). |
| display_field | `string` | No | Nombre del campo que se muestra como descriptivo. |
| created_at | `timestamp` | Yes | Fecha de creación. |
| updated_at | `timestamp` | Yes | Fecha de última actualización. |
| created_by | `uuid` | Yes | Usuario que creó el registro. |
| updated_by | `uuid` | Yes | Usuario que actualizó el registro. |

### master_fields

Metadatos de los campos de cada maestro.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único del campo. |
| master_definition_id | `uuid` | Yes | Referencia al maestro. |
| field_name | `string` | Yes | Nombre interno del campo. |
| field_type | `string` | Yes | Tipo de dato (string, number, date, boolean, etc.). |
| is_visible | `boolean` | Yes | Indica si el campo es visible en vistas de lista. |
| is_required | `boolean` | Yes | Indica si el campo es obligatorio. |
| is_display_field | `boolean` | Yes | Indica si el campo es el display_field del maestro. |
| order | `integer` | No | Orden de aparición en la UI. |
| created_at | `timestamp` | Yes | Fecha de creación. |
| updated_at | `timestamp` | Yes | Fecha de última actualización. |

### master_definitions_audit

Histórico de cambios en los maestros.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único del registro de auditoría. |
| master_definition_id | `uuid` | Yes | Referencia al maestro auditado. |
| action | `string` | Yes | Tipo de acción (create, update, delete). |
| timestamp | `timestamp` | Yes | Fecha y hora de la acción. |
| user_id | `uuid` | Yes | Usuario que realizó la acción. |
| old_values | `jsonb` | No | Valores previos antes de la acción. |
| new_values | `jsonb` | No | Valores posteriores después de la acción. |

### master_permissions

Permisos de acceso a cada maestro por rol.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único del permiso. |
| master_definition_id | `uuid` | Yes | Referencia al maestro. |
| role_id | `uuid` | Yes | Identificador del rol. |
| can_create | `boolean` | Yes | Permite crear registros. |
| can_update | `boolean` | Yes | Permite actualizar registros. |
| can_read | `boolean` | Yes | Permite consultar registros. |
| can_delete | `boolean` | Yes | Permite eliminar registros. |

### master_relations

Define relaciones entre maestros sin crear tablas intermedias.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| id | `uuid` | Yes | Identificador único de la relación. |
| master_definition_id | `uuid` | Yes | Maestro origen. |
| related_master_definition_id | `uuid` | Yes | Maestro destino. |
| relation_type | `string` | Yes | Tipo de relación (one-to-one, one-to-many, many-to-many). |
| description | `string` | No | Descripción de la relación. |

## Relationships

| From | To | Type | Description |
|------|-----|------|-------------|
| master_definitions | master_fields | one-to-many | Un maestro tiene varios campos definidos. |
| master_definitions | master_permissions | one-to-many | Un maestro puede tener varios permisos asociados a roles. |
| master_definitions | master_relations | one-to-many | Un maestro puede estar involucrado en varias relaciones. |
| master_relations | master_definitions | many-to-one | La relación apunta a otro maestro como destino. |
| master_definitions | master_definitions_audit | one-to-many | Un maestro tiene un historial de auditoría. |

