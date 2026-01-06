# ThingsBoard Rule Engine API: TimeseriesSaveRequest

## Purpose
Represents a request to save or update timeseries data for an entity in the rule engine context.

## Key Responsibilities
- Encapsulates entity ID, key-value pairs, and timestamps for saving.
- Used by timeseries-related nodes and services.

## Usage Pattern
- Constructed and passed to telemetry service methods for saving/updating.

## Best Practices
- Validate entity, keys, and timestamps before saving.

## Pitfalls
- Incorrect timestamps can cause data misalignment.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
