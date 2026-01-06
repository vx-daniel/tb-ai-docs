# ThingsBoard Rule Engine API: SmsException

## Purpose
Custom exception for SMS sending errors in the rule engine context.

## Key Responsibilities
- Encapsulates error details for SMS failures.
- Used by SmsSender and SmsService implementations.

## Usage Pattern
- Thrown when SMS sending fails due to provider or network issues.

## Best Practices
- Provide clear error messages for debugging and alerting.

## Pitfalls
- Swallowing exceptions can hide SMS delivery problems.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
