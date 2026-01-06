# ThingsBoard Rule Engine API: FirebaseService

## Purpose
Provides integration with Firebase for push notifications and messaging in the rule engine context.

## Key Responsibilities
- Sends push notifications via Firebase Cloud Messaging (FCM).
- Used by notification nodes and services.

## Usage Pattern
- Used by nodes/services to send push notifications to devices or users.

## Best Practices
- Validate device tokens and payloads before sending.
- Monitor delivery status and handle failures.

## Pitfalls
- Invalid tokens or payloads can cause notification loss.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
