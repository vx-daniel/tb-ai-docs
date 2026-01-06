# ThingsBoard Rule Engine API: DeviceStateManager

## Purpose
Manages device state transitions and status within the rule engine.

## Key Responsibilities
- Tracks and updates device state (active, inactive, etc.).
- Provides APIs for querying and modifying device state.

## Usage Pattern
- Used by device-related nodes and services to manage device lifecycle.

## Best Practices
- Use for all device state changes to ensure consistency.

## Pitfalls
- Not updating state can cause inaccurate device status reporting.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
