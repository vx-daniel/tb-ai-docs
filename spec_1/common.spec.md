# ThingsBoard Module: common

## Language and Context
- **Language:** Java
- **Context:** Shared utilities, data models, and protocol definitions used across all ThingsBoard modules.

## Structure and Key Components
- **Data Models:** Entity classes (Device, Asset, Tenant, User, etc.) and enums (EntityType, ObjectType).
- **Utilities:** Helper classes for JSON, async handling (`DonAsynchron`), and error codes.
- **Protobuf Definitions:** Protocols for device communication (MQTT, CoAP, LwM2M, etc.).

## Example: DonAsynchron
- **Purpose:** Simplifies handling of `ListenableFuture` with success/failure callbacks.
- **Logic Flow:**
  1. Accepts a future and two consumers (onSuccess, onFailure).
  2. Registers callbacks for async result handling.

## Complex Concepts
- **Asynchronous Programming:** Extensive use of `ListenableFuture` and callback patterns for non-blocking operations.
- **Protobuf:** Enables cross-language communication with IoT devices.

## Best Practices
- Use utility classes for common patterns (async, JSON, error handling).
- Keep data models immutable where possible.

## Common Pitfalls
- Not handling all async failure cases can lead to silent errors.
- Protobuf changes require regeneration of Java classes.

## Recommendations
- Document all utility methods and data models.
- Use static analysis to catch unhandled async errors.

---
See also: [thingsboard-architecture.spec.md](thingsboard-architecture.spec.md)
