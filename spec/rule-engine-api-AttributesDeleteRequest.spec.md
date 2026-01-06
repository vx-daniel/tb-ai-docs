# ThingsBoard Rule Engine API: AttributesDeleteRequest

## Purpose
Represents a request to delete attributes from an entity in the rule engine context.

## Key Responsibilities
- Encapsulates entity ID and attribute keys to be deleted.
- Used by attribute-related nodes and services.

## Usage Pattern
- Constructed and passed to attribute service methods for deletion.

## Best Practices
- Validate entity and attribute keys before deletion.

## Pitfalls
- Deleting non-existent attributes has no effect but may indicate logic errors.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
