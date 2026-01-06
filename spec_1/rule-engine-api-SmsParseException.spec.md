# ThingsBoard Rule Engine API: SmsParseException

## Purpose
Custom exception for SMS parsing errors in the rule engine context.

## Key Responsibilities
- Encapsulates error details for SMS message parsing failures.
- Used by SmsSender and SmsService implementations.

## Usage Pattern
- Thrown when SMS message content cannot be parsed or formatted.

## Best Practices
- Provide clear error messages for debugging and alerting.

## Pitfalls
- Swallowing exceptions can hide message format issues.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
