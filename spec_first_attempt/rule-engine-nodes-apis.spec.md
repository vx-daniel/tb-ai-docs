# ThingsBoard Rule Engine: Node and API Overview

## Node API: `TbNode`
- **Interface for all rule engine nodes.**
- **Key Methods:**
  - `init(TbContext ctx, TbNodeConfiguration configuration)`: Initialize node with context and config.
  - `onMsg(TbContext ctx, TbMsg msg)`: Handle incoming message.
  - `destroy()`: Cleanup resources (optional override).
  - `onPartitionChangeMsg(...)`: Handle partition changes (optional override).
  - `upgrade(...)`: Upgrade node configuration between versions (optional override).

## Node Context: `TbContext`
- **Provides access to message flow control, entity services, and utility methods.**
- **Key Methods:**
  - `tellSuccess`, `tellNext`, `tellFailure`: Control message routing and error handling.
  - `enqueue`, `input`, `output`: Advanced message routing and queueing.
  - `newMsg`, `transformMsg`: Create and transform messages.
  - Provides access to all core services (DeviceService, AlarmService, etc.).

## Node Types (Examples)
- **Action Nodes:** Perform actions (e.g., log, send notification, update entity).
- **Filter Nodes:** Route messages based on conditions (e.g., type, relation, JS filter).
- **Telemetry Nodes:** Save or process telemetry and attributes.
- **Transformation Nodes:** Modify message data or metadata.
- **External Integration Nodes:** Interact with external systems (e.g., REST, MQTT, Kafka, SMS, Mail).
- **AI/ML Nodes:** Integrate with AI models or services.

## Node Implementation Pattern
- Extend/implement `TbNode`.
- Use `TbContext` for all interactions (message flow, entity access, logging).
- Register node with `@RuleNode` annotation for metadata and configuration.

## Best Practices
- Always handle both success and failure in async flows.
- Use context-provided services for all entity operations.
- Keep node logic focused and stateless where possible.

## Common Pitfalls
- Not acknowledging messages (success/failure) can block rule chains.
- Resource leaks if `destroy()` is not implemented for nodes using external resources.

---
See also: [rule-engine.spec.md](rule-engine.spec.md)
