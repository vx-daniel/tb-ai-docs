# ThingsBoard Rule Engine API: TbNodeUtils

## Purpose
Utility class for common node operations, such as configuration conversion and validation.

## Key Responsibilities
- Converts generic node configuration to specific configuration classes.
- Provides helper methods for node initialization and validation.

## Usage Pattern
- Used in node `init()` methods to convert `TbNodeConfiguration` to a concrete config class.

## Best Practices
- Use for all configuration conversions to ensure consistency.
- Validate configuration after conversion.

## Pitfalls
- Incorrect conversion can cause runtime errors in node logic.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
