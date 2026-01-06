# ThingsBoard Specification Documentation

Comprehensive technical specifications for the ThingsBoard IoT platform, organized by domain. These specifications are designed for developers, architects, and AI assistants working with the ThingsBoard codebase.

---

## Architecture & Design

High-level architectural patterns, design principles, and system blueprints.

| Document | Description |
|----------|-------------|
| [Architecture Blueprint](architecture-blueprint.md) | System architecture, module structure, patterns, and cross-cutting concerns |
| [Infrastructure & Queue](infrastructure-queue.md) | Queue backends, partitioning, technology stack, and high availability |
| [Testing & CI/CD](testing-cicd.md) | Test strategies, frameworks, pipelines, and quality gates |

---

## Rule Engine

Core rule engine interfaces, messaging, and node implementation.

| Document | Description |
|----------|-------------|
| [Rule Engine Core](rule-engine-core.md) | TbNode, TbContext, TbMsg, TbMsgMetaData interfaces and lifecycle |
| [Rule Node Implementation Guide](rule-node-implementation-guide.md) | How to implement custom rule nodes with examples |
| [Script Engine](script-engine.md) | TBEL and JavaScript scripting APIs, built-in functions, and examples |

---

## Transport Layer

Device communication protocols and transport-to-rule-engine flow.

| Document | Description |
|----------|-------------|
| [Transport Layer](transport-layer.md) | MQTT, HTTP, CoAP protocols, session management, and rate limiting |

---

## Device & Asset Management

Device lifecycle, profiles, RPC, and state management.

| Document | Description |
|----------|-------------|
| [Device & Asset Management](device-asset-management.md) | Profiles, lifecycle, RPC, credentials, provisioning, and relations |

---

## Notifications & Alarms

Alarm lifecycle, notification channels, and delivery.

| Document | Description |
|----------|-------------|
| [Alarm & Notification Services](alarm-notification-services.md) | Alarm rules, propagation, notification templates, and channels |

---

## Security & Multi-Tenancy

Authentication, authorization, and tenant isolation.

| Document | Description |
|----------|-------------|
| [Security & Authentication](security-auth.md) | JWT, OAuth2, MFA, device auth, RBAC, and rate limiting |

---

## UI & Real-Time

Frontend patterns and real-time data delivery.

| Document | Description |
|----------|-------------|
| [UI & Real-Time](ui-realtime.md) | Dashboards, widgets, WebSocket subscriptions, and real-time updates |

---

## Document Summary

| File | Topics Covered |
|------|----------------|
| [architecture-blueprint.md](architecture-blueprint.md) | Modules, layers, patterns, extension points, deployment |
| [rule-engine-core.md](rule-engine-core.md) | TbNode, TbContext, TbMsg, message types, metadata, services |
| [rule-node-implementation-guide.md](rule-node-implementation-guide.md) | Node lifecycle, configuration, routing, async, testing |
| [script-engine.md](script-engine.md) | TBEL, JavaScript, filter/transform/switch scripts |
| [transport-layer.md](transport-layer.md) | MQTT, HTTP, CoAP, session management, authentication |
| [device-asset-management.md](device-asset-management.md) | Profiles, RPC, state, credentials, provisioning |
| [alarm-notification-services.md](alarm-notification-services.md) | Alarms, notifications, templates, channels |
| [security-auth.md](security-auth.md) | JWT, OAuth2, MFA, device auth, permissions |
| [ui-realtime.md](ui-realtime.md) | Dashboards, widgets, WebSocket subscriptions |
| [infrastructure-queue.md](infrastructure-queue.md) | Kafka, partitioning, DLQ, HA, monitoring |
| [testing-cicd.md](testing-cicd.md) | Unit/integration/E2E tests, CI/CD, quality gates |

---

## Source Specification Directories

These merged specifications consolidate content from:

- `spec_1/` - Rule Engine API interface specifications
- `spec_2/` - Architecture, design, process, and infrastructure specifications
- `spec_3/` - Domain-specific specifications (transport, device, notification, security)
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
