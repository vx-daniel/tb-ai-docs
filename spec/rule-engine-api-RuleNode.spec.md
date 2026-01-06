# ThingsBoard Rule Engine API: RuleNode Annotation

## Purpose
Provides metadata for rule engine nodes, enabling ThingsBoard to discover, configure, and document nodes in the UI and backend.

## Key Responsibilities
- Declares node type, name, configuration class, description, icon, documentation URL, and other metadata.
- Used by the platform to generate UI forms and documentation for node configuration.

## Usage Pattern
- Annotate each node class with `@RuleNode`.
- Specify all required metadata for discoverability and usability.

## Best Practices
- Provide clear descriptions and documentation URLs for each node.
- Keep configuration classes up to date with annotation metadata.

## Pitfalls
- Missing or incorrect metadata can make nodes hard to use or discover in the UI.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
