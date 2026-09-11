[
  {
    "name": "GetUsers",
    "description": "Obtiene la lista de usuarios pertenecientes al scope del solicitante.",
    "endpoint": "/api/users",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token JWT con Scope"
      },
      "queryParams": {
        "page": "número de página",
        "limit": "límites por página"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "users": [
          {
            "id": "string",
            "name": "string",
            "email": "string",
            "organization_id": "string",
            "role": "string"
          }
        ],
        "page": "número actual",
        "total_pages": "total de páginas"
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado. Falta o token inválido."
      },
      {
        "status": 403,
        "message": "Acceso prohibido. El scope no permite esta operación."
      }
    ]
  },
  {
    "name": "GetUserById",
    "description": "Obtiene los detalles de un usuario especificado, validando que el id pertenezca al scope del solicitante.",
    "endpoint": "/api/users/{id}",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token JWT con Scope"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "string",
        "name": "string",
        "email": "string",
        "organization_id": "string",
        "role": "string"
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado."
      },
      {
        "status": 403,
        "message": "Acceso prohibido. El id no pertenece al scope."
      },
      {
        "status": 404,
        "message": "Usuario no encontrado."
      }
    ]
  },
  {
    "name": "CreateUser",
    "description": "Crea un nuevo usuario dentro del scope del solicitante. El campo organization_id debe coincidir con el scope.",
    "endpoint": "/api/users",
    "method": "POST",
    "request": {
      "headers": {
        "Authorization": "Bearer token JWT con Scope"
      },
      "body": {
        "name": "string",
        "email": "string",
        "password": "string",
        "organization_id": "string",
        "role": "string"
      }
    },
    "response": {
      "status": 201,
      "body": {
        "id": "string",
        "name": "string",
        "email": "string",
        "organization_id": "string",
        "role": "string"
      }
    },
    "errors": [
      {
        "status": 400,
        "message": "Datos de entrada inválidos."
      },
      {
        "status": 401,
        "message": "No autorizado."
      },
      {
        "status": 403,
        "message": "El scope no permite crear usuarios en esta organización."
      }
    ]
  },
  {
    "name": "UpdateUser",
    "description": "Actualiza la información de un usuario, asegurando que el id esté dentro del scope del solicitante.",
    "endpoint": "/api/users/{id}",
    "method": "PUT",
    "request": {
      "headers": {
        "Authorization": "Bearer token JWT con Scope"
      },
      "body": {
        "name": "string opcional",
        "email": "string opcional",
        "password": "string opcional",
        "role": "string opcional"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "id": "string",
        "name": "string",
        "email": "string",
        "organization_id": "string",
        "role": "string"
      }
    },
    "errors": [
      {
        "status": 400,
        "message": "Datos de entrada inválidos."
      },
      {
        "status": 401,
        "message": "No autorizado."
      },
      {
        "status": 403,
        "message": "El scope no permite actualizar este usuario."
      },
      {
        "status": 404,
        "message": "Usuario no encontrado."
      }
    ]
  },
  {
    "name": "DeleteUser",
    "description": "Elimina un usuario, validando que el id esté dentro del scope del solicitante.",
    "endpoint": "/api/users/{id}",
    "method": "DELETE",
    "request": {
      "headers": {
        "Authorization": "Bearer token JWT con Scope"
      }
    },
    "response": {
      "status": 204,
      "body": {}
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado."
      },
      {
        "status": 403,
        "message": "El scope no permite eliminar este usuario."
      },
      {
        "status": 404,
        "message": "Usuario no encontrado."
      }
    ]
  },
  {
    "name": "GetOrganizations",
    "description": "Lista las organizaciones disponibles dentro del scope del solicitante.",
    "endpoint": "/api/organizations",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token JWT con Scope"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "organizations": [
          {
            "id": "string",
            "name": "string",
            "description": "string"
          }
        ]
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado."
      },
      {
        "status": 403,
        "message": "Acceso prohibido. El scope no permite ver organizaciones."
      }
    ]
  }
]