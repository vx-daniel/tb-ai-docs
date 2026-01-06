# ThingsBoard Rule Engine API: RuleEngineDeviceRpcResponse

## Purpose
Represents a response to a device RPC (Remote Procedure Call) request in the rule engine context.

## Key Responsibilities
- Encapsulates response data, status, and error information.
- Used by nodes/services to process device RPC results.

## Usage Pattern
- Received from RPC services or nodes after execution.

## Best Practices
- Handle all possible response statuses and errors.

## Pitfalls
- Ignoring errors can cause missed device state changes.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
