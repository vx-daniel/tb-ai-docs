# ThingsBoard Rule Engine API: JobManager

## Purpose
Manages background jobs and scheduled tasks within the rule engine context.

## Key Responsibilities
- Schedules, executes, and tracks jobs for rule nodes and services.
- Provides APIs for job lifecycle management.

## Usage Pattern
- Used by nodes/services that require background or periodic processing.

## Best Practices
- Use for recurring or long-running tasks to avoid blocking main processing threads.

## Pitfalls
- Poor job management can lead to resource leaks or missed executions.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
