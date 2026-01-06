# ThingsBoard Rule Engine API: RuleEngineAlarmService

## Purpose
Manages alarm creation, updates, and clearing within the rule engine.

## Key Responsibilities
- Provides APIs for alarm lifecycle management.
- Used by alarm-related nodes and services.

## Usage Pattern
- Called by nodes to create, update, or clear alarms based on message content.

## Best Practices
- Use for all alarm operations to ensure consistency.

## Pitfalls
- Not clearing alarms can cause persistent false alerts.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
