# Lab 2: Queue & Actor Tracing

## Goal

Trace a single message from producer → queue partition → consumer → App/Tenant/RuleChain actors, confirming `correlationId`, `partition`, and routing decisions.

## Prerequisites

- Access to server logs with DEBUG enabled for queue/actor processing
- A simple rule chain that forwards messages to a known node
- Ability to send a single telemetry message from a device

## Starting Point

- Ensure the queue used by Rule Engine is correctly configured
- Identify the queue name (e.g., `Main`) and enable debug logs for relevant packages

## Steps

1. Send a test message from a device (one key/value is sufficient)
2. Observe producer logs:
   - Look for entries from `TbRuleEngineProducerService` showing `resolveAll` and partition decisions
   - Confirm a `correlationId` is set when multiple partitions are involved
3. Observe consumer logs:
   - Confirm protobuf `ToRuleEngineMsg` is consumed and converted to `QueueToRuleEngineMsg`
4. Observe actor logs:
   - App/Tenant actor receives `QueueToRuleEngineMsg`
   - `RuleChainActorMessageProcessor` decides target based on relation types or first node
5. Confirm delivery:
   - If same partition (tpi.isMyPartition), expect `pushMsgToNode`
   - Otherwise, expect `putToQueue` with new `TbMsg` id

## Success Criteria

- You can identify the `partition` and `correlationId` (if applicable) in logs
- You see `QueueToRuleEngineMsg` creation and delivery to the appropriate actor
- You can point to the routing decision (`pushToTarget`/`putToQueue`) for the message

## Example Outputs

### Suggested Logger Categories

- org.thingsboard.server.queue.common.TbRuleEngineProducerService
- org.thingsboard.server.service.queue.ruleengine
- org.thingsboard.server.actors.app
- org.thingsboard.server.actors.ruleChain

### Sample Log Excerpts

- Producer:
  - "resolveAll partitions for tenant=... partitions=[...]"
  - "Cloning TbMsg for partition=... correlationId=... id=..."
- Consumer:
  - "Polled ToRuleEngineMsg partition=... size=1"
  - "Created QueueToRuleEngineMsg tenant=... relationTypes=null"
- Actor:
  - "Pushing message to single target: [RULE_NODE {nodeId}]"
  - "putToQueue -> new TbMsg id={uuid} ruleChainId={ruleChainId}"

## References

- [spec/rule-engine-queue-and-actors.md](../spec/rule-engine-queue-and-actors.md)
- [thingsboard/common/queue/src/main/java/org/thingsboard/server/queue/common/TbRuleEngineProducerService.java](../thingsboard/common/queue/src/main/java/org/thingsboard/server/queue/common/TbRuleEngineProducerService.java)
- [thingsboard/common/message/src/main/java/org/thingsboard/server/common/msg/queue/QueueToRuleEngineMsg.java](../thingsboard/common/message/src/main/java/org/thingsboard/server/common/msg/queue/QueueToRuleEngineMsg.java)
- [thingsboard/application/src/main/java/org/thingsboard/server/actors/ruleChain/RuleChainActorMessageProcessor.java](../thingsboard/application/src/main/java/org/thingsboard/server/actors/ruleChain/RuleChainActorMessageProcessor.java)
