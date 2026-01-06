# ThingsBoard Rule Engine API: RuleEngineTelemetryService

## Purpose
Manages telemetry data operations (save, update, delete) for entities in the rule engine context.

## Key Responsibilities
- Provides APIs for telemetry lifecycle management.
- Used by telemetry-related nodes and services.

## Usage Pattern
- Called by nodes to save, update, or delete telemetry data based on message content.

## Best Practices
- Use for all telemetry operations to ensure consistency and performance.

## Pitfalls
- Not handling telemetry errors can cause data loss or inconsistency.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
