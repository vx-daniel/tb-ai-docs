# ThingsBoard Rule Engine API: NotificationCenter

## Purpose
Central hub for managing notifications within the rule engine context.

## Key Responsibilities
- Registers, dispatches, and tracks notifications for nodes and services.
- Integrates with notification channels (email, SMS, Slack, etc.).

## Usage Pattern
- Used by nodes/services to send or receive notifications.

## Best Practices
- Use for all notification flows to ensure consistency and tracking.

## Pitfalls
- Not registering notifications can cause missed alerts.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
