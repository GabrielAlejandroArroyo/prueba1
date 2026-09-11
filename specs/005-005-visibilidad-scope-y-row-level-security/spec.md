# 005 - Visibilidad, Scope y Row Level Security

> **Input**: User description
> **Date**: 2026-09-04
> **Status**: Specify

## User Stories

### US01: Determinación del Scope efectivo de cada usuario (Priority: P1)

Como administrador de backend, quiero que el sistema calcule automáticamente el Scope efectivo basado en el rol y la organización del usuario, para garantizar que solo tenga acceso a los recursos permitidos.

#### Acceptance Criteria

- [ ] El Scope se determina correctamente para cada rol (Super Admin, Admin, Manager, Member, Client).
- [ ] El Scope se actualiza cuando cambia el rol o la organización del usuario.
- [ ] El Scope se incluye en el token JWT y se valida en cada solicitud.

### US02: Aplicación de Scope a consultas de lectura (Priority: P1)

Como usuario, quiero que todas las consultas de lectura respeten mi Scope, para que no pueda ver datos de otras organizaciones o registros no asignados.

#### Acceptance Criteria

- [ ] Las consultas SELECT incluyen filtros automáticos basados en el Scope.
- [ ] Los resultados devueltos cumplen con las restricciones de Scope.
- [ ] No se devuelve ningún dato que pertenezca a otro Scope.

### US03: Aplicación de Scope a operaciones de modificación (Priority: P1)

Como usuario, quiero que las operaciones de creación, actualización y eliminación se realicen solo sobre registros dentro de mi Scope, para evitar modificaciones no autorizadas.

#### Acceptance Criteria

- [ ] Las operaciones DML (INSERT, UPDATE, DELETE) incluyen verificaciones de Scope.
- [ ] Los intentos de modificar registros fuera del Scope resultan en error 403.
- [ ] Los logs registran el Scope y la acción realizada.

### US04: Aislamiento entre organizaciones (Priority: P1)

Como desarrollador, quiero que los datos de una organización estén completamente aislados de los de otra, para cumplir con requisitos de privacidad y cumplimiento.

#### Acceptance Criteria

- [ ] No se permite acceder a datos de otra organización incluso con credenciales de alto nivel.
- [ ] Los índices y particiones están configurados para optimizar el aislamiento.
- [ ] Las pruebas de integración verifican la separación de datos.

### US05: Implementación de políticas RLS en Supabase (Priority: P1)

Como DBA, quiero que se apliquen políticas Row Level Security en PostgreSQL para reforzar la seguridad a nivel de base de datos.

#### Acceptance Criteria

- [ ] Todas las tablas relevantes tienen RLS habilitado.
- [ ] Las políticas RLS reflejan los mismos Scopes definidos en el backend.
- [ ] Las pruebas de seguridad confirman que RLS bloquea accesos indebidos.

### US06: Prevención de acceso mediante manipulación directa de API (Priority: P2)

Como auditor, quiero que el sistema detecte y bloquee intentos de acceso directo a endpoints sin validación de Scope.

#### Acceptance Criteria

- [ ] Los endpoints verifican el Scope antes de procesar la solicitud.
- [ ] Los intentos de acceso sin Scope o con Scope inválido reciben respuesta 401/403.
- [ ] Los logs registran el origen y motivo del bloqueo.

### US07: Prevención de acceso modificando parámetros o identificadores (Priority: P2)

Como tester, quiero que el sistema no permita acceder a recursos cambiando parámetros de URL o identificadores en la solicitud.

#### Acceptance Criteria

- [ ] Los endpoints validan que el identificador pertenece al Scope del usuario.
- [ ] Los intentos de acceder a identificadores fuera del Scope resultan en error 403.
- [ ] Los logs documentan la violación de Scope.

### US08: Aplicación consistente del modelo en cualquier frontend (Priority: P2)

Como arquitecto, quiero que el modelo de Scope funcione de la misma manera en todos los frontends (web, móvil, API), para mantener la coherencia de seguridad.

#### Acceptance Criteria

- [ ] Los clientes reciben el mismo token JWT con Scope.
- [ ] Las validaciones de Scope se realizan exclusivamente en el backend.
- [ ] Los frontends no pueden alterar el Scope de manera que afecte la autorización.

## Edge Cases

- El usuario intenta acceder a un registro de otra organización cambiando el ID en la URL.
- El token JWT es manipulado para incluir un Scope más amplio.
- El usuario elimina un registro que pertenece a otra organización.
- El sistema recibe una petición con parámetros vacíos o nulos que deberían ser rechazados.
- El usuario intenta crear un registro con un campo de organización que no coincide con su Scope.

## Requirements

Implementar un middleware de autenticación que calcule y adjunte el Scope al contexto de la solicitud.
Aplicar filtros automáticos en todas las consultas SELECT según el Scope.
Validar el Scope antes de ejecutar cualquier operación DML.
Configurar y mantener políticas RLS en Supabase que reflejen los Scopes definidos.
Registrar todas las acciones en logs con información de Scope y usuario.
Probar exhaustivamente con diferentes roles y organizaciones para asegurar aislamiento.
Documentar el flujo de autorización y los puntos de entrada del backend.

## Success Criteria

- Todas las peticiones que intentan violar el Scope reciben respuestas 403 o 401.
- Los logs muestran la trazabilidad completa de cada operación con su Scope.
- Las pruebas de penetración no identifican brechas de acceso a datos fuera del Scope.
- El sistema mantiene un rendimiento aceptable con filtros RLS aplicados.
- Los frontends no pueden alterar el Scope ni acceder a recursos no autorizados.



