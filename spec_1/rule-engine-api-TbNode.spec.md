# ThingsBoard Rule Engine API: TbNode

## Purpose
Defines the contract for all rule engine nodes. Every node must implement this interface to participate in the rule engine pipeline.

## Key Methods
- `init(TbContext ctx, TbNodeConfiguration configuration)`: Initialize the node with context and configuration.
- `onMsg(TbContext ctx, TbMsg msg)`: Handle an incoming message. Must call `tellSuccess`, `tellNext`, or `tellFailure` on the context.
- `destroy()`: Cleanup resources (optional override).
- `onPartitionChangeMsg(...)`: Handle partition changes (optional override).
- `upgrade(...)`: Upgrade node configuration between versions (optional override).

## Usage Pattern
- Implement this interface for each custom rule node.
- Register node with `@RuleNode` annotation for metadata and configuration.

## Best Practices
- Always acknowledge messages (success/failure) to avoid blocking the rule chain.
- Keep node logic stateless and focused.

## Pitfalls
- Not handling all message outcomes can block processing.
- Resource leaks if `destroy()` is not implemented for nodes using external resources.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
