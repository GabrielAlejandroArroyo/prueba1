# 006 - Definición de Maestros

> **Input**: User description
> **Date**: 2026-09-04
> **Status**: Specify

## User Stories

### US01: Crear un nuevo Maestro (Priority: P1)

Como administrador, quiero crear un nuevo Maestro para que pueda registrar entidades de negocio configurables sin desarrollar una nueva pantalla ni tabla.

#### Acceptance Criteria

- [ ] El usuario debe poder ingresar código, nombre, descripción y estado.
- [ ] El Maestro se guarda en la tabla `master_definitions` sin crear una nueva tabla.
- [ ] El usuario recibe un mensaje de éxito y el Maestro aparece en la lista.

### US02: Editar un Maestro existente (Priority: P1)

Como administrador, quiero editar un Maestro existente para actualizar sus atributos y relaciones.

#### Acceptance Criteria

- [ ] El usuario puede cambiar cualquier campo editable.
- [ ] Las actualizaciones se guardan en la misma tabla `master_definitions`.
- [ ] El historial de cambios se registra en la tabla `master_definitions_audit`.

### US03: Consultar Maestros (Priority: P2)

Como usuario autorizado, quiero consultar la lista de Maestros para ver sus detalles y estado.

#### Acceptance Criteria

- [ ] El usuario puede filtrar por código, nombre y estado.
- [ ] Los resultados se muestran en una tabla con paginación.
- [ ] Se muestra la información de auditoría básica (creado, actualizado).

### US04: Desactivar un Maestro (Priority: P1)

Como administrador, quiero desactivar un Maestro para que ya no esté disponible para selección en otras entidades.

#### Acceptance Criteria

- [ ] El Maestro pasa al estado `inactivo`.
- [ ] Las entidades que lo referencian siguen funcionando, pero no se permite crear nuevas relaciones con él.
- [ ] El usuario recibe una confirmación de desactivación.

### US05: Configurar campos descriptivos y visibilidad (Priority: P2)

Como administrador, quiero configurar qué campo se muestra como descriptivo y la visibilidad de los campos del Maestro.

#### Acceptance Criteria

- [ ] El usuario selecciona el campo que será `display_field`.
- [ ] Los campos marcados como no visibles no aparecen en las vistas de lista.
- [ ] La configuración se guarda en la tabla `master_fields`.

### US06: Configurar permisos y relaciones (Priority: P3)

Como administrador, quiero asignar permisos y relaciones a un Maestro para controlar su acceso y vinculación con otras entidades.

#### Acceptance Criteria

- [ ] Se puede asignar un conjunto de roles que pueden crear/editar/consultar el Maestro.
- [ ] Se pueden definir relaciones (1:N, N:M) con otros Maestros.
- [ ] Las configuraciones se almacenan en `master_permissions` y `master_relations`.

## Edge Cases

- Intentar crear un Maestro con un código duplicado debe generar un error de validación.
- Desactivar un Maestro que ya está relacionado con registros activos debe requerir confirmación adicional.
- Eliminar un Maestro que tiene relaciones definidas debe impedir la operación y mostrar dependencias.
- Cambiar el `display_field` a un campo que no existe debe generar un error.
- Aplicar permisos a un Maestro sin roles asignados debe dejarlo accesible a todos.

## Requirements

El sistema debe permitir CRUD completo de Maestros sin generar nuevas tablas ni pantallas.
El modelo debe ser metadata-driven, con tablas genéricas para definiciones, campos, relaciones y permisos.
Las operaciones de creación y edición deben validar unicidad de código y existencia de organización.
El sistema debe registrar auditoría de cambios en `master_definitions_audit`.
Se debe implementar paginación y filtrado en la consulta de Maestros.
Los permisos de acceso deben respetar la jerarquía de roles y organización.

## Success Criteria

- Los Maestros se crean, actualizan y consultan sin necesidad de nuevas pantallas ni tablas.
- La configuración de campos, visibilidad, permisos y relaciones se refleja correctamente en la UI.
- Los usuarios autorizados pueden gestionar Maestros de manera segura y auditada.
- El rendimiento de las operaciones CRUD es comparable al de tablas existentes.
- El sistema cumple con los requisitos de seguridad y cumplimiento de auditoría.



