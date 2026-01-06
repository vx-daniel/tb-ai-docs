# ThingsBoard Rule Engine API: TbNodeException

## Purpose
Custom exception for rule engine node errors. Used to signal initialization, configuration, or runtime issues in node logic.

## Key Responsibilities
- Encapsulates error details for node failures.
- Used in `init`, `onMsg`, and `upgrade` methods to indicate problems.

## Usage Pattern
- Throw from node methods when configuration or processing fails.
- Catch in rule engine to handle node-specific errors gracefully.

## Best Practices
- Provide clear error messages for easier debugging.
- Use specific exception types for different error scenarios.

## Pitfalls
- Swallowing exceptions can hide node failures.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
