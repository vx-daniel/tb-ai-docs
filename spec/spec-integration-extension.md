---
title: Integration & Extension Mechanisms
version: 1.0
date_created: 2026-01-06
last_updated: 2026-01-06
owner: ThingsBoard Architecture Team
tags: [architecture, integration, extension, api, plugin, microservices]
---

# Introduction

This specification details technical patterns for integrating and extending ThingsBoard, including external system integration, plugin development, and microservice communication.

## 1. Purpose & Scope

Defines how to safely extend and integrate with ThingsBoard. Intended for developers building plugins, integrations, or microservices.

## 2. Definitions

- **Integration**: Connecting external systems/services
- **Extension**: Adding new features via plugins or modules
- **API**: Application Programming Interface
- **Microservice**: Independently deployable service
- **Adapter/Facade**: Pattern for external system integration

## 3. Requirements, Constraints & Guidelines

- **REQ-001**: All integrations must use documented APIs or extension points
- **REQ-002**: Plugins must be registered and versioned
- **REQ-003**: Microservices must communicate via REST/gRPC
- **CON-001**: No direct DB access from plugins/integrations
- **GUD-001**: Use anti-corruption layer for legacy integration
- **PAT-001**: Use Adapter/Facade pattern for external APIs

## 4. Interfaces & Data Contracts

Example REST integration:
```java
@RestController
@RequestMapping("/api/external")
public class ExternalIntegrationController {
    @PostMapping("/sync")
    public ResponseEntity<?> sync(@RequestBody SyncRequest req) { ... }
}
```

Example plugin registration:
```java
@Component
public class MyPlugin implements ThingsBoardPlugin { ... }
```

## 5. Acceptance Criteria

- **AC-001**: All integrations use documented APIs
- **AC-002**: All plugins are registered and versioned
- **AC-003**: All microservices use REST/gRPC for communication

## 6. Test Automation Strategy

- **Unit tests**: JUnit for plugin/integration logic
- **Integration tests**: Simulated external system calls
- **Coverage**: 90%+ for integration code

## 7. Rationale & Context

Safe, versioned integration and extension mechanisms enable rapid feature addition and third-party connectivity.

## 8. Dependencies & External Integrations

- **INF-001**: REST/gRPC APIs
- **SVC-001**: External systems (SMTP, SMS, Slack, AI/ML)

## 9. Examples & Edge Cases

```java
// Edge case: Breaking change in external API
public void sync() {
    // Use adapter to shield core logic from API changes
}
```

## 10. Validation Criteria

- All integrations and plugins pass unit and integration tests
- All API versioning paths are covered

## 11. Related Specifications / Further Reading

- [spec-architecture-blueprint.md](spec-architecture-blueprint.md)
---
