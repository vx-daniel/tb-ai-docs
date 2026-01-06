---
title: Backend Service Implementation Patterns
version: 1.0
date_created: 2026-01-06
last_updated: 2026-01-06
owner: ThingsBoard Architecture Team
tags: [architecture, backend, service, implementation, java, spring]
---

# Introduction

This specification details the technical implementation patterns for backend services in ThingsBoard, focusing on Java/Spring modules. It provides explicit requirements, interface contracts, and best practices for maintainable, scalable, and testable service code.

## 1. Purpose & Scope

Defines how backend services should be structured, implemented, and integrated. Intended for backend developers and architects.

## 2. Definitions

- **Service**: Business logic provider, typically a Spring @Service bean
- **DAO**: Data Access Object
- **DI**: Dependency Injection
- **DTO**: Data Transfer Object
- **Entity**: Persistent domain object

## 3. Requirements, Constraints & Guidelines

- **REQ-001**: All services must be defined as interfaces and implemented as @Service beans
- **REQ-002**: Services must not depend directly on controllers or UI code
- **REQ-003**: Use constructor injection for all dependencies
- **REQ-004**: Service methods must validate input and handle errors
- **CON-001**: Services must be stateless; use transactional boundaries for state changes
- **GUD-001**: Use DTOs for external API boundaries
- **PAT-001**: Use Singleton pattern for service beans

## 4. Interfaces & Data Contracts

Example service interface:
```java
public interface DeviceService {
    DeviceDTO getDeviceById(UUID id);
    List<DeviceDTO> listDevices(PageLink pageLink);
    DeviceDTO createDevice(DeviceDTO device);
    void deleteDevice(UUID id);
}
```

Example implementation:
```java
@Service
public class DeviceServiceImpl implements DeviceService {
    private final DeviceDao deviceDao;
    public DeviceServiceImpl(DeviceDao deviceDao) {
        this.deviceDao = deviceDao;
    }
    // ...method implementations...
}
```

## 5. Acceptance Criteria

- **AC-001**: All service beans are stateless and injectable
- **AC-002**: All public methods validate input and handle exceptions
- **AC-003**: All DAOs are injected via constructor

## 6. Test Automation Strategy

- **Unit tests**: JUnit, Mockito for service logic
- **Integration tests**: Spring Test with in-memory DB
- **Coverage**: 90%+ for service methods

## 7. Rationale & Context

Stateless, interface-driven services enable testability, maintainability, and clear separation of concerns.

## 8. Dependencies & External Integrations

- **INF-001**: Spring Framework (DI, Transactional)
- **DAT-001**: DAOs for persistence

## 9. Examples & Edge Cases

```java
// Edge case: Null input
public DeviceDTO getDeviceById(UUID id) {
    if (id == null) throw new IllegalArgumentException("Device ID required");
    // ...
}
```

## 10. Validation Criteria

- All service beans pass unit and integration tests
- All input validation paths are covered

## 11. Related Specifications / Further Reading

- [spec-architecture-blueprint.md](spec-architecture-blueprint.md)
- [application.spec.md](application.spec.md)
---
