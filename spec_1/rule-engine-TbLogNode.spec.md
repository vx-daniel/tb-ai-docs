# ThingsBoard Rule Engine: TbLogNode

## Language and Context
- **Language:** Java
- **Context:** Rule engine action node for logging messages, part of the event-driven processing pipeline.

## Purpose
- Logs incoming messages in the rule engine, optionally transforming them with a user-defined script (JavaScript or TBEL).

## Structure and Key Components
- **Implements:** `TbNode` interface (standard for all rule nodes)
- **Configurable:** Accepts a script for message transformation, or uses a standard log format.
- **ScriptEngine:** Used for executing user-provided scripts asynchronously.

## Logic Flow
1. **Initialization:**
   - Reads configuration to determine if a custom script is used.
   - If custom, creates a `ScriptEngine` for async execution.
2. **Message Handling (`onMsg`):**
   - If logging is disabled, immediately acknowledges success.
   - If standard config, logs message and metadata in a default format.
   - If custom script, executes script asynchronously and logs the result.
   - On script success, logs output and acknowledges message; on failure, reports error.
3. **Cleanup:**
   - Destroys the script engine if used.

## Important Variables
- `scriptEngine`: Executes user scripts for message transformation.
- `standard`: Boolean flag for standard vs. custom logging.

## Complex Concepts
- **Asynchronous Execution:** Uses Guava `Futures.addCallback` for non-blocking script execution.
- **Script Language Selection:** Supports both JavaScript and TBEL (ThingsBoard Expression Language).

## Best Practices
- Always handle both success and failure in async callbacks.
- Use standard logging for performance unless custom transformation is needed.

## Common Pitfalls
- Custom scripts can introduce errors or performance issues if not validated.
- Not destroying the script engine can lead to resource leaks.

## Recommendations
- Validate user scripts before deployment.
- Monitor log node performance in production.

---
See also: [rule-engine.spec.md](rule-engine.spec.md)
