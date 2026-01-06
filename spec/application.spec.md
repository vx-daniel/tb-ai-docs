# ThingsBoard Module: application

## Language and Context
- **Language:** Java (Spring Boot)
- **Context:** Main server application for ThingsBoard, providing REST APIs, business logic, and integration points for IoT device management and data processing.

## Structure and Key Components
- **Controllers:** Handle HTTP requests, validate input, and delegate to services. All extend `BaseController` for shared logic and error handling.
- **Services:** Implement business logic, interact with DAOs, and manage entities (devices, assets, users, alarms, etc.).
- **Configuration:** Spring configuration classes for security, web, and application settings.
- **Exception Handling:** Centralized in `BaseController` for consistent error responses.

## Example: BaseController
- **Purpose:** Provides shared services, error handling, and logging for all REST controllers.
- **Key Variables:** Injected services (e.g., `DeviceService`, `TenantService`), configuration flags (e.g., `logControllerErrorStackTrace`).
- **Logic Flow:**
  1. Controller method throws exception.
  2. Exception handler maps it to a `ThingsboardException`.
  3. Error is logged and a standardized response is sent.

## Complex Concepts
- **Dependency Injection:** All services are injected via Spring's `@Autowired`.
- **Asynchronous Handling:** Some endpoints use async request processing for scalability.

## Best Practices
- Use `BaseController` for consistent error handling.
- Validate all input in controllers.
- Keep business logic in services, not controllers.

## Common Pitfalls
- Not handling all exception types can lead to generic error responses.
- Over-injecting services can make controllers hard to test.

## Recommendations
- Refactor large controllers into smaller, focused ones.
- Increase test coverage for error scenarios.

---
See also: [thingsboard-architecture.spec.md](thingsboard-architecture.spec.md)
