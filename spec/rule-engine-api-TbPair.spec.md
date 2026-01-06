# ThingsBoard Rule Engine API: TbPair

## Purpose
Generic utility class for holding a pair of values, commonly used for returning two related results from a method.

## Key Responsibilities
- Encapsulates two values (left, right) of potentially different types.
- Used in API methods (e.g., configuration upgrade) to return a result and associated data.

## Usage Pattern
- Returned from methods that need to provide a status and a value (e.g., `upgrade()` in `TbNode`).

## Best Practices
- Use for simple pair returns instead of creating custom classes.

## Pitfalls
- Overuse can reduce code clarity if not documented.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
