# ThingsBoard Rule Engine API: SlackService

## Purpose
Provides integration with Slack for sending notifications and messages from the rule engine.

## Key Responsibilities
- Sends messages to Slack channels or users.
- Used by notification nodes and services.

## Usage Pattern
- Used by nodes/services to send Slack notifications based on events or rules.

## Best Practices
- Validate channel/user IDs and message content before sending.
- Handle API rate limits and errors gracefully.

## Pitfalls
- Invalid credentials or channel IDs can cause notification loss.

---
See also: [rule-engine-nodes-apis.spec.md](rule-engine-nodes-apis.spec.md)
