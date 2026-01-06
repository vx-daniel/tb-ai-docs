# ThingsBoard Rule Engine API: TbNodeConfiguration

## Purpose
Encapsulates configuration for a rule engine node. Used to pass user-defined or default settings to each node instance.

## Key Responsibilities
- Holds configuration data (as JSON or POJOs) for node initialization.
- Used by `TbNode.init()` to configure node behavior.

## Usage Pattern
- Each node defines a configuration class (e.g., `TbLogNodeConfiguration`).
- Configuration is deserialized and passed to the node during initialization.

## Best Practices
- Validate configuration in `init()` and provide defaults for missing values.
- Use versioning and the `upgrade()` method in `TbNode` for backward compatibility.

## Pitfalls
- Invalid or missing configuration can cause node failures.
- Not handling config upgrades can break rule chains after updates.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
