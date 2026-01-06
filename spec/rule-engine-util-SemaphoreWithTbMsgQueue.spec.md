# ThingsBoard Rule Engine Utility: SemaphoreWithTbMsgQueue

## Purpose
Combines a semaphore (for concurrency control) with a message queue, enabling controlled parallel processing of messages in rule nodes.

## Key Responsibilities
- Limits the number of concurrent message processing tasks.
- Queues excess messages for later processing.

## Usage Pattern
- Used in async rule nodes to prevent resource exhaustion.

## Best Practices
- Tune semaphore limits based on system capacity.
- Monitor queue size to avoid bottlenecks.

## Pitfalls
- Too small a limit can reduce throughput; too large can exhaust resources.

---
See also: [rule-engine-nodes-inventory.spec.md](rule-engine-nodes-inventory.spec.md)
