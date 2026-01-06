# ThingsBoard Rule Engine API: RuleEngineRpcService

## Purpose
Provides RPC (Remote Procedure Call) capabilities for rule engine nodes, enabling device and server-side RPC interactions.

## Key Responsibilities
- Sends and receives RPC requests and responses.
- Integrates with device and server-side RPC mechanisms.

## Usage Pattern
- Used by nodes that need to invoke or handle RPC calls (e.g., device control, remote commands).

## Best Practices
- Handle timeouts and errors in RPC flows.
- Validate RPC payloads and permissions.

## Pitfalls
- Unhandled RPC errors can block rule chains or cause inconsistent device state.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
