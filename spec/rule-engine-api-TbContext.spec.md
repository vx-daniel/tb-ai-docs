# ThingsBoard Rule Engine API: TbContext

## Purpose
Provides the execution context for rule engine nodes, including message flow control, access to entity services, and utility methods.

## Key Responsibilities
- **Message Flow:**
  - `tellSuccess`, `tellNext`, `tellFailure`, `tellSelf`, `enqueue`, `input`, `output` for routing and processing messages.
- **Message Creation/Transformation:**
  - `newMsg`, `transformMsg`, `customerCreatedMsg`, `deviceCreatedMsg`, `assetCreatedMsg` for creating and modifying messages.
- **Service Access:**
  - Provides access to all core services (DeviceService, AlarmService, AssetService, etc.) for entity operations.
- **Async/Queue Control:**
  - Methods for queueing, delayed processing, and handling async results.

## Usage Pattern
- Passed to every node's `init` and `onMsg` methods.
- Use context methods for all message routing and entity access.

## Best Practices
- Use context-provided services for all entity operations.
- Always acknowledge messages to avoid blocking the rule chain.
- Use async methods for high-throughput scenarios.

## Pitfalls
- Not using context for message flow can break rule chain logic.
- Direct DB/service access outside context can cause inconsistencies.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
