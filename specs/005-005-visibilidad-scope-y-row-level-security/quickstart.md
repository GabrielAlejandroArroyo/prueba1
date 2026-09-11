# Quickstart Guide

**Date**: 2026-09-10

## Overview

En esta guía rápida aprenderás a crear un sistema de autorización basado en Scope y Row Level Security (RLS) en Supabase, utilizando un middleware de autenticación en Express, filtros automáticos en las consultas SELECT y políticas RLS que reflejen los Scopes definidos.

## Prerequisites

- Node.js 20 o superior
- Cuenta y proyecto en Supabase con PostgreSQL
- Acceso a la consola de Supabase para crear tablas y políticas RLS
- Conocimientos básicos de TypeScript y Express

## Steps

### 1. Paso 1: Inicializar el proyecto

Crea un nuevo directorio y ejecuta `npm init -y`. Instala las dependencias necesarias:

```bash
npm install express @supabase/supabase-js jsonwebtoken dotenv winston
```

### 2. Paso 2: Configurar variables de entorno

En la raíz del proyecto crea un archivo `.env` con las siguientes variables:

```dotenv
SUPABASE_URL=https://<tu-proyecto>.supabase.co
SUPABASE_ANON_KEY=tu-anon-key
JWT_SECRET=una-clave-secreta
```

### 3. Paso 3: Crear el middleware de autenticación

En `src/middleware/auth.ts` define un middleware que verifique el token JWT, calcule el Scope basado en el rol y la organización, y lo adjunte al contexto de la solicitud.

```typescript
// src/middleware/auth.ts
import { Request, Response, NextFunction } from "express";
import jwt from "jsonwebtoken";
import { getScopeForUser } from "../services/scopeService";

export const authMiddleware = async (req: Request, res: Response, next: NextFunction) => {
  const authHeader = req.headers.authorization;
  if (!authHeader || !authHeader.startsWith("Bearer ")) {
    return res.status(401).json({ error: "Token no proporcionado" });
  }

  const token = authHeader.split(" ")[1];
  try {
    const payload: any = jwt.verify(token, process.env.JWT_SECRET!);
    const scope = await getScopeForUser(payload.user_id, payload.role, payload.organization_id);
    // Adjuntar datos al request
    (req as any).user = payload;
    (req as any).scope = scope;
    next();
  } catch (err) {
    return res.status(401).json({ error: "Token inválido" });
  }
};
```

### 4. Paso 4: Implementar el servicio de Scope

En `src/services/scopeService.ts` calcula el Scope efectivo según el rol y la organización del usuario.

```typescript
// src/services/scopeService.ts
export const getScopeForUser = async (userId: string, role: string, orgId: string) => {
  // Ejemplo simple: el Scope es el ID de la organización
  // Se puede ampliar para roles con acceso a múltiples orgs
  return { organizationId: orgId, role };
};
```

### 5. Paso 5: Aplicar filtros automáticos en las consultas SELECT

En los servicios de datos, añade el filtro `WHERE organization_id = $1` usando el Scope del request.

```typescript
// src/services/userService.ts
import { createClient } from "@supabase/supabase-js";
const supabase = createClient(process.env.SUPABASE_URL!, process.env.SUPABASE_ANON_KEY!);

export const getUsers = async (req: any) => {
  const { organizationId } = req.scope;
  const { data, error } = await supabase
    .from("users")
    .select()
    .eq("organization_id", organizationId);
  if (error) throw error;
  return data;
};
```

### 6. Paso 6: Configurar políticas RLS en Supabase

En la consola de Supabase, habilita RLS en las tablas relevantes y crea políticas que verifiquen `organization_id` contra el valor que proviene del JWT.

```sql
CREATE POLICY "Users only visible to org" ON users
FOR SELECT USING (organization_id = auth.jwt() ->> 'organization_id');
```

### 7. Paso 7: Registrar acciones con Winston

En `src/utils/logger.ts` configura Winston para registrar el usuario, el Scope y la acción realizada.

```typescript
// src/utils/logger.ts
import { createLogger, transports, format } from "winston";
export const logger = createLogger({
  level: "info",
  format: format.combine(
    format.timestamp(),
    format.json()
  ),
  transports: [new transports.Console()]
});
```

### 8. Paso 8: Probar la implementación

Con Jest y Supertest escribe pruebas que verifiquen:
- Que los usuarios no pueden acceder a datos de otras organizaciones.
- Que las operaciones DML fuera del Scope devuelven 403.
- Que las políticas RLS bloquean accesos indebidos.

```typescript
// Ejemplo de prueba con Jest
import request from "supertest";
import app from "../src/app";

describe("Scope y RLS", () => {
  it("debe bloquear acceso a otra organización", async () => {
    const res = await request(app)
      .get("/api/users/123")
      .set("Authorization", "Bearer token-usuario-1");
    expect(res.status).toBe(403);
  });
});
```

