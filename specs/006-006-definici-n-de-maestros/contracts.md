[
  {
    "name": "CreateMasterDefinition",
    "description": "Crear un nuevo Maestro con sus campos, permisos y relaciones asociadas.",
    "endpoint": "/api/master-definitions",
    "method": "POST",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "code": "string",
        "name": "string",
        "description": "string",
        "status": "active|inactive",
        "display_field": "string",
        "fields": [
          {
            "field_name": "string",
            "field_type": "string",
            "is_visible": "boolean",
            "is_required": "boolean",
            "is_display_field": "boolean",
            "order": "integer"
          }
        ],
        "permissions": [
          {
            "role_id": "uuid",
            "can_create": "boolean",
            "can_update": "boolean",
            "can_read": "boolean",
            "can_delete": "boolean"
          }
        ],
        "relations": [
          {
            "related_master_definition_id": "uuid",
            "relation_type": "one-to-one|one-to-many|many-to-many",
            "description": "string"
          }
        ]
      }
    },
    "response": {
      "status": 201,
      "body": {
        "message": "Maestro creado exitosamente.",
        "master": {
          "id": "uuid",
          "organization_id": "uuid",
          "code": "string",
          "name": "string",
          "description": "string",
          "status": "active|inactive",
          "display_field": "string",
          "created_at": "timestamp",
          "updated_at": "timestamp",
          "created_by": "uuid",
          "updated_by": "uuid"
        }
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
        "message": "Permiso denegado."
      },
      {
        "status": 409,
        "message": "Código duplicado dentro de la organización."
      }
    ]
  },
  {
    "name": "UpdateMasterDefinition",
    "description": "Actualizar los atributos y relaciones de un Maestro existente.",
    "endpoint": "/api/master-definitions/{id}",
    "method": "PUT",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": {
        "name": "string",
        "description": "string",
        "status": "active|inactive",
        "display_field": "string",
        "fields": [
          {
            "id": "uuid",
            "field_name": "string",
            "field_type": "string",
            "is_visible": "boolean",
            "is_required": "boolean",
            "is_display_field": "boolean",
            "order": "integer"
          }
        ],
        "permissions": [
          {
            "id": "uuid",
            "role_id": "uuid",
            "can_create": "boolean",
            "can_update": "boolean",
            "can_read": "boolean",
            "can_delete": "boolean"
          }
        ],
        "relations": [
          {
            "id": "uuid",
            "related_master_definition_id": "uuid",
            "relation_type": "one-to-one|one-to-many|many-to-many",
            "description": "string"
          }
        ]
      }
    },
    "response": {
      "status": 200,
      "body": {
        "message": "Maestro actualizado exitosamente.",
        "master": {
          "id": "uuid",
          "organization_id": "uuid",
          "code": "string",
          "name": "string",
          "description": "string",
          "status": "active|inactive",
          "display_field": "string",
          "created_at": "timestamp",
          "updated_at": "timestamp",
          "created_by": "uuid",
          "updated_by": "uuid"
        }
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
        "message": "Permiso denegado."
      },
      {
        "status": 404,
        "message": "Maestro no encontrado."
      }
    ]
  },
  {
    "name": "GetMasterDefinitions",
    "description": "Consultar la lista de Maestros con filtrado, paginación y datos de auditoría básica.",
    "endpoint": "/api/master-definitions",
    "method": "GET",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "queryParams": {
        "page": "integer",
        "pageSize": "integer",
        "filterCode": "string",
        "filterName": "string",
        "filterStatus": "active|inactive"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "masters": [
          {
            "id": "uuid",
            "organization_id": "uuid",
            "code": "string",
            "name": "string",
            "description": "string",
            "status": "active|inactive",
            "display_field": "string",
            "created_at": "timestamp",
            "updated_at": "timestamp",
            "created_by": "uuid",
            "updated_by": "uuid",
            "audit": {
              "created_at": "timestamp",
              "created_by": "uuid",
              "updated_at": "timestamp",
              "updated_by": "uuid"
            }
          }
        ],
        "pagination": {
          "page": "integer",
          "pageSize": "integer",
          "totalPages": "integer",
          "totalRecords": "integer"
        }
      }
    },
    "errors": [
      {
        "status": 401,
        "message": "No autorizado."
      },
      {
        "status": 403,
        "message": "Permiso denegado."
      }
    ]
  },
  {
    "name": "DeactivateMasterDefinition",
    "description": "Desactivar un Maestro para que ya no esté disponible para nuevas relaciones.",
    "endpoint": "/api/master-definitions/{id}/deactivate",
    "method": "POST",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      }
    },
    "response": {
      "status": 200,
      "body": {
        "message": "Maestro desactivado exitosamente.",
        "master": {
          "id": "uuid",
          "status": "inactive",
          "updated_at": "timestamp",
          "updated_by": "uuid"
        }
      }
    },
    "errors": [
      {
        "status": 400,
        "message": "El Maestro ya está inactivo."
      },
      {
        "status": 401,
        "message": "No autorizado."
      },
      {
        "status": 403,
        "message": "Permiso denegado."
      },
      {
        "status": 404,
        "message": "Maestro no encontrado."
      }
    ]
  },
  {
    "name": "ConfigureMasterFields",
    "description": "Agregar, actualizar o eliminar campos descriptivos y de visibilidad de un Maestro.",
    "endpoint": "/api/master-definitions/{id}/fields",
    "method": "PATCH",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": [
        {
          "id": "uuid",
          "field_name": "string",
          "field_type": "string",
          "is_visible": "boolean",
          "is_required": "boolean",
          "is_display_field": "boolean",
          "order": "integer",
          "action": "create|update|delete"
        }
      ]
    },
    "response": {
      "status": 200,
      "body": {
        "message": "Campos de maestro configurados exitosamente."
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
        "message": "Permiso denegado."
      },
      {
        "status": 404,
        "message": "Maestro o campo no encontrado."
      }
    ]
  },
  {
    "name": "ConfigureMasterPermissions",
    "description": "Asignar permisos de acceso a un Maestro por roles.",
    "endpoint": "/api/master-definitions/{id}/permissions",
    "method": "PATCH",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": [
        {
          "id": "uuid",
          "role_id": "uuid",
          "can_create": "boolean",
          "can_update": "boolean",
          "can_read": "boolean",
          "can_delete": "boolean",
          "action": "create|update|delete"
        }
      ]
    },
    "response": {
      "status": 200,
      "body": {
        "message": "Permisos de maestro configurados exitosamente."
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
        "message": "Permiso denegado."
      },
      {
        "status": 404,
        "message": "Maestro o permiso no encontrado."
      }
    ]
  },
  {
    "name": "ConfigureMasterRelations",
    "description": "Definir relaciones entre maestros sin crear tablas intermedias.",
    "endpoint": "/api/master-definitions/{id}/relations",
    "method": "PATCH",
    "request": {
      "headers": {
        "Authorization": "Bearer token"
      },
      "body": [
        {
          "id": "uuid",
          "related_master_definition_id": "uuid",
          "relation_type": "one-to-one|one-to-many|many-to-many",
          "description": "string",
          "action": "create|update|delete"
        }
      ]
    },
    "response": {
      "status": 200,
      "body": {
        "message": "Relaciones de maestro configuradas exitosamente."
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
        "message": "Permiso denegado."
      },
      {
        "status": 404,
        "message": "Maestro o relación no encontrada."
      }
    ]
  }
]