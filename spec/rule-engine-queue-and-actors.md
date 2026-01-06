# Rule Engine Queues and Actor Routing

## Language & Context
- Language: Java (server-side)
- Domain: How `TbMsg` moves through partitioned queues into actors that drive rule chains.

Key source files:
- common/queue/src/main/java/org/thingsboard/server/queue/common/TbRuleEngineProducerService.java
- common/message/src/main/java/org/thingsboard/server/common/msg/queue/QueueToRuleEngineMsg.java
- common/message/src/main/java/org/thingsboard/server/common/msg/TbRuleEngineActorMsg.java
- application/src/main/java/org/thingsboard/server/service/queue/ruleengine/TbRuleEngineQueueConsumerManager.java (consumption)

## Produce → Partition → Consume
`TbRuleEngineProducerService` partitions and publishes `TbMsg` to the Rule Engine queue(s).

```mermaid
sequenceDiagram
  participant Producer as TbRuleEngineProducerService
  participant Queue as Queue (partitioned)
  participant Consumer as QueueConsumerManager
  participant Actors as Tenant/RuleChain Actors
  Producer->>Queue: send(ToRuleEngineMsg with TbMsgProto)
  Queue-->>Consumer: poll(partition)
  Consumer->>Actors: wrap as QueueToRuleEngineMsg & dispatch
```

### Partitioning Details
- Resolves partitions via `PartitionService.resolveAll(ServiceType.TB_RULE_ENGINE, queueName, tenantId, originator)`
- If multiple partitions are returned, clones the `TbMsg` per partition:
  - Sets a shared `correlationId`
  - Assigns per-message `partition`
  - Keeps first message’s `id`, others get new UUIDs

```mermaid
flowchart TD
  A[TbMsg] --> B[resolveAll partitions]
  B -->|N>1| C[Loop partitions]
  C --> D[transform(): id/correlationId/partition]
  D --> E[send(partition i)]
```

## Actor Envelope: QueueToRuleEngineMsg
- Wraps `TbMsg` with `tenantId`, optional `relationTypes`, and an optional `failureMessage`
- Implements `TbRuleEngineActorMsg`, enabling polymorphic routing
- On actor stop, calls `msg.callback.onFailure(...)` with a meaningful reason
- `isTellNext()`: whether `relationTypes` exist to drive `tellNext` routing

## End-to-End Context
- Producers: Transport, REST, and other services call `sendToRuleEngine(...)`
- Consumers: `TbRuleEngineQueueConsumerManager` deserializes protobuf to `TbMsg`, creates `QueueToRuleEngineMsg`, and pushes to the actor system (Tenant → RuleChain → RuleNode)

```mermaid
sequenceDiagram
  participant TS as DefaultTransportService
  participant Prod as TbRuleEngineProducerService
  participant Q as Queue
  participant CM as QueueConsumerManager
  participant App as AppActor/TenantActor
  TS->>Prod: sendToRuleEngine(tenantId, TbMsg)
  Prod->>Q: ToRuleEngineMsg(TbMsgProto)
  Q-->>CM: poll
  CM->>App: QueueToRuleEngineMsg(TbMsg)
  App-->>RuleChain: dispatch based on relationTypes/failure
```

## Best Practices
- Preserve `queueName` on `TbMsg` for correct partition resolution
- Use `correlationId` for multi-partition tracing
- Ensure `callback` is present when upstream expects delivery acks (e.g., transport pack)

## References
- common/queue/src/main/java/org/thingsboard/server/queue/common/TbRuleEngineProducerService.java
- common/message/src/main/java/org/thingsboard/server/common/msg/queue/QueueToRuleEngineMsg.java
- application/src/main/java/org/thingsboard/server/service/queue/ruleengine/TbRuleEngineQueueConsumerManager.java
