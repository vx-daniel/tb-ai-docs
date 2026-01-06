---
title: Rule Engine Node Implementation & Lifecycle
version: 1.0
date_created: 2026-01-06
last_updated: 2026-01-06
owner: ThingsBoard Architecture Team
tags: [architecture, rule-engine, node, implementation, java]
---

# Introduction

This specification details the technical implementation and lifecycle management of rule engine nodes in ThingsBoard. It covers node interfaces, context usage, configuration, error handling, and extension patterns.

## 1. Purpose & Scope

Defines how to implement, configure, and manage rule engine nodes. Intended for backend developers and integrators.

## 2. Definitions

- **Node**: Processing unit in the rule engine
- **TbNode**: Node interface contract
- **TbContext**: Execution context for nodes
- **TbMsg**: Message object
- **@RuleNode**: Annotation for node metadata

## 3. Requirements, Constraints & Guidelines

- **REQ-001**: All nodes must implement `TbNode` and be annotated with `@RuleNode`
- **REQ-002**: Nodes must acknowledge every message (success/failure)
- **REQ-003**: Node configuration must be validated in `init()`
- **REQ-004**: Nodes must be stateless except for configuration/state fields
- **CON-001**: Nodes must not access services outside `TbContext`
- **GUD-001**: Use `TbNodeConfiguration` for config data
- **PAT-001**: Use Factory pattern for node instantiation

## 4. Interfaces & Data Contracts

Example node interface:
```java
public interface TbNode {
    void init(TbContext ctx, TbNodeConfiguration configuration) throws TbNodeException;
    void onMsg(TbContext ctx, TbMsg msg) throws ExecutionException, InterruptedException, TbNodeException;
    default void destroy() {}
}
```

Example annotation:
```java
@RuleNode(type = ComponentType.ACTION, name = "Log Node", ...)
public class TbLogNode implements TbNode { ... }
```

## 5. Acceptance Criteria

- **AC-001**: All nodes acknowledge every message
- **AC-002**: All configuration is validated and upgradeable
- **AC-003**: All node errors are handled via `TbNodeException`

## 6. Test Automation Strategy

- **Unit tests**: JUnit, Mockito for node logic
- **Integration tests**: Simulated rule chains
- **Coverage**: 90%+ for node methods

## 7. Rationale & Context

Stateless, context-driven nodes enable extensibility, reliability, and safe async processing.

## 8. Dependencies & External Integrations

- **INF-001**: Rule engine framework
- **DAT-001**: TbContext, TbMsg, TbNodeConfiguration

## 9. Examples & Edge Cases

```java
// Edge case: Unacknowledged message
public void onMsg(TbContext ctx, TbMsg msg) {
    // Missing ctx.tellSuccess or ctx.tellFailure blocks the chain
}
```

## 10. Validation Criteria

- All nodes pass unit and integration tests
- All error and config paths are covered

## 11. Related Specifications / Further Reading

- [spec-architecture-blueprint.md](spec-architecture-blueprint.md)
- [rule-engine.spec.md](rule-engine.spec.md)
- [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
---
