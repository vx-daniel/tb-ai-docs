# ThingsBoard Module: transport

## Language and Context
- **Language:** Java, Protobuf
- **Context:** Handles device connectivity and communication protocols (MQTT, HTTP, CoAP, LwM2M, SNMP).

## Structure and Key Components
- **Protocol Implementations:** Each protocol (e.g., MQTT, HTTP) has its own module and entry point.
- **Protobuf Schemas:** Define message formats for cross-language/device communication.
- **Transport Services:** Manage device sessions, authentication, and message routing.

## Example: MQTT Transport
- **Purpose:** Handles MQTT connections, device authentication, and message delivery.
- **Logic Flow:**
  1. Device connects via MQTT.
  2. Transport authenticates and establishes session.
  3. Messages are routed to rule engine or persisted.

## Complex Concepts
- **Session Management:** Tracks device state and handles reconnects.
- **Protocol Buffers:** Used for efficient, cross-platform message serialization.

## Best Practices
- Isolate protocol logic in separate modules.
- Validate all incoming messages.

## Common Pitfalls
- Protocol-specific bugs can affect all device communication.
- Incomplete session cleanup can cause resource leaks.

## Recommendations
- Add integration tests for each protocol.
- Monitor session and connection metrics.

---
See also: [thingsboard-architecture.spec.md](thingsboard-architecture.spec.md)
