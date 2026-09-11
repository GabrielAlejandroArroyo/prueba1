# Research: El presente informe analiza las mejores prácticas y decisiones técnicas para implementar un sistema de control de acceso basado en roles (RBAC) en una aplicación Node.js con TypeScript y PostgreSQL, considerando requisitos de auditoría, seguridad y rendimiento.

**Date**: 2026-09-10

## Overview

El presente informe analiza las mejores prácticas y decisiones técnicas para implementar un sistema de control de acceso basado en roles (RBAC) en una aplicación Node.js con TypeScript y PostgreSQL, considerando requisitos de auditoría, seguridad y rendimiento.

## Arquitectura de RBAC

Se propone una arquitectura de capa única con un módulo de API REST que expone endpoints para la gestión de roles, permisos y asignaciones. La capa de dominio abstrae la lógica de negocio y utiliza repositorios para la interacción con la base de datos, garantizando separación de responsabilidades y facilitando la prueba unitaria.

### Key Points

- Separación de responsabilidades: API, dominio y persistencia.
- Uso de DTOs para validar y transformar datos entrantes.
- Middleware de autorización que verifica JWT y los roles antes de acceder a los controladores.

## Persistencia y Modelado de Datos

El modelo relacional incluye las tablas `roles`, `permissions`, `role_permissions` y `user_roles`. Se añaden columnas de estado (`is_active`) y auditoría (`created_at`, `updated_at`, `created_by`, `updated_by`). Se emplean índices únicos sobre `name` en `roles` y `identifier` en `permissions` para garantizar unicidad.

### Key Points

- Índices únicos en `roles.name` y `permissions.identifier`.
- Columna `is_active` para desactivar roles sin borrarlos.
- Triggers o funciones de auditoría para registrar cambios en cada tabla.

## Gestión de Roles y Permisos

Los endpoints CRUD de roles y permisos validan la unicidad antes de la inserción y actualización. La asociación de permisos a roles se implementa con una tabla puente que evita duplicaciones mediante una clave única compuesta.

### Key Points

- Validación de nombre único en la capa de servicio.
- Clave única compuesta `(role_id, permission_id)` en `role_permissions`.
- Restricción de asignación de roles inactivos a usuarios.

## Seguridad y Validaciones

Se emplea `jsonwebtoken` para autenticación y `class-validator` para validaciones de entrada. Los endpoints críticos están protegidos con middleware que comprueba la presencia de roles y permisos específicos, devolviendo 403 cuando la autorización falla.

### Key Points

- JWT con firma HMAC o RSA según el entorno.
- Middleware de autorización basado en la lista de permisos del token.
- Respuesta 403 consistente y sin información sensible.

## Políticas de Row-Level Security (RLS)

Para datos sensibles, se activan políticas RLS en PostgreSQL que filtran filas según el rol del usuario. Las políticas se definen en la capa de infraestructuras y se aplican mediante funciones PL/pgSQL que consultan la tabla `user_roles`.

### Key Points

- Política `SELECT` que permite solo filas con `owner_id = current_setting('jwt.claims.sub')`.
- Uso de `pg_set_role` en funciones para simular roles de base de datos.
- Pruebas de auditoría que verifican el aislamiento de datos.

## Auditoría y Registro de Cambios

Se implementa una tabla `audit_logs` que registra `action`, `entity`, `entity_id`, `old_value`, `new_value`, `performed_by` y `performed_at`. Los triggers de PostgreSQL se activan en las tablas críticas para capturar inserciones, actualizaciones y eliminaciones.

### Key Points

- Triggers `AFTER INSERT/UPDATE/DELETE` que insertan en `audit_logs`.
- Campos JSONB para almacenar estados previos y posteriores.
- Endpoint de consulta de auditoría con paginación y filtros.

## Pruebas y Validación

Se utilizan Vitest para pruebas unitarias de los servicios y Playwright para pruebas end-to-end de los flujos de administración. Los tests cubren casos de borde como creación de roles duplicados, asignación de roles inactivos y verificación de RLS.

### Key Points

- Cobertura > 90% en lógica de dominio.
- Test de autorización que intenta acceder a rutas con 403.
- Mock de base de datos con `pg-mock` para simular RLS.

