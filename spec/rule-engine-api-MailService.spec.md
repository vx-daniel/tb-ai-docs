# ThingsBoard Rule Engine API: MailService

## Purpose
Provides email sending capabilities for rule engine nodes and services.

## Key Responsibilities
- Sends emails based on node/service requests.
- Integrates with SMTP or other mail transport mechanisms.

## Usage Pattern
- Used by mail nodes and notification services to send alerts or reports.

## Best Practices
- Validate email addresses and content before sending.
- Handle sending failures and retries gracefully.

## Pitfalls
- Misconfigured mail service can cause notification loss.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
