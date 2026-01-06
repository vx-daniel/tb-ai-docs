# ThingsBoard Rule Engine API: TbEmail

## Purpose
Encapsulates email message details for use in mail-related rule nodes and services.

## Key Responsibilities
- Holds email subject, body, recipients, and attachments.
- Used by mail nodes and services to send notifications.

## Usage Pattern
- Constructed and passed to mail service or node for sending emails.

## Best Practices
- Validate email addresses and content before sending.
- Handle sending failures gracefully.

## Pitfalls
- Invalid email data can cause delivery failures.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
