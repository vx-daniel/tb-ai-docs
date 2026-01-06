# ThingsBoard Rule Engine API: MqttClientSettings

## Purpose
Encapsulates configuration for MQTT client connections used by rule engine nodes.

## Key Responsibilities
- Holds connection parameters (host, port, credentials, etc.).
- Used by MQTT integration nodes for device or cloud connectivity.

## Usage Pattern
- Passed to MQTT nodes/services for establishing connections.

## Best Practices
- Securely store and validate credentials.
- Use TLS/SSL for secure connections.

## Pitfalls
- Incorrect settings can prevent device/cloud communication.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
