# Lab 1: TbContext Routing

## Goal

Build and verify a simple rule node that routes messages via `tellSuccess`, `tellNext(relation)`, and `tellFailure`, and uses `tellSelf(delay)` for a retry path.

## Prerequisites

- Ability to add a custom rule node (or adapt an existing script node) in your ThingsBoard instance
- A test device able to send telemetry
- Access to rule chain editor and server logs

## Starting Point

- Import or create a rule chain with:
  - An input node that accepts all incoming device telemetry
  - Your custom node under test
  - Three outgoing relations from your node: `SUCCESS`, `RETRY`, and `FAILURE`

## Steps

1. Implement node logic:
   - If metadata contains key `route = success`, call `tellSuccess(msg)`
   - If `route = retry`, call `tellSelf(msg, 5000)` and return
   - Otherwise, call `tellNext(msg, "RETRY")` for retries, or `tellFailure(msg, error)` on unrecoverable errors
2. Send test telemetry with metadata markers:
   - Case A: Include metadata `route=success` and confirm it reaches the `SUCCESS` branch
   - Case B: Include metadata `route=retry` and confirm a delayed reprocessing followed by `RETRY` branch
   - Case C: Include metadata `route=failure` (or malformed payload) to trigger `FAILURE`
3. Observe routing:
   - In the Rule Chain UI, confirm which relation path was taken
   - In logs, verify the node’s decision points and callbacks

## Success Criteria

- Case A reaches the node connected via `SUCCESS` relation
- Case B reprocesses the message after ~5s and reaches the `RETRY` relation
- Case C routes to `FAILURE` and the callback reports failure

## Example Outputs

- UI: Visual trace shows message traversing the expected relation
- Logs: messages similar to
  - `Pushing message to single target: [SUCCESS]`
  - `Scheduling tellSelf after 5000ms`
  - `Failure during message processing by Rule Node [<nodeId>]`

## Solution (Reference)

Pseudo-logic for a minimal custom node:

```java
if (meta.getValue("route").orElse("").equals("success")) {
   ctx.tellSuccess(msg);
   return;
}
if (meta.getValue("route").orElse("").equals("retry")) {
   ctx.tellSelf(msg, 5000);
   return;
}
try {
   ctx.tellNext(msg, "RETRY");
} catch (Exception e) {
   ctx.tellFailure(msg, e);
}
```

## References

- [spec/tb-context-and-services.md](../spec/tb-context-and-services.md)
- [thingsboard/application/src/main/java/org/thingsboard/server/actors/ruleChain/RuleChainActorMessageProcessor.java](../thingsboard/application/src/main/java/org/thingsboard/server/actors/ruleChain/RuleChainActorMessageProcessor.java)
