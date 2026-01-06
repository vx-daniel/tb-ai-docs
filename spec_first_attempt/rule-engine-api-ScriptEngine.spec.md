# ThingsBoard Rule Engine API: ScriptEngine

## Purpose
Executes user-defined scripts (JavaScript, TBEL) for message transformation and filtering in rule nodes.

## Key Responsibilities
- Compiles and runs scripts asynchronously or synchronously.
- Provides APIs for executing scripts with message data and metadata.

## Usage Pattern
- Used by nodes like `TbLogNode`, `TbJsFilterNode`, etc., to transform or filter messages.
- Created via `TbContext.createScriptEngine()`.

## Best Practices
- Validate and sandbox scripts to prevent security issues.
- Destroy script engine instances when no longer needed.

## Pitfalls
- Untrusted scripts can introduce vulnerabilities.
- Memory leaks if script engines are not destroyed.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
