# ThingsBoard Rule Engine API: EmptyNodeConfiguration

## Purpose
Represents a default or empty configuration for nodes that do not require user-defined settings.

## Key Responsibilities
- Used as a placeholder for nodes with no configuration.
- Ensures consistent API for all nodes.

## Usage Pattern
- Passed to node `init()` methods when no config is needed.

## Best Practices
- Use for stateless or fixed-behavior nodes.

## Pitfalls
- Modifying empty config can break node assumptions.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
