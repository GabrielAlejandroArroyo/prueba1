# Research: Investigación de decisiones técnicas y mejores prácticas para la gestión de usuarios y organizaciones, enfocada en la implementación segura, escalable y mantenible de los requisitos especificados.

**Date**: 2026-09-10

## Overview

Investigación de decisiones técnicas y mejores prácticas para la gestión de usuarios y organizaciones, enfocada en la implementación segura, escalable y mantenible de los requisitos especificados.

## Arquitectura de la API

Se recomienda una arquitectura RESTful con endpoints bien definidos para cada entidad:

- `POST /organizations` – crear organización
- `PUT /organizations/{id}` – editar
- `PATCH /organizations/{id}/status` – activar/desactivar
- `GET /organizations` – listar con filtrado por status
- `POST /users/{userId}/organization` – asociar usuario
- `GET /users/{userId}/roles` – roles y capabilities

La capa de servicio debe separar la lógica de negocio de la capa de persistencia, permitiendo la inyección de repositorios y facilitando pruebas unitarias.

Ejemplo de controlador en Java Spring Boot:
```java
@PostMapping("/organizations")
public ResponseEntity<OrganizationDto> create(@Valid @RequestBody OrganizationDto dto) {
    Organization org = service.create(dto);
    return ResponseEntity.status(HttpStatus.CREATED).body(toDto(org));
}
```

### Key Points

- Endpoints claros y coherentes con la semántica HTTP.
- Separación de responsabilidades entre controlador, servicio y repositorio.
- Uso de DTOs para desacoplar la capa de presentación del modelo de dominio.

## Gestión de Roles y Permisos

Implementar un sistema de RBAC con tablas `roles`, `capabilities` y `role_capabilities`. Cada usuario tiene un conjunto de roles; las capabilities se derivan de los roles.

Para la verificación de permisos, usar un interceptor que consulte el `organization_id` del perfil y el rol del usuario. El Super Admin tendrá un flag `is_super_admin` que bypassa todas las restricciones.

Ejemplo de verificación en Spring Security:
```java
public boolean hasPermission(Authentication auth, String action) {
    UserPrincipal user = (UserPrincipal) auth.getPrincipal();
    if (user.isSuperAdmin()) return true;
    return user.getRoles().stream()
        .flatMap(r -> r.getCapabilities().stream())
        .anyMatch(c -> c.getName().equals(action));
}
```

### Key Points

- RBAC con roles y capabilities separados.
- Super Admin con flag dedicado.
- Intercepción de permisos basada en la organización del usuario.

## Validación de Unicidad del Código de Organización

Para garantizar la unicidad del campo `code`, la base de datos debe definir un índice único. Además, la capa de servicio debe realizar una búsqueda previa y lanzar una excepción personalizada `DuplicateCodeException`.

Ejemplo en Spring Data JPA:
```java
@Query("SELECT COUNT(o) FROM Organization o WHERE o.code = :code")
long countByCode(@Param("code") String code);
```
Si el conteo > 0, lanzar excepción y devolver `409 Conflict`.

### Key Points

- Índice único en la tabla de organizaciones.
- Validación en la capa de servicio para mensajes de error claros.
- Respuesta HTTP 409 con detalle de la violación.

## Restricción de Acceso por Organización

Cada endpoint que opera sobre usuarios debe filtrar por `organization_id` del perfil del administrador. En la capa de repositorio, añadir un parámetro `orgId` en las consultas.

Ejemplo de método repository:
```java
@Query("SELECT u FROM User u WHERE u.organization.id = :orgId AND u.email = :email")
Optional<User> findByEmailAndOrg(@Param("email") String email, @Param("orgId") Long orgId);
```
Si no coincide, lanzar `AccessDeniedException` y devolver `403 Forbidden`.

### Key Points

- Filtrado por organización en todas las consultas.
- Manejo de excepciones centralizado.
- Mensajes de error estandarizados.

## Gestión de Estados Activo/Inactivo

Los estados se modelan con un enum `Status { ACTIVE, INACTIVE }`. Cambios de estado deben ser atómicos y se deben registrar en un log de auditoría. Cuando una organización se desactiva, se bloquea la creación de nuevos perfiles y se invalidan los tokens de los usuarios activos.

Ejemplo de lógica de desactivación:
```java
@Transactional
public void deactivate(Long orgId) {
    Organization org = repo.findById(orgId).orElseThrow();
    org.setStatus(Status.INACTIVE);
    repo.save(org);
    userRepo.invalidateTokensByOrg(orgId);
}
```


### Key Points

- Enum para estados.
- Transacciones atómicas para cambios de estado.
- Invalidación de tokens al desactivar.

## Notificaciones y Comunicación

Al asociar un usuario a una organización, se debe enviar una notificación por correo. Utilizar un servicio de mensajería asíncrona (RabbitMQ, Kafka) para desacoplar la lógica de negocio y la entrega de mensajes.

Ejemplo de envío:
```java
notificationService.send(
    new Email(
        user.getEmail(),
        "Asociación a organización", 
        "Has sido asociado a " + org.getName()
    )
);
```

### Key Points

- Mensajes asíncronos para alta disponibilidad.
- Plantillas de correo reutilizables.
- Registro de eventos en log de auditoría.

## Manejo de Errores y Mensajes

Definir un esquema de error consistente con códigos de error internos y mensajes legibles. Utilizar excepciones personalizadas y un `@ControllerAdvice` en Spring Boot.

Ejemplo de excepción:
```java
public class OrganizationNotFoundException extends RuntimeException {
    public OrganizationNotFoundException(Long id) {
        super("Organization not found: " + id);
    }
}
```
`@ControllerAdvice` convierte la excepción en:
```json
{ "error": "OrganizationNotFound", "message": "Organization not found: 42", "status": 404 }
```

### Key Points

- Excepciones específicas por caso.
- Respuesta JSON con código y mensaje.
- Documentación de errores en OpenAPI.

## Pruebas y Calidad

Para asegurar la cobertura, se deben implementar:

- Tests unitarios de servicios con mocks de repositorios.
- Tests de integración con la base de datos H2 en memoria.
- Tests de seguridad que verifiquen que un usuario no puede acceder a otra organización.
- Tests de concurrencia para la creación de organizaciones con códigos simultáneos.

Ejemplo de test de concurrencia en JUnit 5:
```java
@Test
void concurrentDuplicateCode() throws InterruptedException {
    ExecutorService pool = Executors.newFixedThreadPool(2);
    CountDownLatch latch = new CountDownLatch(1);
    Callable<Void> task = () -> {
        latch.await();
        try { service.create(dto("ORG1")); } catch (DuplicateCodeException e) {}
        return null;
    };
    pool.submit(task); pool.submit(task); latch.countDown();
    pool.shutdown(); pool.awaitTermination(5, TimeUnit.SECONDS);
    assertEquals(1, repo.countByCode("ORG1"));
}
```

### Key Points

- Cobertura de pruebas al menos 80%.
- Tests de seguridad y concurrencia.
- Uso de H2 y Testcontainers para entornos de prueba.

## Escalabilidad y Rendimiento

Para grandes volúmenes de usuarios y organizaciones:

- Indexar columnas `organization_id`, `status` y `code`.
- Usar paginación (`Pageable`) en listas de usuarios y organizaciones.
- Cachear resultados de listas estáticas con Redis.
- Implementar circuit breaker (Hystrix, Resilience4j) para llamadas externas como correo.

Ejemplo de paginación en Spring Data:
```java
Page<User> list = userRepo.findAllByOrganizationId(orgId, PageRequest.of(0, 50));
```

### Key Points

- Índices adecuados en la BD.
- Paginación y caching.
- Resiliencia en servicios externos.

