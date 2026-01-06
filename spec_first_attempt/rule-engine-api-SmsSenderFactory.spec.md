# ThingsBoard Rule Engine API: SmsSenderFactory

## Purpose
Factory for creating SmsSender instances based on configuration or provider type.

## Key Responsibilities
- Instantiates appropriate SmsSender implementations.
- Used by SmsService and notification nodes.

## Usage Pattern
- Called to obtain an SmsSender for a given provider or configuration.

## Best Practices
- Register all supported providers with the factory.

## Pitfalls
- Missing provider registration can cause runtime errors.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
