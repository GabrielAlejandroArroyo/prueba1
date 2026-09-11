# 002 - Gestión de Usuarios y Organizaciones

> **Input**: User description
> **Date**: 2026-09-04
> **Status**: Specify

## User Stories

### US01: Crear organización (Priority: P1)

Como administrador, quiero crear una nueva organización, para poder agrupar usuarios bajo una entidad común.

#### Acceptance Criteria

- [ ] El administrador puede llenar un formulario con los campos id, code, name, description, status.
- [ ] El sistema valida que el code sea único.
- [ ] Al guardar, la organización aparece en la lista de organizaciones con estado activo.

### US02: Editar organización (Priority: P1)

Como administrador, quiero editar los detalles de una organización, para mantener la información actualizada.

#### Acceptance Criteria

- [ ] El administrador puede modificar name, description y status.
- [ ] Los cambios se reflejan inmediatamente en la lista de organizaciones.
- [ ] El campo code no se puede modificar.

### US03: Activar y desactivar organización (Priority: P2)

Como administrador, quiero activar o desactivar una organización, para controlar su disponibilidad.

#### Acceptance Criteria

- [ ] El administrador puede cambiar el status entre activo e inactivo.
- [ ] Una organización inactiva no permite crear nuevos perfiles asociados.
- [ ] Los perfiles existentes permanecen pero no pueden iniciar sesión.

### US04: Asociar usuarios con organizaciones (Priority: P1)

Como administrador, quiero asociar un usuario existente con una organización, para asignarle contexto de negocio.

#### Acceptance Criteria

- [ ] El administrador puede seleccionar un usuario por email y asignarle una organización.
- [ ] El sistema crea o actualiza el perfil con organization_id.
- [ ] El usuario recibe notificación de la asociación.

### US05: Ver roles y capabilities del usuario (Priority: P2)

Como administrador, quiero ver los roles y capabilities de un usuario, para auditoría y control de acceso.

#### Acceptance Criteria

- [ ] El administrador puede abrir una vista con los roles asignados al usuario.
- [ ] El administrador puede ver las capabilities derivadas de esos roles.
- [ ] La vista muestra los estados activos/inactivos.

### US06: Restricción de acceso por organización (Priority: P1)

Como administrador, quiero que solo pueda gestionar usuarios de mi organización, para mantener la seguridad.

#### Acceptance Criteria

- [ ] El sistema verifica el organization_id del perfil del administrador.
- [ ] El administrador no puede acceder a usuarios de otras organizaciones.
- [ ] Intentos de acceso fuera de la organización devuelven error 403.

### US07: Super Admin gestionando todas las organizaciones (Priority: P1)

Como Super Admin, quiero gestionar todas las organizaciones, sin restricciones de organización.

#### Acceptance Criteria

- [ ] El Super Admin puede crear, editar y borrar cualquier organización.
- [ ] El Super Admin puede asociar usuarios a cualquier organización.
- [ ] El sistema permite acceder a la lista completa de organizaciones.

## Edge Cases

- Intento de crear una organización con un código duplicado.
- Asociar un usuario a una organización que no existe.
- Administrador intentando gestionar usuarios de otra organización.
- Desactivar una organización con usuarios activos.
- Crear perfil sin email válido.

## Requirements

CRUD completo de organizaciones.
CRUD completo de perfiles de usuario.
Validación de unicidad de code en organizaciones.
Restricción de acceso basada en organización para admins.
Permitir a Super Admin acceso global.
Gestión de status activo/inactivo para organizaciones y perfiles.
Asociación de usuarios a organizaciones.
Visualización de roles y capabilities del usuario.

## Success Criteria

- Todas las organizaciones pueden ser creadas, editadas y desactivadas según los roles.
- Los perfiles de usuario se crean y actualizan correctamente con organization_id.
- Los administradores sólo acceden a usuarios de su organización.
- El Super Admin puede gestionar todas las organizaciones sin restricciones.
- La aplicación devuelve errores claros y seguros en casos de borde.



