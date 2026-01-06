# ThingsBoard Rule Engine API: SmsSendException

## Purpose
Custom exception for SMS sending failures in the rule engine context.

## Key Responsibilities
- Encapsulates error details for SMS send operation failures.
- Used by SmsSender and SmsService implementations.

## Usage Pattern
- Thrown when SMS sending fails due to provider, network, or configuration issues.

## Best Practices
- Provide clear error messages for debugging and alerting.

## Pitfalls
- Swallowing exceptions can hide SMS delivery problems.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
