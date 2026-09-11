[
  {
    "name": "CrearOrganizacion",
    "description": "Crear una nueva organización",
    "endpoint": "/api/organizations",
    "method": "POST",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "id": "int",
        "code": "string",
        "name": "string",
        "description": "string",
        "status": "string"
      }
    },
    "response": {
      "status": 201,
      "body": {
        "id": "int",
        "code": "string",
        "name": "string",
        "description": "string",
        "status": "string",
        "created_at": "string",
        "updated_at": "string"
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
        "message": "Acceso denegado"
      },
      {
        "status": 409,
        "message": "Código de organización duplicado"
      }
    ]
  },
  {
    "name": "ListarOrganizaciones",
    "description": "Listar todas las organizaciones con paginación",
    "endpoint": "/api/organizations",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "queryParams": {
        "page": "int",
        "size": "int"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "organizations": [
          {
            "id": "int",
            "code": "string",
            "name": "string",
            "description": "string",
            "status": "string",
            "created_at": "string",
            "updated_at": "string"
          }
        ],
        "page": "int",
        "size": "int",
        "total": "int"
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Acceso denegado"
      }
    ]
  },
  {
    "name": "ObtenerOrganizacion",
    "description": "Obtener detalles de una organización",
    "endpoint": "/api/organizations/{id}",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "int",
        "code": "string",
        "name": "string",
        "description": "string",
        "status": "string",
        "created_at": "string",
        "updated_at": "string"
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Organización no encontrada"
      }
    ]
  },
  {
    "name": "ActualizarOrganizacion",
    "description": "Actualizar nombre, descripción y estado de una organización (code no modificable)",
    "endpoint": "/api/organizations/{id}",
    "method": "PUT",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "name": "string",
        "description": "string",
        "status": "string"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "int",
        "code": "string",
        "name": "string",
        "description": "string",
        "status": "string",
        "created_at": "string",
        "updated_at": "string"
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
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Organización no encontrada"
      }
    ]
  },
  {
    "name": "CambiarEstadoOrganizacion",
    "description": "Activar o desactivar una organización",
    "endpoint": "/api/organizations/{id}/status",
    "method": "PATCH",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "status": "string"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "int",
        "status": "string"
      }
    },
    "errors": [
      {
        "status": 400,
        "message": "Estado inválido"
      },
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Organización no encontrada"
      }
    ]
  },
  {
    "name": "CrearUsuario",
    "description": "Crear un nuevo perfil de usuario",
    "endpoint": "/api/users",
    "method": "POST",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "email": "string",
        "name": "string",
        "password": "string",
        "status": "string",
        "organization_id": "int"
      }
    },
    "response": {
      "status": 201,
      "body": {
        "id": "int",
        "email": "string",
        "name": "string",
        "status": "string",
        "organization_id": "int",
        "created_at": "string",
        "updated_at": "string"
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
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Organización no encontrada"
      },
      {
        "status": 409,
        "message": "Email duplicado"
      }
    ]
  },
  {
    "name": "ListarUsuarios",
    "description": "Listar perfiles de usuario con filtrado por organización",
    "endpoint": "/api/users",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "queryParams": {
        "page": "int",
        "size": "int",
        "organization_id": "int"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "users": [
          {
            "id": "int",
            "email": "string",
            "name": "string",
            "status": "string",
            "organization_id": "int",
            "is_super_admin": "boolean"
          }
        ],
        "page": "int",
        "size": "int",
        "total": "int"
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Acceso denegado"
      }
    ]
  },
  {
    "name": "ObtenerUsuario",
    "description": "Obtener detalles de un perfil de usuario",
    "endpoint": "/api/users/{id}",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "int",
        "email": "string",
        "name": "string",
        "status": "string",
        "organization_id": "int",
        "is_super_admin": "boolean",
        "created_at": "string",
        "updated_at": "string"
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Usuario no encontrado"
      }
    ]
  },
  {
    "name": "ActualizarUsuario",
    "description": "Actualizar datos de un usuario, incluida la asociación a organización",
    "endpoint": "/api/users/{id}",
    "method": "PUT",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "email": "string",
        "name": "string",
        "password": "string",
        "status": "string",
        "organization_id": "int"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "int",
        "email": "string",
        "name": "string",
        "status": "string",
        "organization_id": "int",
        "updated_at": "string"
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
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Usuario no encontrado"
      },
      {
        "status": 404,
        "message": "Organización no encontrada"
      }
    ]
  },
  {
    "name": "CambiarEstadoUsuario",
    "description": "Activar o desactivar un perfil de usuario",
    "endpoint": "/api/users/{id}/status",
    "method": "PATCH",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "status": "string"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "int",
        "status": "string"
      }
    },
    "errors": [
      {
        "status": 400,
        "message": "Estado inválido"
      },
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Usuario no encontrado"
      }
    ]
  },
  {
    "name": "AsociarUsuarioOrganizacion",
    "description": "Asociar un usuario existente a una organización",
    "endpoint": "/api/users/{id}/associate",
    "method": "POST",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "organization_id": "int"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "int",
        "organization_id": "int"
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
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Usuario no encontrado"
      },
      {
        "status": 404,
        "message": "Organización no encontrada"
      }
    ]
  },
  {
    "name": "VerRolesUsuario",
    "description": "Obtener roles asignados a un usuario",
    "endpoint": "/api/users/{id}/roles",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "roles": [
          {
            "id": "int",
            "name": "string",
            "description": "string",
            "status": "string"
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
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Usuario no encontrado"
      }
    ]
  },
  {
    "name": "VerCapacidadesUsuario",
    "description": "Obtener capabilities derivadas de los roles de un usuario",
    "endpoint": "/api/users/{id}/capabilities",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "capabilities": [
          {
            "id": "int",
            "name": "string",
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
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Usuario no encontrado"
      }
    ]
  },
  {
    "name": "EliminarOrganizacion",
    "description": "Eliminar una organización (solo super admin)",
    "endpoint": "/api/organizations/{id}",
    "method": "DELETE",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 204
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Organización no encontrada"
      }
    ]
  },
  {
    "name": "EliminarUsuario",
    "description": "Eliminar un perfil de usuario",
    "endpoint": "/api/users/{id}",
    "method": "DELETE",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 204
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado"
      },
      {
        "status": 403,
        "message": "Acceso denegado"
      },
      {
        "status": 404,
        "message": "Usuario no encontrado"
      }
    ]
  }
]