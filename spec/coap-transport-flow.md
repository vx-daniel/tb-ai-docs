# CoAP Transport Flow Specification

## Overview

This document describes the CoAP transport layer in ThingsBoard, which enables lightweight device communication for constrained IoT devices.

---

## Key Components

### CoapTransportResource

Handles incoming CoAP requests from devices.

| Resource Path                | Method | Description                        |
|------------------------------|--------|------------------------------------|
| /api/v1/{deviceToken}/telemetry | POST | Submit device telemetry            |
| /api/v1/{deviceToken}/attributes | POST | Submit device attributes          |
| /api/v1/{deviceToken}/attributes | GET  | Request shared attributes          |
| /api/v1/{deviceToken}/rpc    | GET    | Observe for server-side RPC        |
| /api/v1/{deviceToken}/rpc/{requestId} | POST | Respond to server-side RPC  |

---

## Request Flow

```mermaid
sequenceDiagram
  participant Device
  participant CoAP as CoapTransportResource
  participant Service as TransportService
  participant Actors as ActorSystem
  Device->>CoAP: POST /telemetry
  CoAP->>Service: processTelemetry(token, payload)
  Service->>Actors: send TbMsg
  Actors-->>Service: ack
  Service-->>CoAP: response
  CoAP-->>Device: 2.04 Changed
```

---

## Authentication

- Device token in URI path
- Token validated against device credentials
- Invalid token returns 4.01 Unauthorized

---

## Payload Formats

- JSON (application/json)
- CBOR (application/cbor) for binary efficiency

---

## Error Handling

| CoAP Code | Meaning                        |
|-----------|--------------------------------|
| 2.04      | Changed (success)              |
| 4.00      | Bad request                    |
| 4.01      | Unauthorized                   |
| 5.00      | Internal server error          |

---

## Complete Resource Paths

### Telemetry Resources

| Resource Path                              | Method | Description                |
|--------------------------------------------|--------|----------------------------|
| /api/v1/{deviceToken}/telemetry            | POST   | Submit device telemetry    |

### Attribute Resources

| Resource Path                              | Method | Description                |
|--------------------------------------------|--------|----------------------------|
| /api/v1/{deviceToken}/attributes           | POST   | Publish client attributes  |
| /api/v1/{deviceToken}/attributes           | GET    | Request attributes         |
| /api/v1/{deviceToken}/attributes/updates   | GET    | Observe attribute updates  |

### RPC Resources

| Resource Path                              | Method | Description                |
|--------------------------------------------|--------|----------------------------|
| /api/v1/{deviceToken}/rpc                  | GET    | Observe server-side RPC    |
| /api/v1/{deviceToken}/rpc/{requestId}      | POST   | Respond to server RPC      |
| /api/v1/{deviceToken}/rpc                  | POST   | Client-side RPC request    |

### Device Management Resources

| Resource Path                              | Method | Description                |
|--------------------------------------------|--------|----------------------------|
| /api/v1/{deviceToken}/claim                | POST   | Claim device               |
| /api/v1/provision                          | POST   | Provision new device       |
| /api/v1/{deviceToken}/firmware             | GET    | Get firmware info          |
| /api/v1/{deviceToken}/firmware/chunk       | GET    | Download firmware chunk    |

---

## CoAP Message Types

| Type    | Description                                         |
|---------|-----------------------------------------------------|
| CON     | Confirmable - requires ACK                          |
| NON     | Non-confirmable - no ACK required                   |
| ACK     | Acknowledgment for CON messages                     |
| RST     | Reset - indicates message processing error          |

### Message Flow (Confirmable)

```mermaid
sequenceDiagram
  participant Device
  participant Server
  Device->>Server: CON POST /telemetry
  Server-->>Device: ACK 2.04 Changed
```

### Message Flow (Non-Confirmable)

```mermaid
sequenceDiagram
  participant Device
  participant Server
  Device->>Server: NON POST /telemetry
  Note over Server: No ACK required
```

---

## Observe Pattern (RFC 7641)

### Subscribe to Attribute Updates

```mermaid
sequenceDiagram
  participant Device
  participant Server
  participant Rule as Rule Engine
  Device->>Server: GET /attributes (Observe=0)
  Server-->>Device: 2.05 Content + Observe token
  Note over Device,Server: Subscription active
  Rule->>Server: Attribute changed
  Server-->>Device: 2.05 Content (notification)
  Rule->>Server: Attribute changed
  Server-->>Device: 2.05 Content (notification)
```

### Subscribe to Server RPC

```mermaid
sequenceDiagram
  participant Device
  participant Server
  participant App as Application
  Device->>Server: GET /rpc (Observe=0)
  Server-->>Device: 2.05 Content + Observe token
  App->>Server: Send RPC
  Server-->>Device: 2.05 Content (RPC request)
  Device->>Server: POST /rpc/{requestId}
  Server-->>App: RPC response
```

---

## Power Saving Modes

### Power Mode Configuration

| Mode  | Description                                         |
|-------|-----------------------------------------------------|
| DRX   | Discontinuous Reception - periodic wake-up          |
| PSM   | Power Saving Mode - extended sleep periods          |
| eDRX  | Extended DRX - longer sleep with paging windows     |

### eDRX Configuration

| Parameter              | Description                              |
|------------------------|------------------------------------------|
| edrxCycle              | eDRX cycle duration in milliseconds      |
| pagingTransmissionWindow | Paging window duration                 |

```mermaid
flowchart TD
  Device[Device] --> Mode{Power Mode}
  Mode -->|DRX| DRX[Periodic Wake]
  Mode -->|PSM| PSM[Extended Sleep]
  Mode -->|eDRX| EDRX[Extended DRX]
  EDRX --> Paging[Paging Window]
  Paging --> Receive[Receive Messages]
```

---

## DTLS Security

### DTLS Configuration

| Property                          | Description                          |
|-----------------------------------|--------------------------------------|
| transport.coap.dtls.enabled       | Enable DTLS                          |
| transport.coap.dtls.credentials.type | PEM or KEYSTORE                   |
| transport.coap.dtls.key_store     | Keystore path                        |
| transport.coap.dtls.key_password  | Key password                         |

### DTLS Handshake

```mermaid
sequenceDiagram
  participant Device
  participant Server
  Device->>Server: ClientHello
  Server-->>Device: HelloVerifyRequest
  Device->>Server: ClientHello + Cookie
  Server-->>Device: ServerHello + Certificate
  Device->>Server: ClientKeyExchange + Finished
  Server-->>Device: Finished
  Note over Device,Server: Secure channel established
```

### Client Certificate Authentication

```mermaid
sequenceDiagram
  participant Device
  participant Server
  participant Auth as DeviceAuthService
  Device->>Server: DTLS + Client Certificate
  Server->>Auth: validateCertificate(cert)
  Auth-->>Server: DeviceInfo (from CN)
  Server-->>Device: DTLS Established
```

---

## Block-Wise Transfer (RFC 7959)

### Large Payload Upload

```mermaid
sequenceDiagram
  participant Device
  participant Server
  Device->>Server: POST /telemetry Block1=0/M
  Server-->>Device: 2.31 Continue
  Device->>Server: POST /telemetry Block1=1/M
  Server-->>Device: 2.31 Continue
  Device->>Server: POST /telemetry Block1=2/0
  Server-->>Device: 2.04 Changed
```

### Firmware Download

```mermaid
sequenceDiagram
  participant Device
  participant Server
  Device->>Server: GET /firmware Block2=0
  Server-->>Device: 2.05 Content Block2=0/M
  Device->>Server: GET /firmware Block2=1
  Server-->>Device: 2.05 Content Block2=1/M
  Device->>Server: GET /firmware Block2=2
  Server-->>Device: 2.05 Content Block2=2/0
```

---

## CBOR Payload Format

### Telemetry (CBOR)

Binary CBOR encoding of:

```json
{"temperature": 22.5, "humidity": 60}
```

CBOR advantages:

- 30-50% smaller than JSON
- Faster parsing on constrained devices
- Native binary data support

---

## Rate Limiting

| Limit Type         | Scope    | Description                              |
|--------------------|----------|------------------------------------------|
| requests.limit     | Device   | Max CoAP requests per device per second  |
| requests.limit     | Tenant   | Max CoAP requests per tenant per second  |

---

## Configuration Properties

| Property                          | Default | Description                          |
|-----------------------------------|---------|--------------------------------------|
| transport.coap.bind_address       | 0.0.0.0 | CoAP server bind address             |
| transport.coap.bind_port          | 5683    | CoAP server port                     |
| transport.coap.dtls.bind_port     | 5684    | DTLS server port                     |
| transport.coap.timeout            | 10000   | Request timeout in ms               |

---

## Best Practices

- Use DTLS for security
- Prefer CBOR for constrained devices
- Use observe for RPC and attribute updates
- Configure appropriate power saving modes for battery devices
- Use block-wise transfer for large payloads
- Implement retransmission for CON messages
- Consider NON messages for non-critical telemetry

---

## See Also

- [MQTT Transport Flow](mqtt-transport-flow.md)
- [HTTP Transport Flow](http-transport-flow.md)
- [Transport to Rule Engine Flow](transport-to-rule-engine-flow.md)
- [Device Asset Profiles](device-asset-profiles.md)
