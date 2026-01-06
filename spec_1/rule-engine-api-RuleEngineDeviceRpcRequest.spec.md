# ThingsBoard Rule Engine API: RuleEngineDeviceRpcRequest

## Purpose
Represents a device RPC (Remote Procedure Call) request in the rule engine context.

## Key Responsibilities
- Encapsulates RPC method, parameters, and target device information.
- Used by nodes/services to initiate device RPCs.

## Usage Pattern
- Constructed and passed to RPC services or nodes for execution.

## Best Practices
- Validate RPC parameters and device state before sending.

## Pitfalls
- Invalid RPCs can cause device errors or failures.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
