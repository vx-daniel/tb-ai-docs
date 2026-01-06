# ThingsBoard Device RPC via Rule Engine

## Language & Context
- Language: Java (server-side)
- Domain: Device RPC request/response DTOs used by the Rule Engine and Transport.

Key source files:
- org/thingsboard/rule/engine/api/RuleEngineDeviceRpcRequest.java
- org/thingsboard/rule/engine/api/RuleEngineDeviceRpcResponse.java

## RPC Request DTO
`RuleEngineDeviceRpcRequest` (immutable, Lombok `@Data @Builder`) encapsulates an outbound RPC invocation from the platform to a device.

Fields (selected):
- `tenantId`, `deviceId`: routing context
- `requestId` (int) and `requestUUID` (UUID): correlation identifiers
- `originServiceId`: service that originated the RPC (for clustered deployments)
- `oneway`: fire-and-forget semantics (no response expected)
- `persisted`: whether to persist and retry according to policies
- `method`, `body`: RPC method name and payload
- `expirationTime`: epoch millis deadline
- `restApiCall`: whether originated from REST
- `additionalInfo`: free-form JSON string for custom context
- `retries`: optional retry budget

```mermaid
classDiagram
  class RuleEngineDeviceRpcRequest {
    +TenantId tenantId
    +DeviceId deviceId
    +int requestId
    +UUID requestUUID
    +String originServiceId
    +boolean oneway
    +boolean persisted
    +String method
    +String body
    +long expirationTime
    +boolean restApiCall
    +String additionalInfo
    +Integer retries
  }
```

## RPC Response DTO
`RuleEngineDeviceRpcResponse` (immutable, Lombok `@Data @Builder`) represents a device response or error.

Fields:
- `deviceId`, `requestId`
- `response`: `Optional<String>` payload
- `error`: `Optional<RpcError>` when failed or timed out

```mermaid
classDiagram
  class RuleEngineDeviceRpcResponse {
    +DeviceId deviceId
    +int requestId
    +Optional~String~ response
    +Optional~RpcError~ error
  }
```

## Typical Flow
```mermaid
sequenceDiagram
  participant Node as TbNode
  participant Ctx as TbContext
  participant TR as Transport
  Node->>Ctx: build RPC request (method/body)
  Ctx-->>TR: RuleEngineRpcService.send(request)
  TR-->>Device: Deliver RPC
  alt oneway == true
    TR-->>Ctx: Ack only
  else response expected
    Device-->>TR: Response or timeout
    TR-->>Ctx: Response/Error -> emit TbMsg
  end
```

## Best Practices
- Set `expirationTime` and `retries` in alignment with transport capabilities.
- Avoid large `body` payloads; consider schema and compression if needed.
- For idempotency, prefer `requestUUID` for cross-service correlation.
- Use persisted RPCs for unreliable connectivity; monitor retry queues.

## Common Pitfalls
- Forgetting to check `oneway` and awaiting a response that will never arrive.
- Unbounded retry loops without `expirationTime` degrade system health.

## References
- org/thingsboard/rule/engine/api/RuleEngineDeviceRpcRequest.java
- org/thingsboard/rule/engine/api/RuleEngineDeviceRpcResponse.java
