# ThingsBoard Codebase: High-Level Architecture and Best Practices

## Language and Context
- **Backend:** Java (Spring, Maven, Protobuf)
- **Frontend:** Angular (TypeScript, RxJS)
- **Domain:** IoT platform for device management, data processing, and visualization

## Main Modules
- **application/**: Core server logic, REST controllers, business services
- **common/**: Shared utilities, data models, transport protocols
- **dao/**: Data access layer (SQL/NoSQL)
- **docker/**: Deployment scripts and environment configs
- **rule-engine/**: Event-driven processing, rule nodes, async message queues
- **transport/**: Device communication (MQTT, HTTP, CoAP, LwM2M, SNMP)
- **ui-ngx/**: Angular web UI, widgets, dashboards

## Key Patterns and Concepts
- **Dependency Injection:** Spring (Java) and Angular (TypeScript)
- **Asynchronous Programming:** ListenableFuture, FutureCallback (Java); Observable, Subject (RxJS)
- **Centralized Error Handling:** BaseController for REST, global error handlers in UI
- **Data Structures:** Maps, Queues, Caches for entity management and message passing
- **Design Patterns:** Observer, Strategy, Factory, Singleton, Modularization

## Best Practices
- Centralize error handling and logging
- Use interface-driven design for services
- Validate all inputs and outputs
- Modularize code for maintainability
- Use RxJS operators for memory management in UI
- Document architecture and extension points

## Common Pitfalls
- Silent failures in async flows
- Stale cache data
- Overly complex observable chains
- Insufficient test coverage for edge cases

## Recommendations
- Improve cache invalidation strategies
- Monitor queue backpressure in rule engine
- Increase test coverage for async and error flows
- Enhance documentation with OpenSpec files

---
This spec provides a high-level overview and actionable best practices for developers working with the ThingsBoard codebase. For detailed module specs, see additional files in this folder.
