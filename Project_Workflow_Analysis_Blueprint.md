# Project Workflow Analysis Blueprint

*Generated: 2026-01-06*

---

## Initial Detection Phase

- **Primary Technology Stack:** Java (Spring Boot, Akka), Angular (UI), Python (scripts/labs)
- **Entry Points:** REST API controllers, MQTT/HTTP transport handlers, Angular UI components
- **Persistence:** SQL Database (PostgreSQL, via JPA/Hibernate), in-memory caches, message queues
- **Architecture Pattern:** Layered + Event-Driven (actor-based), Clean Architecture influences

---

## Workflow 1: Device Telemetry Upload (End-to-End)

### 1. Workflow Overview

- **Name:** Device Telemetry Upload
- **Description:** Device sends telemetry data (e.g., temperature, humidity) to the platform, which is processed, validated, persisted, and triggers rule engine logic.
- **Business Purpose:** Ingest and process real-time device data for monitoring, automation, and analytics.
- **Trigger:** Device publishes telemetry via MQTT/HTTP POST
- **Files/Classes Involved:**
  - `MqttTransportHandler`, `HttpTransportController`
  - `TbMsg`, `TbMsgMetaData`
  - `DefaultTransportService`, `ActorSystem`
  - `RuleEngineActor`, `RuleNode`
  - `TelemetryService`, `TelemetryRepository`
  - `Device`, `TelemetryEntity`

### 2. Entry Point Implementation

- **Transport Handler:**
  - `MqttTransportHandler.handlePostTelemetry()`
  - `HttpTransportController.postTelemetry()`
- **Method Signature (Java):**

  ```java
  @PostMapping("/api/v1/{deviceToken}/telemetry")
  public ResponseEntity<?> postTelemetry(@PathVariable String deviceToken, @RequestBody JsonNode payload)
  ```

- **DTO/Model:**
  - `JsonNode` payload (dynamic JSON)
- **Validation:**
  - Device token validation, JSON schema checks
- **Auth:**
  - Device credentials checked before processing

### 3. Service Layer Implementation

- **Service:** `DefaultTransportService`
- **Method:** `processTelemetry(TbMsg msg)`
- **Dependencies:** Actor system, device service, telemetry service
- **Business Logic:**
  - Wraps payload in `TbMsg`, enriches metadata, routes to actor system
- **DI Pattern:** Spring @Autowired constructor injection

### 4. Data Mapping Patterns

- **Mapping:**
  - JSON payload → `TbMsg.data`
  - Metadata enrichment (device info, timestamps)
- **Validation:**
  - Schema validation, required fields

### 5. Data Access Implementation

- **Repository:** `TelemetryRepository`
- **Method:** `save(TelemetryEntity entity)`
- **Entity:** `TelemetryEntity` (fields: deviceId, ts, key, value)
- **ORM:** JPA annotations, Hibernate
- **Transaction:** Spring @Transactional

### 6. Response Construction

- **Response DTO:**
  - Success: HTTP 200/201, empty or status message
  - Error: HTTP 400/401/500, error message JSON
- **Mapping:**
  - Exception → error response

### 7. Error Handling Patterns

- **Exceptions:**
  - `DeviceNotFoundException`, `InvalidPayloadException`, generic `Exception`
- **Try/Catch:**
  - At controller and service layers
- **Global Handler:**
  - `@ControllerAdvice` for API errors
- **Logging:**
  - SLF4J logger, error details
- **Retry:**
  - At transport/actor level for transient errors

### 8. Asynchronous Processing Patterns

- **Actor System:**
  - Telemetry message routed asynchronously to rule engine
- **Event Publication:**
  - Rule engine nodes may emit events, trigger alarms
- **Queue:**
  - Internal message queues for backpressure
- **Callback:**
  - Success/failure callback to transport handler

### 9. Testing Approach

- **Unit Tests:**
  - Controller, service, repository layers (JUnit, Mockito)
- **Integration Tests:**
  - Embedded DB, mock transport
- **Test Data:**
  - Builders for device, telemetry
- **API Tests:**
  - REST-assured, Postman collections

### 10. Sequence Diagram

```mermaid
sequenceDiagram
  participant Device
  participant Transport as MQTT/HTTP Handler
  participant Service as DefaultTransportService
  participant Actor as ActorSystem
  participant Rule as RuleEngine
  participant Repo as TelemetryRepository
  Device->>Transport: POST telemetry
  Transport->>Service: processTelemetry(payload)
  Service->>Actor: send TbMsg
  Actor->>Rule: process TbMsg
  Rule->>Repo: save TelemetryEntity
  Repo-->>Rule: ack
  Rule-->>Actor: processing result
  Actor-->>Service: callback
  Service-->>Transport: response
  Transport-->>Device: HTTP 200/201
```

### 11. Naming Conventions

- Controller: `TransportController`, `DeviceController`
- Service: `TransportService`, `TelemetryService`
- Repository: `TelemetryRepository`
- DTO: `TelemetryRequest`, `TelemetryResponse`
- Entity: `TelemetryEntity`
- Method: `save`, `processTelemetry`, `handlePostTelemetry`
- Variables: camelCase
- Files: by feature/module

### 12. Implementation Templates

- **API Endpoint:**

  ```java
  @PostMapping("/api/v1/{deviceToken}/telemetry")
  public ResponseEntity<?> postTelemetry(@PathVariable String deviceToken, @RequestBody JsonNode payload) { ... }
  ```

- **Service Method:**

  ```java
  public void processTelemetry(TbMsg msg) { ... }
  ```

- **Repository Method:**

  ```java
  public void save(TelemetryEntity entity) { ... }
  ```

- **Entity:**

  ```java
  @Entity
  public class TelemetryEntity { ... }
  ```

- **Error Handling:**

  ```java
  @ControllerAdvice
  public class ApiExceptionHandler { ... }
  ```

---

## Implementation Guidelines

### Step-by-Step Process

1. Define API/controller method for new telemetry type
2. Implement service logic to wrap and route message
3. Map payload to entity, validate, and persist
4. Add async routing via actor system if needed
5. Implement error handling and logging
6. Add unit/integration tests

### Common Pitfalls

- Forgetting to validate device credentials
- Not copying metadata for async branches
- Missing transaction boundaries in repository
- Unhandled exceptions leaking to client

### Extension Mechanisms

- Add new transport adapters via interface
- Add new rule nodes via plugin API
- Use configuration for feature toggles

---

**Conclusion:**
Maintain strict layering, use async actor boundaries, validate inputs, and follow naming/templates for consistency. Extend via interfaces and plugins, not by modifying core logic.
