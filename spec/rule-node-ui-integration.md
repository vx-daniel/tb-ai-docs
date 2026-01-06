# RuleNode UI Integration & Metadata

## Language & Context
- Language: Java (server-side) + JSON templates
- Domain: How `@RuleNode` annotation metadata drives the TB UI and configuration lifecycle.

Key source:
- org/thingsboard/rule/engine/api/RuleNode.java

## Annotation Fields and UI Effects
- `type`: `ComponentType` — category (e.g., FILTER, TRANSFORMATION, ACTION)
- `name`: display name in the palette
- `nodeDescription` / `nodeDetails`: help text and docs for the editor
- `configClazz`: configuration class that the UI uses to render a form
- `relationTypes`: default allowed relation labels (e.g., `SUCCESS`, `FAILURE`)
- `customRelations`: whether users can add arbitrary relation types
- `hasQueueName`: exposes queue selection in the UI when true
- `scope`: `ComponentScope` (TENANT, SYSTEM) for visibility
- `ruleChainNode`: marks a node that encapsulates a nested rule chain
- `ruleChainTypes`: which chain types support this node (CORE, EDGE)
- `version`: current config version; used with `TbNode.upgrade`
- `uiResources` / `configDirective` / `icon` / `iconUrl` / `docUrl`: UI customization hooks

```mermaid
flowchart TD
  Dev[Implement TbNode + @RuleNode] --> UI[UI loads metadata]
  UI --> Form[Render config form]
  UI --> Palette[Show icon/name]
  Form --> JSON[Persist config JSON]
  JSON --> Node[init(TbNodeConfiguration)]
```

## Configuration Lifecycle
- UI serializes form values into node `configuration` (JSON)
- Engine passes `TbNodeConfiguration.data` to `TbNode.init`
- If `version` increases in a future release, engine may call `TbNode.upgrade(fromVersion, oldConfiguration)` to migrate configuration

## Authoring Guidance
- Keep `configClazz` aligned with the actual config POJO used inside `TbNode.init`
- Provide succinct `nodeDescription` and actionable `nodeDetails` with examples
- Choose relation labels that match expected `TbMsgType` branches or business outcomes
- Increment `version` and implement `upgrade` when changing config structure

## Mapping to Templates
- Rule chain JSON templates reference node types by FQCN (e.g., `org.thingsboard.rule.engine.filter.TbMsgTypeSwitchNode`)
- Node `configuration` is embedded under that node entry and must match the expected config schema

## References
- org/thingsboard/rule/engine/api/RuleNode.java
- application/src/main/data/json/tenant/rule_chains/root_rule_chain.json
