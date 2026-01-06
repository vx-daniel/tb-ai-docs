# ThingsBoard Rule Engine API: SmsSender

## Purpose
Defines the contract for SMS sending implementations in the rule engine context.

## Key Responsibilities
- Sends SMS messages to specified recipients.
- Used by SmsService and notification nodes.

## Usage Pattern
- Implemented by classes that integrate with specific SMS providers.

## Best Practices
- Implement error handling and retries for SMS delivery.

## Pitfalls
- Not handling provider errors can cause message loss.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
