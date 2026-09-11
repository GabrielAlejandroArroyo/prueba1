# Tasks: 006 - Definición de Maestros

**Input**: Design documents from spec and plan
**Prerequisites**: plan.md (required), spec.md (required for user stories)

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2)
- Include exact file paths in descriptions

---

## Phase 1: Setup

**Purpose**: Preparar el entorno de desarrollo y la configuración inicial del proyecto

> **Checkpoint**: Repositorio configurado, dependencias instaladas y conexión a la base de datos establecida

- [ ] T001 [US01] Crear rama de feature y clonar el repositorio
- [ ] T002 [P] [US01] Instalar dependencias de TypeScript, Express, TypeORM, class-validator y otros paquetes
- [ ] T003 [P] [US01] Configurar variables de entorno (.env) y archivo ormconfig.ts
- [ ] T004 [P] [US01] Configurar scripts de npm para compilación, pruebas y ejecución

---

## Phase 2: Database Design

**Purpose**: Definir y crear las tablas genéricas necesarias para la gestión de Maestros

> **Checkpoint**: Esquema de base de datos creado y migraciones ejecutadas con éxito

- [ ] T005 [US01] Diseñar entidad master_definitions con campos id, code, name, description, status, display_field, organization_id, timestamps
- [ ] T006 [P] [US05] Crear tabla master_fields para configurar visibilidad y tipo de cada campo de un Maestro
- [ ] T007 [P] [US06] Crear tabla master_permissions para almacenar roles y permisos por Maestro
- [ ] T008 [P] [US06] Crear tabla master_relations para definir relaciones 1:N y N:M entre Maestros
- [ ] T009 [P] [US02] Crear tabla master_definitions_audit para auditoría de cambios
- [ ] T010 [US01] Generar y ejecutar migraciones de TypeORM

---

## Phase 3: Domain Layer

**Purpose**: Implementar entidades y lógica de negocio

> **Checkpoint**: Entidades y servicios de dominio implementados y testeados unitariamente

- [ ] T011 [US01] Crear entidad MasterDefinition con validaciones de unicidad de code y existencia de organization_id
- [ ] T012 [US02] Implementar lógica de negocio para crear, actualizar, consultar y desactivar Maestros
- [ ] T013 [US05] Implementar lógica de negocio para configurar display_field y visibilidad de campos
- [ ] T014 [US06] Implementar lógica de negocio para asignar permisos y relaciones entre Maestros

---

## Phase 4: Infrastructure Layer

**Purpose**: Conectar el dominio con la base de datos y crear repositorios

> **Checkpoint**: Repositorios funcionales y pruebas de integración pasadas

- [ ] T015 [US01] Crear repositorio MasterDefinitionRepository con métodos CRUD y auditoría
- [ ] T016 [P] [US05] Crear repositorios para master_fields, master_permissions y master_relations
- [ ] T017 [P] [US02] Implementar lógica de auditoría que inserta registros en master_definitions_audit

---

## Phase 5: API Layer

**Purpose**: Exponer endpoints RESTful para la gestión de Maestros

> **Checkpoint**: Endpoints funcionando y documentados

- [ ] T018 [US01] Crear ruta POST /masters para crear un nuevo Maestro
- [ ] T019 [US02] Crear ruta PUT /masters/:id para editar un Maestro existente
- [ ] T020 [US03] Crear ruta GET /masters para listar y filtrar Maestros con paginación
- [ ] T021 [US04] Crear ruta DELETE /masters/:id para desactivar un Maestro
- [ ] T022 [P] [US05] Crear ruta PATCH /masters/:id/display-field para configurar el campo descriptivo
- [ ] T023 [P] [US06] Crear ruta POST /masters/:id/permissions y /masters/:id/relations para configurar permisos y relaciones

---

## Phase 6: Validation & Business Rules

**Purpose**: Garantizar integridad y cumplimiento de reglas de negocio

> **Checkpoint**: Validaciones y reglas aplicadas correctamente en todos los endpoints

- [ ] T024 [US01] Validar unicidad de código en creación y edición
- [ ] T025 [US01] Validar existencia de organization_id en todas las operaciones
- [ ] T026 [US05] Validar que display_field corresponda a un campo existente en master_fields
- [ ] T027 [US06] Validar que un Maestro con relaciones activas no pueda ser eliminado
- [ ] T028 [US04] Implementar confirmación adicional al desactivar Maestro con registros activos

---

## Phase 7: Audit & Logging

**Purpose**: Registrar historial de cambios y operaciones críticas

> **Checkpoint**: Auditoría completa y accesible a través de endpoints

- [ ] T029 [US02] Insertar registro en master_definitions_audit tras cada actualización
- [ ] T030 [P] [US03] Exponer endpoint GET /masters/:id/audit para consultar historial

---

## Phase 8: Pagination & Filtering

**Purpose**: Optimizar la consulta de Maestros con paginación y filtros

> **Checkpoint**: Resultados filtrados y paginados correctamente

- [ ] T031 [US03] Implementar lógica de paginación (limit, offset) en el servicio de listado
- [ ] T032 [US03] Permitir filtros por código, nombre y estado en la consulta
- [ ] T033 [P] [US03] Incluir campos de auditoría (created_at, updated_at) en la respuesta

---

## Phase 9: Permissions & Role Hierarchy

**Purpose**: Controlar acceso basado en roles y jerarquías de organización

> **Checkpoint**: Solo usuarios autorizados pueden crear, editar o consultar Maestros

- [ ] T034 [US06] Implementar middleware de autenticación y autorización
- [ ] T035 [P] [US06] Verificar roles asignados en master_permissions antes de permitir operaciones
- [ ] T036 [P] [US06] Aplicar regla de que sin roles asignados el Maestro es accesible a todos

---

## Phase 10: Testing

**Purpose**: Asegurar calidad mediante pruebas unitarias y de integración

> **Checkpoint**: Todos los tests pasan con cobertura mínima del 80%

- [ ] T037 [US01] Escribir pruebas unitarias para entidades y servicios de dominio
- [ ] T038 [US01] Crear pruebas de integración para cada endpoint REST
- [ ] T039 [P] [US01] Testear casos de borde: código duplicado, desactivación con registros activos, eliminación con relaciones, display_field inválido

---

## Phase 11: Documentation & Deployment

**Purpose**: Documentar API y desplegar en entorno de pruebas

> **Checkpoint**: API documentada con OpenAPI y aplicación desplegada con éxito

- [ ] T040 [US01] Generar documentación OpenAPI para todos los endpoints
- [ ] T041 [P] [US01] Crear scripts de despliegue y pipeline CI/CD

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

