# ThingsBoard Rule Engine API: TbMsg

## Purpose
Represents a message flowing through the rule engine. Encapsulates data, metadata, type, and originator information for each event or command.

## Key Responsibilities
- Holds message payload (data), metadata (key-value pairs), type, and originator entity.
- Used as the primary unit of processing in all rule nodes.

## Usage Pattern
- Created and transformed using `TbContext` methods (`newMsg`, `transformMsg`).
- Passed to each node's `onMsg` method for processing.

## Best Practices
- Use metadata for routing and filtering logic.
- Keep payloads concise for performance.

## Pitfalls
- Large or complex payloads can impact performance.
- Not updating metadata can cause routing errors.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
