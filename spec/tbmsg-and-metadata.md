# ThingsBoard Messaging Model: `TbMsg` and `TbMsgMetaData`

## Language & Context
- Language: Java (server-side)
- Domain: Internal message model used by Transport, Actors, and Rule Engine.

## Overview
+ `TbMsg`: immutable message envelope carrying type, originator, payload, routing context, and callbacks.
+ `TbMsgMetaData`: thread-safe key-value map for supplemental attributes.

Key source files:
- common/message/src/main/java/org/thingsboard/server/common/msg/TbMsg.java
- common/message/src/main/java/org/thingsboard/server/common/msg/TbMsgMetaData.java
- common/data/src/main/java/org/thingsboard/server/common/data/msg/TbMsgType.java

## `TbMsg` Structure
Fields (selected):
- `queueName`: destination processing queue
- `id`, `ts`: message identity and timestamp
- `type`, `internalType`: string type and enum `TbMsgType` (prefer enum)
- `originator`: `EntityId` (Device/Asset/...) that produced the message
- `customerId`: optional customer context (auto-inferred if originator is a Customer)
- `metaData`: `TbMsgMetaData` key-value store
- `dataType`: `TbMsgDataType` (e.g., JSON)
- `data`: payload as string
- `ruleChainId`, `ruleNodeId`: current processing context
- `correlationId`, `partition`: cross-service tracing & queue partition
- `previousCalculatedFieldIds`: guard against repeated calculated-field loops
- `ctx`: internal processing stack (push/pop), not serialized
- `callback`: `TbMsgCallback` for backpressure/timeouts, not serialized

```mermaid
classDiagram
  class TbMsg {
    +String queueName
    +UUID id
    +long ts
    +String type
    +TbMsgType internalType
    +EntityId originator
    +CustomerId customerId
    +TbMsgMetaData metaData
    +TbMsgDataType dataType
    +String data
    +RuleChainId ruleChainId
    +RuleNodeId ruleNodeId
    +UUID correlationId
    +Integer partition
  }
  class TbMsgMetaData {
    +Map~String,String~ data
    +getValue(key)
    +putValue(key,value)
    +values()
    +copy()
  }
  TbMsg --> TbMsgMetaData
```

## Construction & Transformation
- Builder API: `TbMsg.newMsg()` → fluent setters → `build()`
- Copy/transform:
  - `copy()`: clone with modifications
  - `transform()`: context-safe transform; copies internal ctx and resets `ruleNodeId` when `ruleChainId` changes
  - `transform(queueName)`: convenience to redirect to another queue
  - `newMsg(tbMsg, queueName, ruleChainId, ruleNodeId)`: for `enqueueForTellNext`

Best practice: always prefer `.type(TbMsgType.X)` over `.type(String)` (string is deprecated in multiple APIs).

## Serialization
- Protobuf conversion: `TbMsg.toProto()` / `TbMsg.fromProto(...)`
- Preserves identity, routing context, metadata, payload, and processing ctx (proto form)

## Runtime Helpers
- `getAndIncrementRuleNodeCounter()`: guard against infinite loops
- `isValid()`: check if callback has not timed out/canceled
- `getMetaDataTs()`: prefer metadata `ts` if present, else internal `ts`

## `TbMsgMetaData`
- Thread-safe, copy-on-write semantics for safe sharing
- `EMPTY` singleton is immutable; use `copy()` to duplicate
- Recommended pattern: start with `new TbMsgMetaData()`, add values, then pass into message

## Example: Transport → Rule Engine
```mermaid
sequenceDiagram
  participant MQTT as MQTT Transport
  participant T as DefaultTransportService
  participant RE as Rule Engine
  MQTT->>T: PostTelemetryMsg(tsKvList)
  T->>T: meta = new TbMsgMetaData()
  T->>T: meta.put("deviceName","...")
  T->>RE: sendToRuleEngine(..., TbMsgType.POST_TELEMETRY_REQUEST, meta)
  RE-->>Node: onMsg(ctx, TbMsg)
```

## Best Practices & Pitfalls
- Do not mutate metadata shared across async flows; use `.copyMetaData()` or `metaData.copy()` when branching.
- Ensure `callback` is propagated when you expect pack-level acks/timeouts.
- Normalize `data` to a clear schema (JSON with a contract) to reduce parsing errors in nodes.

## Verification Checklist
- [ ] Uses `TbMsgType` enums
- [ ] Metadata copied when reused across branches
- [ ] Routing context set/cleared intentionally (ruleChainId/ruleNodeId)
- [ ] Callback supplied where timeouts/backpressure matter

## References
- common/message/src/main/java/org/thingsboard/server/common/msg/TbMsg.java
- common/message/src/main/java/org/thingsboard/server/common/msg/TbMsgMetaData.java
- common/transport/transport-api/src/main/java/org/thingsboard/server/common/transport/service/DefaultTransportService.java
