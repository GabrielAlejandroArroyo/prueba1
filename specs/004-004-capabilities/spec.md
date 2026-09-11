# 004 - Capabilities

> **Input**: User description
> **Date**: 2026-09-04
> **Status**: Specify

## User Stories

### US01: Creación y edición de capacidades (Priority: P1)

Como administrador del sistema, quiero crear y editar capacidades, para que el sistema refleje las acciones disponibles para los usuarios y roles.

#### Acceptance Criteria

- [ ] El administrador puede crear una nueva capacidad con nombre y descripción.
- [ ] El administrador puede editar el nombre y la descripción de una capacidad existente.
- [ ] El sistema valida que el nombre de la capacidad sea único.
- [ ] El administrador puede desactivar una capacidad, impidiendo su asignación futura.

### US02: Asignación de capacidades a roles (Priority: P1)

Como administrador de roles, quiero asignar capacidades a roles, para que los usuarios inherentes a esos roles obtengan los permisos necesarios.

#### Acceptance Criteria

- [ ] El administrador puede seleccionar una capacidad y asignarla a un rol.
- [ ] La asignación se refleja en la lista de capacidades del rol.
- [ ] El sistema evita duplicados al asignar la misma capacidad al mismo rol.
- [ ] El administrador puede remover una capacidad de un rol.

### US03: Asignación directa de capacidades a usuarios (Priority: P1)

Como administrador, quiero asignar capacidades directamente a usuarios, para otorgar permisos específicos sin modificar su rol.

#### Acceptance Criteria

- [ ] El administrador puede seleccionar una capacidad y asignarla a un usuario.
- [ ] El usuario ve la capacidad asignada en su lista de permisos.
- [ ] El administrador puede remover una capacidad asignada directamente a un usuario.
- [ ] El sistema no permite asignar una capacidad que ya esté activada a través de un rol del usuario.

### US04: Visualización de capacidades efectivas (Priority: P2)

Como usuario, quiero ver las capacidades que tengo efectivamente, tanto a través de roles como asignaciones directas, para comprender mis privilegios.

#### Acceptance Criteria

- [ ] El usuario puede acceder a una vista que lista todas las capacidades activas.
- [ ] La lista incluye capacidades heredadas de roles y asignaciones directas.
- [ ] El sistema muestra el origen de cada capacidad (rol o asignación directa).

### US05: Restricción de scope por compañía (Priority: P1)

Como sistema, quiero asegurar que una capacidad solo permita acciones dentro del scope de la compañía del usuario, evitando privilegios cruzados.

#### Acceptance Criteria

- [ ] Al intentar ejecutar una acción con una capacidad, el sistema verifica que la entidad objetivo pertenezca a la misma compañía del usuario.
- [ ] Si el scope no coincide, la acción se rechaza con un mensaje de error claro.
- [ ] El registro de auditoría incluye el intento fallido y la razón.

## Edge Cases

- Asignar una capacidad a un usuario que ya la posee a través de un rol.
- Remover una capacidad de un rol cuando usuarios están activos y dependen de esa capacidad.
- Crear una capacidad con nombre que ya existe en otro idioma o con caracteres especiales.
- Asignar una capacidad a un usuario de otra compañía y luego intentar usarla en la compañía original.
- Desactivar una capacidad que ya está asignada a varios roles y usuarios.

## Requirements

El sistema debe permitir CRUD completo de la entidad Capability.
El sistema debe soportar asignación de capacidades a roles y a usuarios.
El sistema debe calcular y mostrar las capacidades efectivas de un usuario.
El sistema debe validar unicidad de nombres de capacidades.
El sistema debe aplicar restricciones de scope basadas en la compañía del usuario.
El sistema debe registrar en auditoría cada asignación, eliminación y uso de capacidades.
El sistema debe ofrecer API REST para gestionar capacidades, asignaciones y consultas de permisos.

## Success Criteria

- Todas las capacidades creadas se reflejan correctamente en la UI y en la API.
- Los usuarios solo pueden ejecutar acciones dentro de su compañía cuando poseen la capacidad correspondiente.
- No existen duplicados de asignaciones de capacidad a roles o usuarios.
- Los logs de auditoría contienen información completa sobre asignaciones y usos.
- La interfaz de usuario muestra claramente el origen de cada capacidad (rol o directa).



