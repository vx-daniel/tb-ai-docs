# ThingsBoard Rule Engine API: AttributesSaveRequest

## Purpose
Represents a request to save or update attributes for an entity in the rule engine context.

## Key Responsibilities
- Encapsulates entity ID, attribute scope, and key-value pairs to be saved.
- Used by attribute-related nodes and services.

## Usage Pattern
- Constructed and passed to attribute service methods for saving/updating.

## Best Practices
- Validate entity, scope, and attribute data before saving.

## Pitfalls
- Overwriting important attributes without checks can cause data loss.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
