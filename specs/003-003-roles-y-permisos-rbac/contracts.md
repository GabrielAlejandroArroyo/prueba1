[
  {
    "name": "CrearRol",
    "description": "Crear un nuevo rol",
    "endpoint": "/api/roles",
    "method": "POST",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "name": "string",
        "description": "string"
      }
    },
    "response": {
      "status": 201,
      "body": {
        "id": "uuid",
        "name": "string",
        "description": "string",
        "is_active": true,
        "created_at": "timestamp",
        "created_by": "uuid"
      }
    },
    "errors": [
      {
        "status": 400,
        "message": "Datos inválidos"
      },
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      },
      {
        "status": 409,
        "message": "Rol ya existe"
      }
    ]
  },
  {
    "name": "ListarRoles",
    "description": "Listar roles con paginación y filtrado por estado",
    "endpoint": "/api/roles",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "queryParams": {
        "page": "número de página",
        "limit": "tamaño de página",
        "is_active": "booleano"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "roles": [
          {
            "id": "uuid",
            "name": "string",
            "description": "string",
            "is_active": true,
            "created_at": "timestamp",
            "updated_at": "timestamp"
          }
        ],
        "page": 1,
        "limit": 20,
        "total": 100
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      }
    ]
  },
  {
    "name": "ObtenerRol",
    "description": "Obtener los detalles de un rol",
    "endpoint": "/api/roles/{roleId}",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "uuid",
        "name": "string",
        "description": "string",
        "is_active": true,
        "created_at": "timestamp",
        "updated_at": "timestamp"
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      },
      {
        "status": 404,
        "message": "Rol no encontrado"
      }
    ]
  },
  {
    "name": "ActualizarRol",
    "description": "Actualizar nombre o descripción de un rol",
    "endpoint": "/api/roles/{roleId}",
    "method": "PUT",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "name": "string",
        "description": "string"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "uuid",
        "name": "string",
        "description": "string",
        "is_active": true,
        "updated_at": "timestamp",
        "updated_by": "uuid"
      }
    },
    "errors": [
      {
        "status": 400,
        "message": "Datos inválidos"
      },
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      },
      {
        "status": 404,
        "message": "Rol no encontrado"
      },
      {
        "status": 409,
        "message": "Nombre de rol duplicado"
      }
    ]
  },
  {
    "name": "DesactivarRol",
    "description": "Marcar un rol como inactivo",
    "endpoint": "/api/roles/{roleId}/deactivate",
    "method": "PATCH",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "uuid",
        "is_active": false,
        "updated_at": "timestamp",
        "updated_by": "uuid"
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      },
      {
        "status": 404,
        "message": "Rol no encontrado"
      }
    ]
  },
  {
    "name": "CrearPermiso",
    "description": "Crear un nuevo permiso",
    "endpoint": "/api/permissions",
    "method": "POST",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "identifier": "string",
        "description": "string"
      }
    },
    "response": {
      "status": 201,
      "body": {
        "id": "uuid",
        "identifier": "string",
        "description": "string",
        "created_at": "timestamp",
        "created_by": "uuid"
      }
    },
    "errors": [
      {
        "status": 400,
        "message": "Datos inválidos"
      },
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      },
      {
        "status": 409,
        "message": "Permiso ya existe"
      }
    ]
  },
  {
    "name": "ListarPermisos",
    "description": "Listar todos los permisos",
    "endpoint": "/api/permissions",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "queryParams": {
        "page": "número de página",
        "limit": "tamaño de página"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "permissions": [
          {
            "id": "uuid",
            "identifier": "string",
            "description": "string",
            "created_at": "timestamp"
          }
        ],
        "page": 1,
        "limit": 20,
        "total": 50
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      }
    ]
  },
  {
    "name": "AsociarPermisosRol",
    "description": "Asignar múltiples permisos a un rol",
    "endpoint": "/api/roles/{roleId}/permissions",
    "method": "POST",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "permission_ids": [
          "uuid",
          "uuid"
        ]
      }
    },
    "response": {
      "status": 200,
      "body": {
        "role_id": "uuid",
        "permission_ids": [
          "uuid",
          "uuid"
        ]
      }
    },
    "errors": [
      {
        "status": 400,
        "message": "Datos inválidos"
      },
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      },
      {
        "status": 404,
        "message": "Rol o permiso no encontrado"
      },
      {
        "status": 409,
        "message": "Asociación ya existente"
      }
    ]
  },
  {
    "name": "ListarPermisosRol",
    "description": "Listar los permisos asociados a un rol",
    "endpoint": "/api/roles/{roleId}/permissions",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "role_id": "uuid",
        "permissions": [
          {
            "id": "uuid",
            "identifier": "string",
            "description": "string"
          }
        ]
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      },
      {
        "status": 404,
        "message": "Rol no encontrado"
      }
    ]
  },
  {
    "name": "AsignarRolesUsuario",
    "description": "Asignar uno o varios roles a un usuario",
    "endpoint": "/api/users/{userId}/roles",
    "method": "POST",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "role_ids": [
          "uuid",
          "uuid"
        ]
      }
    },
    "response": {
      "status": 200,
      "body": {
        "user_id": "uuid",
        "role_ids": [
          "uuid",
          "uuid"
        ]
      }
    },
    "errors": [
      {
        "status": 400,
        "message": "Datos inválidos"
      },
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      },
      {
        "status": 404,
        "message": "Usuario o rol no encontrado"
      },
      {
        "status": 409,
        "message": "Rol ya asignado"
      }
    ]
  },
  {
    "name": "RemoverRolUsuario",
    "description": "Eliminar un rol asignado a un usuario",
    "endpoint": "/api/users/{userId}/roles/{roleId}",
    "method": "DELETE",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 204,
      "body": {}
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      },
      {
        "status": 404,
        "message": "Asignación no encontrada"
      }
    ]
  },
  {
    "name": "ListarRolesUsuario",
    "description": "Listar los roles asignados a un usuario",
    "endpoint": "/api/users/{userId}/roles",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "user_id": "uuid",
        "roles": [
          {
            "id": "uuid",
            "name": "string",
            "description": "string",
            "is_active": true
          }
        ]
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Sin permiso"
      },
      {
        "status": 404,
        "message": "Usuario no encontrado"
      }
    ]
  }
]