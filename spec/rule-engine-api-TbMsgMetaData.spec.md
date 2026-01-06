# ThingsBoard Rule Engine API: TbMsgMetaData

## Purpose
Encapsulates metadata (key-value pairs) for a `TbMsg`, used for routing, filtering, and enrichment in the rule engine.

## Key Responsibilities
- Stores and retrieves metadata associated with a message.
- Used by nodes to make routing and processing decisions.

## Usage Pattern
- Accessed via `TbMsg.getMetaData()` and manipulated in node logic.

## Best Practices
- Use metadata for lightweight, frequently accessed properties.
- Avoid storing large data in metadata.

## Pitfalls
- Overloading metadata with too much data can impact performance.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
