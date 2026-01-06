# ThingsBoard Rule Engine API: SmsService

## Purpose
Provides SMS sending capabilities for rule engine nodes and services.

## Key Responsibilities
- Sends SMS messages based on node/service requests.
- Integrates with SMS gateways and providers.

## Usage Pattern
- Used by SMS nodes and notification services to send alerts or codes.

## Best Practices
- Validate phone numbers and message content before sending.
- Handle sending failures and retries gracefully.

## Pitfalls
- Misconfigured SMS service can cause notification loss.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
