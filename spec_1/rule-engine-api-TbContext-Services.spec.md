# ThingsBoard Rule Engine API: TbContext Service Access

## Purpose
Provides access to all core ThingsBoard services (DeviceService, AlarmService, AssetService, etc.) from within rule nodes.

## Key Responsibilities
- Enables nodes to perform CRUD and query operations on all major entity types.
- Abstracts service access behind the context for consistency and testability.

## Usage Pattern
- Accessed via `TbContext` methods and properties in node logic.

## Best Practices
- Use context for all entity operations to ensure consistency.
- Avoid direct service injection in nodes.

## Pitfalls
- Bypassing context can cause data consistency issues.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
