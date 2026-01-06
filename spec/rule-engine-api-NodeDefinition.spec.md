# ThingsBoard Rule Engine API: NodeDefinition

## Purpose
Describes the definition and metadata for a rule engine node, used for discovery and configuration in the platform.

## Key Responsibilities
- Holds node type, name, description, config class, and other metadata.
- Used by the platform to generate UI and documentation for nodes.

## Usage Pattern
- Instantiated and registered for each node type.

## Best Practices
- Keep metadata up to date for accurate UI representation.

## Pitfalls
- Outdated definitions can cause confusion or misconfiguration.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
