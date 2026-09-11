# 003 - Roles y Permisos RBAC

> **Input**: User description
> **Date**: 2026-09-04
> **Status**: Specify

## User Stories

### US01: Crear un nuevo rol (Priority: P1)

Como administrador, quiero crear un nuevo rol, para poder asignar permisos específicos a grupos de usuarios.

#### Acceptance Criteria

- [ ] El sistema muestra un formulario de creación de rol con campos obligatorios: nombre y descripción.
- [ ] El nombre del rol es único y se valida antes de la creación.
- [ ] Al crear el rol, se guarda en la tabla `roles` y el usuario recibe una confirmación.

### US02: Modificar un rol existente (Priority: P1)

Como administrador, quiero modificar los atributos de un rol, para actualizar su descripción o nombre cuando sea necesario.

#### Acceptance Criteria

- [ ] El sistema muestra un formulario con los datos actuales del rol.
- [ ] Los cambios se guardan en la tabla `roles` y se confirma la actualización.
- [ ] El nombre sigue siendo único después de la modificación.

### US03: Desactivar un rol (Priority: P1)

Como administrador, quiero desactivar un rol, para impedir que nuevos usuarios lo reciban sin eliminarlo.

#### Acceptance Criteria

- [ ] El rol se marca como inactivo en la tabla `roles`.
- [ ] Los usuarios que ya tienen el rol siguen manteniéndolo, pero no pueden asignarlo a otros usuarios.
- [ ] El sistema muestra el estado activo/inactivo en la lista de roles.

### US04: Consultar roles (Priority: P1)

Como usuario con permiso adecuado, quiero consultar la lista de roles, para revisar quién tiene qué acceso.

#### Acceptance Criteria

- [ ] El usuario ve una tabla con los campos: nombre, descripción, estado activo, fecha de creación.
- [ ] La tabla permite filtrar por estado y ordenar por nombre.

### US05: Crear un nuevo permiso (Priority: P1)

Como administrador, quiero crear un permiso, para poder asignarlo a roles.

#### Acceptance Criteria

- [ ] El permiso tiene un identificador único y una descripción.
- [ ] Se guarda en la tabla `permissions` y el usuario recibe una confirmación.

### US06: Asociar permisos con roles (Priority: P1)

Como administrador, quiero asignar permisos a un rol, para definir qué acciones puede realizar.

#### Acceptance Criteria

- [ ] El sistema permite seleccionar múltiples permisos para un rol.
- [ ] Las asociaciones se guardan en la tabla `role_permissions`.
- [ ] Se evita la duplicación de asociaciones.

### US07: Asignar roles a usuarios (Priority: P1)

Como administrador, quiero asignar uno o varios roles a un usuario, para otorgarle los permisos correspondientes.

#### Acceptance Criteria

- [ ] El usuario puede seleccionar roles de la lista activa.
- [ ] Las asignaciones se guardan en la tabla `user_roles`.
- [ ] El sistema actualiza la sesión del usuario para reflejar los nuevos roles.

### US08: Remover roles de usuarios (Priority: P1)

Como administrador, quiero remover roles de un usuario, para revocar sus permisos.

#### Acceptance Criteria

- [ ] El usuario puede deseleccionar roles asignados.
- [ ] Las eliminaciones se reflejan en la tabla `user_roles`.
- [ ] Se mantiene la integridad referencial y no se elimina el rol en sí.

### US09: Verificar autorización en servidor (Priority: P2)

Como desarrollador, quiero que la verificación de permisos se realice en el servidor, para asegurar que la lógica no pueda ser eludida.

#### Acceptance Criteria

- [ ] Todas las rutas protegidas comprueban el token y los roles/permiso antes de procesar la solicitud.
- [ ] Los intentos de acceso sin autorización devuelven un error 403.

### US10: Aplicar políticas RLS a operaciones críticas (Priority: P2)

Como administrador, quiero que las consultas sensibles se restrinjan mediante RLS, para garantizar la seguridad de los datos.

#### Acceptance Criteria

- [ ] Las políticas RLS se aplican a las tablas relevantes.
- [ ] Los usuarios solo ven los registros que cumplen la política.
- [ ] Las pruebas de auditoría confirman que la RLS funciona correctamente.

## Edge Cases

- Intentar crear un rol con un nombre que ya existe.
- Asignar un rol inactivo a un usuario.
- Eliminar un rol que todavía está asignado a usuarios.
- Asignar un permiso que no existe a un rol.
- Remover un rol que es el único asignado a un usuario.
- Cambiar el estado de un rol mientras hay procesos en curso que lo usan.

## Requirements

Crear, leer, actualizar y desactivar roles.
Crear y consultar permisos.
Asociar y desasociar permisos con roles.
Asignar y remover roles a usuarios.
Validar unicidad de nombres de roles y permisos.
Mantener estado activo/inactivo en roles.
Proteger endpoints con verificación de autorización en servidor.
Aplicar políticas RLS en tablas críticas.
Registrar auditoría de cambios en roles y permisos.

## Success Criteria

- Todos los endpoints de roles y permisos pasan pruebas unitarias y de integración.
- Los usuarios con roles adecuados pueden realizar sus acciones y los sin autorización reciben 403.
- Los registros en las tablas `roles`, `permissions`, `role_permissions` y `user_roles` reflejan los cambios correctamente.
- La RLS restringe el acceso a datos sensibles según la política definida.
- No se permite la creación de roles o permisos duplicados.
- El sistema no permite asignar roles inactivos a usuarios.



