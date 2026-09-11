# Tasks: 003 - Roles y Permisos RBAC

**Input**: Design documents from spec and plan
**Prerequisites**: plan.md (required), spec.md (required for user stories)

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2)
- Include exact file paths in descriptions

---

## Phase 1: Setup

**Purpose**: Establecer el entorno de desarrollo y las dependencias necesarias.

> **Checkpoint**: Todos los paquetes instalados y la base de datos inicializada.

- [ ] T001 Inicializar proyecto con TypeScript, Express y TypeORM.
- [ ] T002 Configurar conexión PostgreSQL y crear archivo .env.
- [ ] T003 Instalar y configurar Vitest y Playwright para pruebas.
- [ ] T004 Crear estructura de carpetas según el plan de proyecto.

---

## Phase 2: Database Schema

**Purpose**: Definir tablas y políticas RLS en PostgreSQL.

> **Checkpoint**: Todas las tablas y políticas RLS creadas y migraciones ejecutadas.

- [ ] T005 [US01] Crear tabla `roles` con campos id, name, description, active, created_at.
- [ ] T006 [US05] Crear tabla `permissions` con campos id, name, description.
- [ ] T007 [US06] Crear tabla `role_permissions` (role_id, permission_id) con claves foráneas.
- [ ] T008 [US07] Crear tabla `user_roles` (user_id, role_id) con claves foráneas.
- [ ] T009 Agregar campos auditados a `roles` y `permissions` (created_by, updated_by, etc.).
- [ ] T010 [US10] Definir políticas RLS para la tabla `sensitive_data`.

---

## Phase 3: API Endpoints

**Purpose**: Implementar rutas y controladores REST para roles y permisos.

> **Checkpoint**: Todas las rutas básicas están disponibles y responden correctamente.

- [ ] T011 [US01] Crear ruta POST `/roles` y controlador para US01.
- [ ] T012 [US02] Crear ruta PUT `/roles/:id` y controlador para US02.
- [ ] T013 [US03] Crear ruta DELETE `/roles/:id` que marca como inactivo para US03.
- [ ] T014 [US04] Crear ruta GET `/roles` con filtrado y ordenación para US04.
- [ ] T015 [US05] Crear ruta POST `/permissions` y controlador para US05.
- [ ] T016 [US06] Crear ruta POST `/roles/:id/permissions` para asignar permisos (US06).
- [ ] T017 [US07] Crear ruta POST `/users/:id/roles` para asignar roles a usuarios (US07).
- [ ] T018 [US08] Crear ruta DELETE `/users/:userId/roles/:roleId` para remover roles (US08).

---

## Phase 4: Business Logic & Validation

**Purpose**: Implementar servicios, validaciones y reglas de negocio.

> **Checkpoint**: Todas las reglas de negocio validan correctamente los datos.

- [ ] T019 [US01] Implementar servicio de creación de rol con validación de unicidad de nombre.
- [ ] T020 [US02] Implementar servicio de actualización de rol con validación de unicidad.
- [ ] T021 [US06] Implementar servicio de asignación de permisos a roles, evitando duplicados.
- [ ] T022 [US07] Implementar servicio de asignación de roles a usuarios, filtrando roles inactivos.
- [ ] T023 [US08] Implementar servicio de remoción de roles de usuarios con control de integridad.

---

## Phase 5: Authorization Middleware

**Purpose**: Proteger rutas y verificar permisos en el servidor.

> **Checkpoint**: Todas las rutas protegidas devuelven 403 cuando el token o roles son inválidos.

- [ ] T024 Crear middleware de verificación de JWT.
- [ ] T025 [US09] Crear middleware de autorización basado en roles y permisos para US09.

---

## Phase 6: RLS Implementation

**Purpose**: Aplicar y probar políticas Row-Level Security.

> **Checkpoint**: Políticas RLS aplicadas y verificadas en pruebas de auditoría.

- [ ] T026 [US10] Escribir scripts SQL para crear políticas RLS en tablas críticas.
- [ ] T027 [US10] Desarrollar pruebas de integración que verifiquen la restricción de datos según RLS.

---

## Phase 7: Auditing

**Purpose**: Registrar cambios en roles y permisos.

> **Checkpoint**: Registros de auditoría creados y accesibles.

- [ ] T028 Implementar trigger de auditoría en `roles` y `permissions`.
- [ ] T029 Crear endpoint para consultar logs de auditoría (opcional).

---

## Phase 8: Testing

**Purpose**: Garantizar la calidad con pruebas unitarias e integración.

> **Checkpoint**: Todas las pruebas pasan y cubren los casos de borde.

- [ ] T030 [P] Escribir pruebas unitarias para servicios de roles y permisos.
- [ ] T031 [P] Escribir pruebas unitarias para middlewares de autorización.
- [ ] T032 Desarrollar pruebas de integración con Playwright para flujos completos.
- [ ] T033 Probar edge cases: crear rol duplicado, asignar rol inactivo, eliminar rol asignado.

---

## Phase 9: Documentation

**Purpose**: Proveer documentación técnica y de API.

> **Checkpoint**: Documentación completa publicada en el repositorio.

- [ ] T034 Crear README con descripción del proyecto y requisitos.
- [ ] T035 Generar documentación Swagger/OpenAPI para los endpoints.

---

## Phase 10: Deployment & Release

**Purpose**: Desplegar la aplicación y versionar la release.

> **Checkpoint**: Aplicación en producción y release etiquetado.

- [ ] T036 Configurar CI/CD para despliegue automático a entorno de staging.
- [ ] T037 Realizar pruebas de regresión en staging.
- [ ] T038 Efectuar despliegue a producción y verificar funcionalidad.

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup**: No dependencies - can start immediately
- **Foundational**: Depends on Setup - BLOCKS all user stories
- **User Stories**: All depend on Foundational
- **Polish**: Depends on all user stories

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story
- Each user story should be independently testable
- Verify tests fail before implementing
- Commit after each task or logical group

