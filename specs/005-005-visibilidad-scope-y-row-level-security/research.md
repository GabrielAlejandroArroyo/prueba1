# Research: Investigación sobre la implementación de Scope y Row Level Security (RLS) en un entorno Supabase con enfoque en autenticación, autorización, aislamiento de datos y auditoría.

**Date**: 2026-09-10

## Overview

Investigación sobre la implementación de Scope y Row Level Security (RLS) en un entorno Supabase con enfoque en autenticación, autorización, aislamiento de datos y auditoría.

## Cálculo y manejo del Scope

El Scope representa el conjunto de recursos a los que un usuario puede acceder. Se calcula en un middleware de autenticación que extrae el rol y la organización del token JWT, asigna un Scope jerárquico y lo adjunta al contexto de la solicitud. Este proceso debe ser idempotente y reaccionar a cambios en la identidad del usuario.

### Key Points

- Utilizar un algoritmo de resolución de Scope basado en reglas de negocio definidas en un archivo JSON de configuración.
- Actualizar el Scope en el token JWT cada vez que el rol u organización cambie, evitando la necesidad de refrescar tokens manualmente.
- Incluir el Scope en el header `x-user-scope` para que los microservicios posteriores lo consuman sin decodificar el JWT.

## Filtros automáticos en consultas SELECT

Todas las consultas de lectura deben aplicar automáticamente el Scope mediante una capa de abstracción ORM o funciones de vista materializada. Esto garantiza que los desarrolladores no olviden añadir filtros y que la lógica de seguridad sea centralizada.

### Key Points

- Implementar un `scopeFilter()` que añada cláusulas `WHERE org_id = $1` o `WHERE role_id IN (...)` según corresponda.
- Configurar vistas con RLS activado para que el filtro sea aplicado a nivel de base de datos.
- Probar con casos de edge: acceso a registros con IDs modificados en la URL.

## Validación de Scope en operaciones DML

Las operaciones INSERT, UPDATE y DELETE deben validar que el registro pertenece al Scope del usuario antes de ejecutarse. Se recomienda usar triggers o funciones almacenadas que verifiquen la pertenencia y devuelvan un error 403 si no es válida.

### Key Points

- Crear un trigger `before_insert` que compare `NEW.org_id` con el Scope del usuario.
- En el caso de UPDATE, verificar `OLD.org_id = NEW.org_id` y que coincida con el Scope.
- Registrar cada intento fallido con la causa y el ID del recurso.

## Políticas Row Level Security en Supabase

RLS debe ser la última línea de defensa. Las políticas deben reflejar exactamente los Scopes calculados por el backend y aprovechar funciones de PostgreSQL como `auth.uid()` y `auth.role()`.

### Key Points

- Habilitar RLS en todas las tablas sensibles: `ALTER TABLE table ENABLE ROW LEVEL SECURITY;`
- Definir políticas `SELECT`, `INSERT`, `UPDATE`, `DELETE` con expresiones que comparen `org_id` con la variable de sesión `current_setting('app.scope')`.
- Usar `set_config('app.scope', '<scope>', true)` al inicio de cada sesión para que la política se aplique automáticamente.

## Prevención de manipulaciones externas

El sistema debe detectar y bloquear intentos de modificar parámetros de URL, IDs, o manipular el token JWT. Esto se logra mediante validaciones estrictas y auditoría centralizada.

### Key Points

- Validar que los IDs recibidos coincidan con los registros dentro del Scope antes de procesar la petición.
- Revalidar el JWT en cada endpoint, rechazando tokens con Scope no coincidente.
- Registrar cada bloqueo con la IP, usuario y motivo en un esquema de logs centralizado.

## Auditoría y trazabilidad

Los logs deben contener información completa: usuario, rol, Scope, acción, recurso y resultado. Se recomienda usar una librería de logging como Winston con formato JSON para facilitar la ingesta en sistemas SIEM.

### Key Points

- Incluir campos: `user_id`, `role`, `scope`, `action`, `resource_id`, `status`, `timestamp`.
- Enviar los logs a un endpoint seguro de centralización (e.g., Loki, ElasticSearch).
- Realizar pruebas de penetración enfocadas en la extracción de logs y la manipulación de Scope.

## Pruebas y validación

Para garantizar el aislamiento, se deben ejecutar pruebas unitarias, de integración y de penetración con diferentes combinaciones de roles y organizaciones.

### Key Points

- Jest + Supertest para pruebas de API con tokens de diferentes Scopes.
- Test de integración con Supabase que verifique que RLS bloquea accesos indebidos.
- Escenarios de edge: tokens manipulados, IDs cambiados, parámetros nulos.

