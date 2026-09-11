# Implementation Plan: 004 - Capabilities

**Branch**: `c9375691-af38-4922-bca6-8583d4da8406` | **Date**: 2026-09-10 | **Spec**: [spec.md](spec.md)
**Input**: Feature specification from spec.md

## Summary

Plan de implementación de capacidades con gestión de roles y usuarios, validación de unicidad, scope por compañía y auditoría.

## Technical Context

| Aspect | Value |
|--------|-------|
| Language/Version | TypeScript |
| Primary Dependencies | express, typeorm, pg, class-validator, class-transformer, winston, dotenv |
| Storage | PostgreSQL |
| Testing | Vitest y Playwright |
| Target Platform | Web (API REST y UI React) |
| Project Type | single |
| Performance Goals | TBD |
| Constraints | TBD |
| Scale/Scope | TBD |

## Constitution Check

*GATE: Must pass before implementation*

_No constitution checks defined_

## Project Structure

### Source Code

```text
src/
├─ api/                     # Endpoints REST con Express
│  ├─ controllers/          # Lógica de manejo de peticiones
│  ├─ routes/               # Definición de rutas
│  └─ middleware/           # Autenticación, auditoría, validaciones
├─ domain/                   # Entidades, Value Objects, Repositorios
│  ├─ capability/           # Entidad Capability, DTOs
│  ├─ role/                 # Entidad Role y asignaciones
│  └─ user/                 # Entidad User y asignaciones directas
├─ infrastructure/          # Implementaciones concretas de repositorios
│  ├─ db/                   # Conexión a PostgreSQL, migraciones
│  └─ logger/               # Configuración de Winston
├─ application/             # Casos de uso, servicios
│  ├─ capability/           # Create, Update, Delete, List
│  ├─ assignment/           # Asignar/Remover a roles y usuarios
│  └─ permission/           # Calcular capacidades efectivas y scope
├─ tests/                   # Tests unitarios y e2e
│  ├─ unit/                 # Vitest
│  └─ e2e/                  # Playwright
└─ config/                  # Variables de entorno y configuración global
```

**Structure Decision**: Separar la capa de dominio, infraestructura y aplicación para facilitar pruebas, escalabilidad y mantenibilidad. Utilizar monorepo con paquetes independientes.

## Quality Gates

- [ ] All tests pass
- [ ] Code follows project style
- [ ] No security vulnerabilities

## Complexity Tracking

_No complexity violations_

