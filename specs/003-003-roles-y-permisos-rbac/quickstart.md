# Quickstart Guide

**Date**: 2026-09-10

## Overview

Esta guía rápida te llevará a través de la creación de un sistema completo de gestión de roles y permisos (RBAC) con PostgreSQL, TypeScript y Express, incluyendo la configuración de políticas RLS y auditoría.

## Prerequisites

- Node.js 20+ instalado
- PostgreSQL 15+ con extensión RLS habilitada
- Acceso a un repositorio Git para el código fuente
- Conocimientos básicos de TypeScript y Express

## Steps

### 1. Paso 1: Inicializar el proyecto

Crea un nuevo directorio y ejecuta `npm init -y`. Instala las dependencias necesarias:

```javascript
npm install express pg typeorm jsonwebtoken class-validator dotenv
npm install -D typescript ts-node-dev @types/express @types/node
```

### 2. Paso 2: Configurar TypeScript y TypeORM

Añade un archivo `tsconfig.json` con la configuración básica y crea el archivo `ormconfig.json` para conectar a PostgreSQL. Ejemplo de `ormconfig.json`:

```javascript
{
  "type": "postgres",
  "host": "localhost",
  "port": 5432,
  "username": "postgres",
  "password": "password",
  "database": "rbac_db",
  "synchronize": false,
  "logging": false,
  "entities": ["src/domain/entities/**/*.ts"],
  "migrations": ["src/infra/database/migrations/**/*.ts"],
  "subscribers": ["src/domain/subscribers/**/*.ts"],
  "cli": {
    "entitiesDir": "src/domain/entities",
    "migrationsDir": "src/infra/database/migrations",
    "subscribersDir": "src/domain/subscribers"
  }
}
```

### 3. Paso 3: Definir las entidades

Crea las entidades `Role`, `Permission`, `RolePermission`, `UserRole` y `AuditLog` en `src/domain/entities`. Ejemplo de la entidad `Role`:

```javascript
import { Entity, PrimaryGeneratedColumn, Column, CreateDateColumn, UpdateDateColumn, ManyToMany, JoinTable } from 'typeorm';
import { Permission } from './Permission';

@Entity()
export class Role {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ unique: true })
  name: string;

  @Column({ nullable: true })
  description?: string;

  @Column({ default: true })
  is_active: boolean;

  @CreateDateColumn()
  created_at: Date;

  @UpdateDateColumn()
  updated_at: Date;

  @ManyToMany(() => Permission, { eager: true })
  @JoinTable({ name: 'role_permissions' })
  permissions: Permission[];
}
```

### 4. Paso 4: Crear migraciones de base de datos

Genera migraciones con TypeORM para crear las tablas y las políticas RLS. Ejemplo de migración para la tabla `roles` y la política RLS:

```javascript
import { MigrationInterface, QueryRunner } from 'typeorm';

export class CreateRolesTable1620000000000 implements MigrationInterface {
  public async up(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.query(`
      CREATE TABLE roles (
        id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
        name TEXT NOT NULL UNIQUE,
        description TEXT,
        is_active BOOLEAN NOT NULL DEFAULT TRUE,
        created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT now(),
        updated_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT now(),
        created_by UUID NOT NULL,
        updated_by UUID NOT NULL
      );
    `);

    // Política RLS para que solo el dueño o admins vean el rol
    await queryRunner.query(`
      CREATE POLICY roles_rls ON roles
      USING (is_active OR created_by = current_setting('app.current_user_id')::uuid);
    `);
  }

  public async down(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.query('DROP TABLE IF EXISTS roles;');
    await queryRunner.query('DROP POLICY IF EXISTS roles_rls ON roles;');
  }
}
```

### 5. Paso 5: Implementar los controladores y rutas

En `src/api/controllers` crea `RoleController` con métodos `create`, `update`, `deactivate`, `list`. En `src/api/routes` define las rutas REST. Ejemplo de ruta para crear un rol:

```javascript
import { Router } from 'express';
import { RoleController } from '../controllers/RoleController';
import { authMiddleware } from '../middlewares/auth';

const router = Router();
const roleCtrl = new RoleController();

router.post('/roles', authMiddleware('admin'), roleCtrl.create.bind(roleCtrl));
export default router;
```

### 6. Paso 6: Middleware de autorización

Crea `authMiddleware` que verifica el JWT, extrae los roles y permisos y bloquea las rutas no autorizadas. Ejemplo mínimo:

```javascript
import { Request, Response, NextFunction } from 'express';
import jwt from 'jsonwebtoken';

export function authMiddleware(requiredRole: string) {
  return (req: Request, res: Response, next: NextFunction) => {
    const authHeader = req.headers.authorization;
    if (!authHeader) return res.status(401).json({ message: 'Token requerido' });

    const token = authHeader.split(' ')[1];
    try {
      const payload: any = jwt.verify(token, process.env.JWT_SECRET!);
      req.user = payload;
      if (!payload.roles.includes(requiredRole)) {
        return res.status(403).json({ message: 'Acceso denegado' });
      }
      next();
    } catch {
      return res.status(401).json({ message: 'Token inválido' });
    }
  };
}
```

### 7. Paso 7: Registrar auditoría de cambios

En cada servicio que modifique datos, inserta un registro en `AuditLog`. Ejemplo en `RoleService` al crear un rol:

```javascript
async createRole(dto: CreateRoleDto, userId: string) {
  const role = this.roleRepo.create(dto);
  role.created_by = userId;
  role.updated_by = userId;
  await this.roleRepo.save(role);

  await this.auditRepo.save({
    action: 'CREATE',
    entity: 'Role',
    entity_id: role.id,
    new_value: role,
    performed_by: userId,
    performed_at: new Date()
  });

  return role;
}
```

### 8. Paso 8: Probar la API con Vitest y Playwright

Escribe pruebas unitarias para los servicios y pruebas de integración para las rutas. Ejemplo de prueba de creación de rol con Vitest:

```javascript
import { describe, it, expect } from 'vitest';
import { createRole } from '../src/domain/services/RoleService';

describe('RoleService', () => {
  it('should create a role with unique name', async () => {
    const role = await createRole({ name: 'editor', description: 'Can edit content' }, 'admin-id');
    expect(role.name).toBe('editor');
  });
});
```

### 9. Paso 9: Desplegar y monitorear

Una vez que todas las pruebas pasen, despliega la aplicación en tu entorno (Docker, Kubernetes, etc.). Configura monitoreo de logs y alertas para auditorías y errores de autorización.

