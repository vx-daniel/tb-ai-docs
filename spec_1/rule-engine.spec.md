# ThingsBoard Module: rule-engine

## Language and Context
- **Language:** Java
- **Context:** Event-driven processing engine for IoT data, supporting custom rules, message queues, and async processing.

## Structure and Key Components
- **Rule Nodes:** Modular processing units for filtering, transforming, and acting on messages.
- **Message Queues:** Asynchronous message passing using Guava `ListenableFuture` and custom queue classes.
- **Utilities:** Helpers for async entity loading, callback management, and queue control.

## Example: SemaphoreWithTbMsgQueue
- **Purpose:** Combines semaphore (concurrency control) with a message queue for rule node processing.
- **Logic Flow:**
  1. Messages are enqueued and processed with concurrency limits.
  2. Callbacks handle success/failure of each message.

## Complex Concepts
- **Asynchronous Message Processing:** Ensures high throughput and scalability.
- **Strategy Pattern:** Used for observation and processing strategies.

## Best Practices
- Use modular rule nodes for extensibility.
- Monitor queue sizes and processing times.

## Common Pitfalls
- Queue backpressure can cause memory issues if not managed.
- Complex async flows can be hard to debug.

## Recommendations
- Add metrics for queue health.
- Document custom rule node development.

---
See also: [thingsboard-architecture.spec.md](thingsboard-architecture.spec.md)
