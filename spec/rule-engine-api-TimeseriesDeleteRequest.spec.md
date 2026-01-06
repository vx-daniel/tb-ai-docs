# ThingsBoard Rule Engine API: TimeseriesDeleteRequest

## Purpose
Represents a request to delete timeseries data for an entity in the rule engine context.

## Key Responsibilities
- Encapsulates entity ID, keys, and time range for deletion.
- Used by timeseries-related nodes and services.

## Usage Pattern
- Constructed and passed to telemetry service methods for deletion.

## Best Practices
- Validate entity, keys, and time range before deletion.

## Pitfalls
- Deleting the wrong time range can cause data loss.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
