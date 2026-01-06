# ThingsBoard Rule Engine API: RuleEngineApiUsageStateService

## Purpose
Tracks and manages API usage state for rule engine nodes, supporting rate limiting and quota enforcement.

## Key Responsibilities
- Monitors API usage and enforces limits.
- Used by nodes/services that need to respect usage quotas.

## Usage Pattern
- Called by nodes before making API calls to check limits.

## Best Practices
- Always check usage state before external API calls.

## Pitfalls
- Ignoring limits can cause service disruptions or extra costs.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
