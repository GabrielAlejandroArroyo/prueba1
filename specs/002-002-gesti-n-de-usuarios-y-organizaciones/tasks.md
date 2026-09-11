# Tasks: 002 - Gestión de Usuarios y Organizaciones

**Input**: Design documents from spec and plan
**Prerequisites**: plan.md (required), spec.md (required for user stories)

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2)
- Include exact file paths in descriptions

---

## Phase 1: Setup

**Purpose**: Preparar el entorno de desarrollo y la base de datos inicial.

> **Checkpoint**: Repositorio configurado, dependencias instaladas, migraciones aplicadas.

- [ ] T001 [General] Crear rama de feature y configurar repositorio.
- [ ] T002 [General] Instalar dependencias básicas del framework y el ORM.
- [ ] T003 [General] Configurar entorno de desarrollo con variables de entorno y base de datos.
- [ ] T004 [General] Diseñar esquema inicial de la base de datos (usuarios, organizaciones, roles, capabilities).
- [ ] T005 [General] Crear migraciones y ejecutar la base de datos.

---

## Phase 2: Design

**Purpose**: Especificar la arquitectura, endpoints y reglas de negocio.

> **Checkpoint**: Diseño completo validado y aprobado.

- [ ] T006 [P] [US01, US02, US03] Diseñar endpoints REST para CRUD de organizaciones.
- [ ] T007 [P] [US04] Diseñar endpoints REST para CRUD de perfiles de usuario.
- [ ] T008 [P] [US01] Definir reglas de validación de unicidad del campo code en organizaciones.
- [ ] T009 [P] [US03, US04] Establecer lógica de status activo/inactivo para organizaciones y perfiles.
- [ ] T010 [P] [US04] Diseñar lógica de asociación de usuario a organización.
- [ ] T011 [P] [US05] Diseñar vista de roles y capabilities del usuario.
- [ ] T012 [P] [US06, US07] Crear middleware de autorización por organization_id.
- [ ] T013 [P] [Edge Cases] Documentar casos de borde y flujos de error.

---

## Phase 3: Development

**Purpose**: Implementar la lógica de negocio y los endpoints.

> **Checkpoint**: Todos los endpoints funcionales y probados.

- [ ] T014 [US01] Implementar endpoint POST /organizations para crear organizaciones (US01).
- [ ] T015 [US01, US02, US03, US07] Implementar endpoint GET /organizations para listar organizaciones.
- [ ] T016 [US02] Implementar endpoint PUT /organizations/:id para editar detalles (US02).
- [ ] T017 [US03] Implementar endpoint PATCH /organizations/:id/status para cambiar status (US03).
- [ ] T018 [US03] Bloquear creación de perfiles cuando la organización está inactiva.
- [ ] T019 [US04] Implementar endpoint POST /users/:id/associate para asociar usuario a organización (US04).
- [ ] T020 [US05] Implementar endpoint GET /users/:id/roles para ver roles y capabilities (US05).
- [ ] T021 [US06] Aplicar middleware de restricción de acceso por organización (US06).
- [ ] T022 [US07] Agregar rutas y lógica para Super Admin (US07).
- [ ] T023 [US04] Enviar notificación por email al usuario tras asociación (US04).
- [ ] T024 [Edge Cases] Validar email y código duplicado al crear organizaciones y usuarios.
- [ ] T025 [General] Escribir pruebas unitarias básicas para cada endpoint.
- [ ] T026 [General] Escribir pruebas de integración con base de datos.
- [ ] T027 [Edge Cases] Implementar pruebas para casos de borde (duplicado, inexistente, acceso fuera de organización).

---

## Phase 4: Testing

**Purpose**: Asegurar calidad, cobertura y seguridad de la funcionalidad.

> **Checkpoint**: Todas las pruebas pasan y cobertura > 80%.

- [ ] T028 [General] Ejecutar pruebas unitarias completas.
- [ ] T029 [General] Ejecutar pruebas de integración.
- [ ] T030 [General] Revisar cobertura de pruebas y mejorar donde sea necesario.
- [ ] T031 [General] Revisar seguridad (inyección, autorización, CSRF).
- [ ] T032 [General] Verificar manejo de errores y logs.
- [ ] T033 [General] Evaluar rendimiento de endpoints CRUD bajo carga simulada.

---

## Phase 5: Deployment & Documentation

**Purpose**: Publicar la funcionalidad y documentarla para usuarios y desarrolladores.

> **Checkpoint**: API documentada y desplegada en producción.

- [ ] T034 [General] Documentar API en Swagger/OpenAPI.
- [ ] T035 [General] Documentar reglas de negocio y validaciones.
- [ ] T036 [General] Preparar despliegue en entorno de staging.
- [ ] T037 [General] Realizar despliegue en producción.
- [ ] T038 [General] Monitorear logs y métricas post-despliegue.

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

