# ThingsBoard Rule Engine API: RuleEngineDeviceProfileCache

## Purpose
Caches device profile data for efficient access by rule engine nodes and services.

## Key Responsibilities
- Stores and retrieves device profile information.
- Reduces database load and improves performance.

## Usage Pattern
- Accessed by nodes/services that need device profile data.

## Best Practices
- Invalidate cache on profile updates to ensure consistency.

## Pitfalls
- Stale cache data can cause incorrect rule logic.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
