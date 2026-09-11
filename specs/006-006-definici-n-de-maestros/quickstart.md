# Quickstart Guide

**Date**: 2026-09-10

## Overview

Guía rápida para implementar y probar la gestión de Maestros en una aplicación basada en metadata. Incluye la configuración del entorno, la ejecución de migraciones, el inicio del servidor y ejemplos de llamadas API para crear, editar, consultar y desactivar Maestros.

## Prerequisites

- Node.js 20+ y npm instalados
- PostgreSQL 15+ corriendo localmente o accesible vía red
- Docker (opcional, para levantar la base de datos con docker-compose)
- Postman o curl para probar los endpoints

## Steps

### 1. Step 1: Clonar el repositorio

Descarga el código fuente del proyecto desde el repositorio Git.

```bash
git clone https://github.com/ejemplo/masters-management.git
cd masters-management
```

### 2. Step 2: Instalar dependencias

Instala todas las dependencias de npm necesarias para compilar y ejecutar la aplicación.

```bash
npm install
```

### 3. Step 3: Configurar variables de entorno

Copia el archivo de ejemplo y rellena los valores para la conexión a PostgreSQL y el puerto del servidor.

```bash
cp .env.example .env
# Edita .env con tu configuración
```

### 4. Step 4: Levantar la base de datos (opcional con Docker)

Si prefieres usar Docker, puedes levantar una instancia de PostgreSQL con el siguiente comando:

```bash
docker compose up -d db
```

### 5. Step 5: Ejecutar migraciones

Aplica las migraciones de TypeORM para crear las tablas necesarias.

```bash
npm run migration:run
```

### 6. Step 6: Iniciar el servidor

Arranca el servidor Express en modo desarrollo.

```bash
npm run dev
```

### 7. Step 7: Probar la API de creación de Maestro

Envía una solicitud POST a /api/master-definitions para crear un nuevo Maestro. Asegúrate de incluir los encabezados de autenticación y la organización.

```bash
curl -X POST http://localhost:3000/api/master-definitions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <tu_token>" \
  -d '{
    "organization_id": "uuid-de-la-org",
    "code": "EMP",
    "name": "Empleados",
    "description": "Tabla de empleados",
    "status": "active",
    "display_field": "name"
  }'
```

### 8. Step 8: Editar un Maestro existente

Actualiza los campos de un Maestro existente usando PUT. El endpoint requiere el id del Maestro.

```bash
curl -X PUT http://localhost:3000/api/master-definitions/<id> \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <tu_token>" \
  -d '{
    "name": "Empleados Actualizados",
    "status": "active"
  }'
```

### 9. Step 9: Consultar Maestros con filtros y paginación

Obtén una lista de Maestros aplicando filtros por código y estado. La respuesta incluye paginación.

```bash
curl -X GET "http://localhost:3000/api/master-definitions?code=EMP&status=active&page=1&limit=10" \
  -H "Authorization: Bearer <tu_token>"
```

### 10. Step 10: Desactivar un Maestro

Cambia el estado de un Maestro a inactivo. Se requiere confirmación si existen dependencias activas.

```bash
curl -X PATCH http://localhost:3000/api/master-definitions/<id>/deactivate \
  -H "Authorization: Bearer <tu_token>"
```

