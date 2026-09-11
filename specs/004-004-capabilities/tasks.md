# Tasks: 004 - Capabilities

**Input**: Design documents from spec and plan
**Prerequisites**: plan.md (required), spec.md (required for user stories)

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2)
- Include exact file paths in descriptions

---

## Phase 1: Setup

**Purpose**: Preparar el entorno de desarrollo y la infraestructura base.

> **Checkpoint**: El proyecto debe compilar y conectarse a la base de datos.

- [ ] T001 [P] [None] Configurar repositorio git, rama feature y pipeline CI/CD.
- [ ] T002 [P] [None] Instalar dependencias básicas (express, typeorm, pg, class-validator, winston, dotenv).
- [ ] T003 [P] [None] Configurar conexión a PostgreSQL y crear migraciones iniciales.
- [ ] T004 [P] [None] Configurar dotenv y variables de entorno.
- [ ] T005 [P] [None] Configurar Winston logger para auditoría y logs de aplicación.

---

## Phase 2: Domain Modeling

**Purpose**: Definir entidades, DTOs y repositorios de dominio.

> **Checkpoint**: Todas las entidades deben compilar y las migraciones deben reflejarlas en la BD.

- [ ] T006 [US01] Definir entidad Capability con campos id, name, description, active, createdAt, updatedAt.
- [ ] T007 [US01] Crear DTOs CreateCapabilityDto y UpdateCapabilityDto.
- [ ] T008 [US02] Definir entidad Role y relación many-to-many con Capability.
- [ ] T009 [US03] Definir entidad User y relación many-to-many con Capability (direct).
- [ ] T010 [None] Crear interfaces de repositorio para Capability, Role y User.

---

## Phase 3: API Development – Capability CRUD

**Purpose**: Implementar endpoints REST para crear, leer, actualizar y desactivar capacidades.

> **Checkpoint**: Los endpoints deben cumplir los criterios de aceptación de US01.

- [ ] T011 [US01] Crear endpoint POST /capabilities (crear capacidad).
- [ ] T012 [US01] Crear endpoint GET /capabilities (listar capacidades).
- [ ] T013 [US01] Crear endpoint GET /capabilities/:id (detalle).
- [ ] T014 [US01] Crear endpoint PUT /capabilities/:id (actualizar).
- [ ] T015 [US01] Crear endpoint DELETE /capabilities/:id (soft delete/desactivar).

---

## Phase 4: API Development – Assignments

**Purpose**: Implementar asignación y remoción de capacidades a roles y usuarios.

> **Checkpoint**: Los endpoints deben cumplir los criterios de aceptación de US02 y US03.

- [ ] T016 [US02] Endpoint POST /roles/:roleId/capabilities/:capabilityId (asignar a rol).
- [ ] T017 [US02] Endpoint DELETE /roles/:roleId/capabilities/:capabilityId (remover de rol).
- [ ] T018 [US03] Endpoint POST /users/:userId/capabilities/:capabilityId (asignar directa).
- [ ] T019 [US03] Endpoint DELETE /users/:userId/capabilities/:capabilityId (remover asignación directa).

---

## Phase 5: Permission Calculation Service

**Purpose**: Calcular capacidades efectivas y exponerlas a la UI.

> **Checkpoint**: El endpoint debe devolver la lista completa con origen (rol/directo).

- [ ] T020 [US04] Crear servicio que calcule capacidades efectivas de un usuario.
- [ ] T021 [US04] Endpoint GET /users/:userId/permissions (lista de capacidades efectivas).

---

## Phase 6: Validation & Uniqueness

**Purpose**: Garantizar reglas de negocio y evitar duplicados.

> **Checkpoint**: Todas las validaciones deben ser unitariamente testeadas.

- [ ] T022 [US01] Validar unicidad del nombre de Capability (class-validator).
- [ ] T023 [US02] Validar que no se asigne la misma capacidad a un rol más de una vez.
- [ ] T024 [US03] Validar que no se asigne la misma capacidad a un usuario más de una vez.
- [ ] T025 [US03] Validar que no se asigne una capacidad ya activa a través de un rol al usuario.

---

## Phase 7: Scope & Company Restrictions

**Purpose**: Restringir acciones a la compañía del usuario.

> **Checkpoint**: El middleware debe rechazar acciones fuera de scope y registrar el intento.

- [ ] T026 [US05] Middleware que verifica scope de compañía al ejecutar acción con capacidad.
- [ ] T027 [US05] Implementar lógica de rechazo con mensaje de error y auditoría.

---

## Phase 8: Auditing

**Purpose**: Registrar todas las asignaciones, eliminaciones y usos de capacidades.

> **Checkpoint**: Los logs de auditoría deben contener información completa y estar disponibles para consultas.

- [ ] T028 [None] Configurar auditoría para asignaciones y eliminaciones de capacidades.
- [ ] T029 [US05] Registrar intentos fallidos de acciones con mismatch de scope.

---

## Phase 9: Edge Cases Handling

**Purpose**: Probar y garantizar el comportamiento correcto en situaciones límite.

> **Checkpoint**: Todos los edge cases deben pasar los tests unitarios e integrales.

- [ ] T030 [P] [None] Test: asignar capacidad a usuario que ya la posee vía rol (US03).
- [ ] T031 [P] [None] Test: remover capacidad de rol cuando usuarios dependen de ella (US02).
- [ ] T032 [P] [None] Test: crear capacidad con nombre existente en otro idioma o caracteres especiales (US01).
- [ ] T033 [P] [None] Test: asignar capacidad a usuario de otra compañía y usarla en la compañía original (US03/US05).
- [ ] T034 [P] [None] Test: desactivar capacidad asignada a varios roles y usuarios (US01).

---

## Phase 10: Testing

**Purpose**: Asegurar cobertura completa con tests unitarios, integrales y e2e.

> **Checkpoint**: Todos los tests deben pasar y cobertura debe superar el 90%.

- [ ] T035 [P] [None] Escribir unit tests para dominio y servicios (Vitest).
- [ ] T036 [P] [None] Escribir tests de integración de API (Vitest + supertest).
- [ ] T037 [P] [None] Escribir tests end-to-end con Playwright.

---

## Phase 11: UI Integration

**Purpose**: Implementar componentes React para la gestión de capacidades.

> **Checkpoint**: La UI debe reflejar correctamente los datos y permitir todas las operaciones.

- [ ] T038 [US01] Crear componentes React para listar, crear y editar capacidades (US01).
- [ ] T039 [US02, US03] Crear componentes React para asignar capacidades a roles y usuarios (US02, US03).
- [ ] T040 [US04] Crear vista de capacidades efectivas con origen (rol/directo) (US04).

---

## Phase 12: Documentation & Release

**Purpose**: Documentar API y desplegar la versión final.

> **Checkpoint**: Swagger debe estar disponible y el release tag debe estar creado.

- [ ] T041 [None] Documentar endpoints y modelos en Swagger.
- [ ] T042 [None] Actualizar README con instrucciones de despliegue.
- [ ] T043 [None] Merge rama feature y crear release tag.

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

