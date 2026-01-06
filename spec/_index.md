# ThingsBoard Specification Documentation

Comprehensive technical specifications for the ThingsBoard IoT platform, organized by domain. These specifications are designed for developers, architects, and AI assistants working with the ThingsBoard codebase.

---

## Architecture & Design

High-level architectural patterns, design principles, and system blueprints.

- [Architecture Blueprint](architecture-blueprint.md) - System architecture, patterns, and design principles
- [Backend Service Design](backend-service-design.md) - Service domains, responsibilities, and patterns
- [Technology Stack](technology-stack.md) - Languages, frameworks, and infrastructure dependencies

---

## Rule Engine

Core rule engine interfaces, messaging, and node implementation.

- [Rule Engine Core](rule-engine-core.md) - TbNode, TbContext, TbMsg interfaces and lifecycle
- [Rule Node Implementation Guide](rule-node-implementation-guide.md) - How to implement custom rule nodes
- [Rule Node Inventory](rule-node-inventory.md) - Complete catalog of available rule nodes
- [Rule Chain Templates](rule-chain-templates.md) - JSON templates and patterns for rule chains
- [Script Engine](script-engine.md) - TBEL and JavaScript scripting APIs

---

## Transport Layer

Device communication protocols and transport-to-rule-engine flow.

- [Transport Overview](transport-overview.md) - Transport layer architecture and common patterns
- [MQTT Transport](mqtt-transport.md) - MQTT protocol implementation and topics
- [HTTP Transport](http-transport.md) - HTTP REST API for devices
- [CoAP Transport](coap-transport.md) - CoAP protocol for constrained devices

---

## Device & Asset Management

Device lifecycle, profiles, RPC, and state management.

- [Device & Asset Profiles](device-asset-profiles.md) - Profile configuration and provisioning
- [Device RPC](device-rpc.md) - Server-side and client-side RPC implementation
- [Device State Management](device-state-management.md) - Connectivity and activity tracking
- [OTA Updates](ota-updates.md) - Firmware and software update mechanisms
- [Timeseries & Attributes](timeseries-attributes.md) - Data storage and retrieval APIs

---

## Data & Persistence

Data access layer, entity services, and caching.

- [DAO & Entity Services](dao-entity-services.md) - Data access patterns and entity management
- [Entity Query API](entity-query.md) - Flexible entity querying and filtering
- [Caching Strategies](caching-strategies.md) - Entity and profile caching

---

## Notifications & Messaging

Notification channels, templates, and delivery.

- [Notification Service](notification-service.md) - Notification center and delivery channels
- [Mail Service](mail-service.md) - Email sending and templates
- [SMS Service](sms-service.md) - SMS providers and configuration
- [Alarm Service](alarm-service.md) - Alarm creation, propagation, and querying

---

## Security & Multi-Tenancy

Authentication, authorization, and tenant isolation.

- [Security & Authentication](security-auth.md) - JWT, OAuth2, MFA, and device auth
- [Tenant & Customer Model](tenant-customer-model.md) - Multi-tenancy and customer hierarchy
- [API Usage & Rate Limiting](api-usage-state.md) - Usage tracking and rate limits
- [Audit Logging](audit-logging.md) - Action tracking and compliance

---

## UI & Real-Time

Frontend patterns and real-time data delivery.

- [Angular UI Patterns](angular-ui-patterns.md) - Component, service, and state management patterns
- [Dashboard & Widgets](dashboard-widgets.md) - Widget development and data binding
- [WebSocket Subscriptions](websocket-subscriptions.md) - Real-time data subscriptions

---

## Infrastructure & Operations

Actors, queues, monitoring, and deployment.

- [Actor System](actor-system.md) - Akka actors, mailboxes, and processing
- [Queue & Partitioning](queue-partitioning.md) - Message queues and partition strategies
- [Job Manager](job-manager.md) - Scheduled and recurring jobs
- [Edge Integration](edge-integration.md) - ThingsBoard Edge synchronization
- [Monitoring & Observability](monitoring-observability.md) - Logging, metrics, and tracing

---

## Testing & Quality

Testing strategies, CI/CD, and validation.

- [Testing Strategy](testing-strategy.md) - Unit, integration, and E2E testing
- [Error Handling](error-handling.md) - Exception patterns and transaction management
- [Compliance & Auditing](compliance-auditing.md) - Regulatory requirements and audit trails

---

## Quick Reference

| Category | Key Specs |
|----------|-----------|
| Getting Started | [Architecture Blueprint](architecture-blueprint.md), [Rule Engine Core](rule-engine-core.md) |
| Building Nodes | [Rule Node Implementation Guide](rule-node-implementation-guide.md) |
| Device Integration | [MQTT Transport](mqtt-transport.md), [Device RPC](device-rpc.md) |
| Data Access | [DAO & Entity Services](dao-entity-services.md), [Entity Query API](entity-query.md) |
| Security | [Security & Authentication](security-auth.md), [Tenant & Customer Model](tenant-customer-model.md) |

---

## Labs (Hands-on Exercises)

For practical, step-by-step exercises, see:

- [Labs Index](../labs/_index.md)
