# ThingsBoard Rule Engine API: TbNodeState

## Purpose
Represents the state of a rule engine node, used for tracking and managing node lifecycle and status.

## Key Responsibilities
- Holds state information for nodes (e.g., active, suspended, error).
- Used by the rule engine to manage node execution and recovery.

## Usage Pattern
- Accessed and updated by the rule engine during node lifecycle events.

## Best Practices
- Use state transitions to manage node health and recovery.

## Pitfalls
- Not updating state can cause inconsistent node behavior.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
