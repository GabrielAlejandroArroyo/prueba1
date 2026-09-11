# Tasks: 005 - Visibilidad, Scope y Row Level Security

**Input**: Design documents from spec and plan
**Prerequisites**: plan.md (required), spec.md (required for user stories)

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2)
- Include exact file paths in descriptions

---

## Fase 1: Configuración inicial

**Purpose**: Preparar el entorno de desarrollo, repositorio y dependencias básicas.

> **Checkpoint**: El proyecto compila y se puede iniciar el servidor local.

- [ ] T001 Crear rama de feature y configurar .gitignore.
- [ ] T002 Instalar dependencias: express, @supabase/supabase-js, jsonwebtoken, dotenv, winston, jest, supertest.
- [ ] T003 Configurar variables de entorno en .env (SUPABASE_URL, SUPABASE_KEY, JWT_SECRET).
- [ ] T004 Crear estructura de carpetas según plan de proyecto.

---

## Fase 2: Middleware de autenticación y cálculo de Scope

**Purpose**: Implementar la lógica de autenticación y cálculo del Scope efectivo.

> **Checkpoint**: El middleware valida JWT, calcula Scope y lo adjunta al contexto de la solicitud.

- [ ] T005 [US01] Crear middleware auth.ts que decodifique el JWT y verifique su firma.
- [ ] T006 [US01] Implementar lógica de cálculo de Scope según rol y organización del usuario.
- [ ] T007 [US01] Adjuntar el Scope calculado al objeto request (req.scope).
- [ ] T008 Agregar manejo de errores: respuestas 401 cuando el token es inválido o expirado.

---

## Fase 3: Filtros automáticos en consultas SELECT

**Purpose**: Asegurar que todas las lecturas respeten el Scope del usuario.

> **Checkpoint**: Todas las consultas SELECT incluyen filtros basados en Scope y devuelven solo datos permitidos.

- [ ] T009 [US02] Crear helper db.selectWithScope(table, columns, req) que añada filtros automáticos.
- [ ] T010 [US02] Reescribir controladores de lectura para usar selectWithScope.
- [ ] T011 [US02] Agregar pruebas unitarias que verifiquen que los SELECT no devuelven datos fuera del Scope.

---

## Fase 4: Validación de Scope en operaciones DML

**Purpose**: Garantizar que INSERT, UPDATE y DELETE solo actúen sobre registros dentro del Scope.

> **Checkpoint**: Operaciones DML devuelven 403 cuando intentan modificar registros fuera del Scope.

- [ ] T012 [US03] Crear helper db.writeWithScope(operation, table, data, req) que valide Scope antes de ejecutar.
- [ ] T013 [US03] Actualizar controladores de escritura para usar writeWithScope.
- [ ] T014 [US03] Registrar cada operación con Scope y usuario en logs.
- [ ] T015 [US03] Agregar pruebas que intenten modificar registros fuera del Scope y verifiquen error 403.

---

## Fase 5: Políticas Row Level Security (RLS) en Supabase

**Purpose**: Reforzar la seguridad a nivel de base de datos.

> **Checkpoint**: Todas las tablas relevantes tienen RLS habilitado y políticas que reflejan los Scopes.

- [ ] T016 [US05] Habilitar RLS en todas las tablas críticas.
- [ ] T017 [US05] Crear políticas RLS que imiten la lógica de Scope del backend.
- [ ] T018 [US05] Ejecutar pruebas de seguridad que confirmen que RLS bloquea accesos indebidos.

---

## Fase 6: Manejo de Edge Cases y validaciones adicionales

**Purpose**: Detectar y bloquear intentos de manipulación directa de API y parámetros.

> **Checkpoint**: Todas las edge cases generan respuestas 401/403 y se registran en logs.

- [ ] T019 [US07] Validar que el identificador de recurso pertenezca al Scope antes de procesar la solicitud.
- [ ] T020 Detectar y bloquear solicitudes con parámetros vacíos o nulos.
- [ ] T021 Registrar origen y motivo de bloqueos en logs.
- [ ] T022 [US07] Agregar pruebas que modifiquen la URL o ID para intentar acceder a otro Scope.
- [ ] T023 Simular manipulación del token JWT para incluir un Scope más amplio y verificar bloqueo.

---

## Fase 7: Pruebas de integración y rendimiento

**Purpose**: Asegurar aislamiento entre organizaciones y rendimiento aceptable.

> **Checkpoint**: Pruebas de integración pasan con distintos roles y organizaciones, sin brechas de acceso.

- [ ] T024 Crear pruebas de integración que simulen usuarios de distintos roles y orgs.
- [ ] T025 Medir tiempos de respuesta con RLS habilitado y optimizar índices/particiones.
- [ ] T026 Revisar logs para asegurar trazabilidad completa de cada operación.

---

## Fase 8: Documentación y entrega

**Purpose**: Documentar flujo de autorización, puntos de entrada y configuraciones.

> **Checkpoint**: Documentación completa y actualizada disponible en repo y wiki.

- [ ] T027 Escribir documentación sobre el flujo de Scope y RLS.
- [ ] T028 Actualizar README con instrucciones de despliegue y pruebas.
- [ ] T029 Configurar pipeline CI/CD para ejecutar pruebas y desplegar en staging.

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

