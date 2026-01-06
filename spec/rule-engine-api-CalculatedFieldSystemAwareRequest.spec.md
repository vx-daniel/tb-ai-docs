# ThingsBoard Rule Engine API: CalculatedFieldSystemAwareRequest

## Purpose
Represents a request for calculated fields that are aware of system context, used in advanced rule engine scenarios.

## Key Responsibilities
- Encapsulates parameters for calculated field evaluation.
- Used by nodes/services that compute derived values based on system state.

## Usage Pattern
- Constructed and passed to calculated field services or nodes.

## Best Practices
- Ensure all required context is provided for accurate calculations.

## Pitfalls
- Missing context can lead to incorrect calculations.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
