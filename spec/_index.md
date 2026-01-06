# ThingsBoard Codebase Specs

This index links detailed, developer-focused specs explaining core modules and flows in the ThingsBoard codebase (server-side Java).

## Rule Engine

- [Rule Engine Core Interfaces](rule-engine-core-interfaces.md)
- [TbContext & Services](tb-context-and-services.md)
- [Messaging Model (TbMsg & Metadata)](tbmsg-and-metadata.md)
- [Rule Engine Queue & Actors](rule-engine-queue-and-actors.md)
- [Rule Engine Services (RPC, Telemetry, Notifications)](rule-engine-services-rpc-telemetry-notifications.md)
- [Rule Chain JSON Templates](rule-chain-json-templates.md)
- [RuleNode UI Integration](rule-node-ui-integration.md)
- [Script Engine API](script-engine.md)

## Transport Layer

- [Transport → Rule Engine Flow](transport-to-rule-engine-flow.md)
- [MQTT Transport Flow](mqtt-transport-flow.md)
- [HTTP Transport Flow](http-transport-flow.md)
- [CoAP Transport Flow](coap-transport-flow.md)

## Device & Asset Management

- [Device RPC (Request/Response)](device-rpc.md)
- [Device State Management](device-state-management.md)
- [Device and Asset Profiles](device-asset-profiles.md)
- [OTA Updates](ota-updates.md)
- [Timeseries & Attributes Requests](timeseries-and-attributes-requests.md)

## Data & Persistence

- [DAO & Entity Services Overview](dao-entity-services-overview.md)
- [Entity Query API](entity-query.md)
- [Relations and Edge Sync](relations-and-edge-sync.md)

## Notifications & Messaging

- [Notification Service](notification-service.md)
- [Mail Service](mail-service.md)
- [SMS Service](sms-service.md)
- [Alarm Service](alarm-service.md)

## Security & Multi-Tenancy

- [Security and Authentication](security-auth.md)
- [Tenant and Customer Model](tenant-customer-model.md)
- [API Usage State](api-usage-state.md)
- [Audit Logging](audit-logging.md)

## UI & Real-Time

- [Dashboard and Widgets](dashboard-widgets.md)
- [WebSocket Subscriptions](websocket-subscriptions.md)

## Infrastructure

- [Actor Internals Deep Dive](actor-internals-deep-dive.md)
- [Queue and Partitioning](queue-and-partitioning.md)
- [Job Manager](job-manager.md)
- [Edge Integration](edge-integration.md)

## Labs (Hands-on)

Looking for practical, step-by-step exercises? Start here:

- [Labs Index](../labs/_index.md)
