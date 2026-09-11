# Research: Investigación de decisiones técnicas y mejores prácticas para la gestión de capacidades, asignaciones a roles y usuarios, control de scope por compañía y auditoría en un sistema TypeScript/Express/PostgreSQL.

**Date**: 2026-09-10

## Overview

Investigación de decisiones técnicas y mejores prácticas para la gestión de capacidades, asignaciones a roles y usuarios, control de scope por compañía y auditoría en un sistema TypeScript/Express/PostgreSQL.

## Modelo de Datos y Unicidad

El modelo debe reflejar la relación muchos‑a‑muchos entre capacidades, roles y usuarios, además de la activación/desactivación. Se recomienda usar tablas de unión con claves foráneas y restricciones de unicidad.

### Key Points

- Tabla `capabilities` con columnas: id, name (único, índice B-tree), description, is_active, created_at, updated_at.
- Tablas `role_capabilities` y `user_capabilities` con claves compuestas (role_id, capability_id) y (user_id, capability_id) para evitar duplicados.
- Índices compuestos sobre `role_id, capability_id` y `user_id, capability_id` para búsquedas rápidas.
- Trigger de PostgreSQL para validar unicidad de `name` sin depender de la capa de aplicación.

## Control de Scope por Compañía

Cada acción que utiliza una capacidad debe comprobar que la entidad objetivo pertenece a la misma compañía del usuario. Este control debe estar centralizado para evitar duplicación de lógica.

### Key Points

- Agregar columna `company_id` en `users`, `roles` y `capabilities` (si aplica).
- Middleware de autorización que recupere la compañía del token JWT y compare con la entidad objetivo.
- Uso de funciones PL/pgSQL o vistas materializadas para filtrar rápidamente las capacidades válidas por compañía.
- Mensajes de error claros con códigos HTTP 403 y payload JSON explicativo.

## Asignaciones Directas y Heredadas

El sistema debe distinguir entre capacidades heredadas de roles y asignaciones directas, y evitar duplicados. Además, la eliminación de una asignación a un rol no debe afectar a usuarios que la posean directamente.

### Key Points

- Al asignar una capacidad a un usuario, verificar primero si la capacidad ya está activa a través de algún rol; si es así, rechazar la asignación.
- Al remover una capacidad de un rol, emitir un evento que actualice la vista de capacidades efectivas de los usuarios afectados.
- Implementar un servicio de cálculo de capacidades efectivas que combine consultas a `role_capabilities` y `user_capabilities` con filtros por `is_active` y `company_id`.
- Cachear resultados por sesión o token para reducir carga en peticiones frecuentes.

## Auditoría y Registro de Eventos

Todas las operaciones CRUD y de asignación deben registrarse con contexto completo para trazabilidad y cumplimiento.

### Key Points

- Tabla `audit_logs` con columnas: id, event_type, user_id, target_id, target_type, payload, timestamp, ip_address, user_agent.
- Usar middleware de Express que capture cada cambio y lo inserte en la tabla.
- Para acciones de uso (ej. ejecutar una capacidad), registrar intentos fallidos con motivo de scope no coincidente.
- Mantener los logs en un esquema separado y aplicar rotación de logs con retention policies.

## API REST y Seguridad

Los endpoints deben seguir principios RESTful, con autenticación JWT, autorización basada en roles y capacidades.

### Key Points

- Endpoints: `POST /capabilities`, `PUT /capabilities/:id`, `DELETE /capabilities/:id`, `GET /capabilities`, `POST /roles/:id/capabilities`, `DELETE /roles/:id/capabilities/:capId`, `POST /users/:id/capabilities`, `DELETE /users/:id/capabilities/:capId`.
- Middleware `authorize(capability)` que verifique la capacidad requerida antes de entrar al controlador.
- Rate limiting y protección contra CSRF en endpoints críticos.
- Documentar la API con OpenAPI 3.0 y generar clientes TypeScript automáticamente.

## Testing y Calidad

La cobertura de pruebas debe incluir unitarias, de integración y end-to-end, especialmente en edge cases.

### Key Points

- Vitest para pruebas unitarias de servicios y repositorios.
- Pruebas de integración con una base de datos PostgreSQL en Docker, usando migraciones.
- Playwright para pruebas E2E de la UI y flujo de asignaciones.
- Cobertura mínima 90% en líneas y branches.
- Linting con ESLint y formateo con Prettier.

## Escalabilidad y Rendimiento

La arquitectura debe soportar miles de usuarios y capacidades sin degradación.

### Key Points

- Uso de índices compuestos y consultas optimizadas con `EXISTS` en lugar de `JOIN` cuando sea posible.
- Cacheo de capacidades efectivas en Redis con TTL de 5 minutos.
- Batching de operaciones de asignación para reducir número de transacciones.
- Monitoreo con Prometheus y Grafana para métricas de latencia y throughput.

## Manejo de Edge Cases

Se deben cubrir escenarios complejos para evitar inconsistencias.

### Key Points

- Asignar capacidad a usuario que ya la posee a través de rol: respuesta 409 Conflict con detalle.
- Remover capacidad de rol con usuarios activos: emitir evento `role_capability_removed` y actualizar caches.
- Crear capacidad con nombre similar en otro idioma: usar colación `C` y unicidad a nivel de cadena.
- Asignar capacidad a usuario de otra compañía: validar `company_id` en el middleware de autorización.
- Desactivar capacidad asignada: marcar `is_active = false` y revocar en tiempo real mediante WebSocket.

