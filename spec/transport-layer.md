# Transport Layer Specification

## Overview

The Transport layer handles device connectivity, protocol-specific message processing, and routing to the Rule Engine. ThingsBoard supports MQTT, HTTP, CoAP, and LwM2M protocols.

---

## High-Level Architecture

```mermaid
flowchart TD
  subgraph Devices
    D1[MQTT Device]
    D2[HTTP Device]
    D3[CoAP Device]
    D4[LwM2M Device]
  end
  subgraph Transport
    M[MQTT Handler]
    H[HTTP Handler]
    C[CoAP Handler]
    L[LwM2M Handler]
  end
  subgraph Core
    TS[TransportService]
    RE[Rule Engine]
  end
  D1 --> M
  D2 --> H
  D3 --> C
  D4 --> L
  M --> TS
  H --> TS
  C --> TS
  L --> TS
  TS --> RE
```

---

## Transport → Rule Engine Flow

```mermaid
sequenceDiagram
  participant Transport as Transport Handler
  participant TS as DefaultTransportService
  participant RE as Rule Engine
  Transport->>TS: Process message
  TS->>TS: Build TbMsgMetaData
  TS->>RE: sendToRuleEngine(tenantId, deviceId, payload, meta, type)
  RE-->>Nodes: Deliver TbMsg to rule chain
```

### Key Steps

1. Validate limits per session and message (`checkLimits`)
2. Record device activity (`recordActivityInternal`)
3. Prepare identifiers: `TenantId`, `DeviceId`, `CustomerId`
4. Construct metadata per data point
5. Convert incoming data to JSON payload
6. Build and send `TbMsg` with appropriate `TbMsgType`

---

## MQTT Transport

### Topic Structure

| Topic Pattern | Purpose |
|---------------|---------|
| v1/devices/me/telemetry | Publish telemetry data |
| v1/devices/me/attributes | Publish client attributes |
| v1/devices/me/attributes/request/{id} | Request attributes from server |
| v1/devices/me/attributes/response/{id} | Receive attribute response |
| v1/devices/me/rpc/request/{id} | Receive RPC request from server |
| v1/devices/me/rpc/response/{id} | Respond to RPC request |
| v1/devices/me/claim | Claim device |
| v1/devices/me/provision | Provision device |

### Gateway Topics

| Topic Pattern | Purpose |
|---------------|---------|
| v1/gateway/telemetry | Gateway publishes device telemetry |
| v1/gateway/attributes | Gateway publishes device attributes |
| v1/gateway/connect | Gateway connects device |
| v1/gateway/disconnect | Gateway disconnects device |
| v1/gateway/rpc | Gateway RPC handling |

### Connection Flow

```mermaid
sequenceDiagram
  participant Device
  participant Broker as MQTT Broker
  participant Handler as MqttTransportHandler
  participant Auth as DeviceAuthService
  Device->>Broker: CONNECT(clientId, username, password)
  Broker->>Handler: onConnect
  Handler->>Auth: authenticate(credentials)
  Auth-->>Handler: DeviceInfo
  Handler-->>Broker: CONNACK(SUCCESS)
  Broker-->>Device: CONNACK
```

### Authentication Methods

| Method | Configuration |
|--------|---------------|
| Access Token | Username: `$ACCESS_TOKEN`, Password: (empty) |
| Basic Auth | Username: `$DEVICE_NAME`, Password: `$DEVICE_PASSWORD` |
| X.509 Certificate | Client certificate, no username/password |

### Message Processing

```mermaid
flowchart TD
  PUBLISH[MQTT PUBLISH] --> Parse[Parse Topic]
  Parse --> Validate[Validate Session]
  Validate --> Type{Message Type}
  Type -->|telemetry| Telemetry[Process Telemetry]
  Type -->|attributes| Attrs[Process Attributes]
  Type -->|rpc| RPC[Process RPC]
  Telemetry --> Meta[Build Metadata]
  Attrs --> Meta
  RPC --> Meta
  Meta --> Transport[TransportService]
  Transport --> RuleEngine[Rule Engine]
```

---

## HTTP Transport

### Endpoints

| Endpoint | Method | Purpose |
|----------|--------|---------|
| /api/v1/{token}/telemetry | POST | Submit telemetry |
| /api/v1/{token}/attributes | POST | Submit attributes |
| /api/v1/{token}/attributes | GET | Get shared attributes |
| /api/v1/{token}/rpc | GET | Long-poll for RPC requests |
| /api/v1/{token}/rpc/{requestId} | POST | Respond to RPC |
| /api/v1/{token}/claim | POST | Claim device |

### Request Flow

```mermaid
sequenceDiagram
  participant Device
  participant HTTP as HTTP Transport
  participant Auth as DeviceAuthService
  participant TS as TransportService
  Device->>HTTP: POST /api/v1/{token}/telemetry
  HTTP->>Auth: validateToken(token)
  Auth-->>HTTP: DeviceInfo
  HTTP->>TS: process(sessionInfo, telemetryMsg)
  TS-->>HTTP: OK
  HTTP-->>Device: 200 OK
```

---

## CoAP Transport

### Endpoints

| Path | Method | Purpose |
|------|--------|---------|
| /api/v1/{token}/telemetry | POST | Submit telemetry |
| /api/v1/{token}/attributes | POST | Submit attributes |
| /api/v1/{token}/attributes | GET | Get shared attributes |
| /api/v1/{token}/rpc | GET | Observe RPC requests |

### CoAP Features

- Lightweight UDP-based protocol
- Confirmable (CON) and Non-confirmable (NON) messages
- Block-wise transfer for large payloads
- DTLS for secure communication

---

## Device Session Management

### Session Lifecycle

```mermaid
stateDiagram-v2
  [*] --> Connecting
  Connecting --> Authenticated: Credentials valid
  Connecting --> Rejected: Credentials invalid
  Authenticated --> Active: Session created
  Active --> Inactive: Timeout
  Active --> Disconnected: Client disconnect
  Inactive --> Active: Activity received
  Inactive --> Disconnected: Session timeout
  Disconnected --> [*]
```

### Session Properties

| Property | Description |
|----------|-------------|
| sessionId | Unique session identifier |
| tenantId | Owning tenant |
| deviceId | Connected device |
| customerId | Customer (if assigned) |
| transportType | MQTT, HTTP, CoAP, LwM2M |
| lastActivityTime | Last message timestamp |

---

## Rate Limiting

### Configuration

| Property | Description |
|----------|-------------|
| transport.rate_limits.enabled | Enable rate limiting |
| transport.rate_limits.tenant | Tenant-level limits |
| transport.rate_limits.device | Device-level limits |
| transport.sessions.max_per_tenant | Max sessions per tenant |
| transport.sessions.max_per_device | Max sessions per device |

### Limit Types

| Type | Description |
|------|-------------|
| Messages per second | Max message rate |
| Data points per second | Max telemetry points |
| Concurrent sessions | Max active sessions |

---

## Best Practices

### Do's

- Use appropriate protocol for device constraints
- Enable TLS/DTLS for production
- Configure rate limits per tenant
- Monitor session counts and message rates
- Use gateways for legacy protocol conversion

### Don'ts

- Don't expose transport ports without authentication
- Don't disable rate limiting in production
- Don't use HTTP for battery-constrained devices
- Don't ignore connection timeouts

---

## Common Pitfalls

| Pitfall | Solution |
|---------|----------|
| Missing metadata fields | Ensure `meta.copy()` per data point |
| Rate limit rejections | Check `checkLimits` counters |
| Wrong TbMsgType | Telemetry uses `POST_TELEMETRY_REQUEST` |
| Session timeouts | Configure appropriate keep-alive |

---

## See Also

- [Rule Engine Core](rule-engine-core.md)
- [Device State Management](device-state-management.md)
- [Security & Authentication](security-auth.md)
