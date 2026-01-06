# ThingsBoard Rule Engine: Node Inventory

This file lists and categorizes all available rule engine nodes in the ThingsBoard codebase, with a brief description of each node's purpose.

## Action Nodes
- **TbLogNode**: Logs incoming messages, optionally using a script for transformation.
- **TbAssignToCustomerNode**: Assigns an entity to a customer.
- **TbUnassignFromCustomerNode**: Unassigns an entity from a customer.
- **TbCreateAlarmNode**: Creates a new alarm based on message content.
- **TbClearAlarmNode**: Clears an existing alarm.
- **TbCreateRelationNode**: Creates a relation between entities.
- **TbDeleteRelationNode**: Deletes a relation between entities.
- **TbDeviceStateNode**: Updates device state.
- **TbMsgCountNode**: Counts messages passing through the node.
- **TbSaveToCustomCassandraTableNode**: Saves data to a custom Cassandra table.

## Telemetry Nodes
- **TbMsgAttributesNode**: Saves or updates message attributes.
- **TbMsgDeleteAttributesNode**: Deletes message attributes.
- **TbMsgTimeseriesNode**: Saves timeseries data from messages.
- **TbCalculatedFieldsNode**: Calculates and saves derived fields.

## Filter Nodes
- **TbJsFilterNode**: Filters messages using a JavaScript condition.
- **TbMsgTypeFilterNode**: Filters messages by type.
- **TbOriginatorTypeFilterNode**: Filters by originator entity type.
- **TbCheckAlarmStatusNode**: Checks the status of an alarm.
- **TbCheckMessageNode**: Checks message content against a condition.
- **TbCheckRelationNode**: Checks for a relation between entities.
- **TbDeviceTypeSwitchNode**: Switches logic based on device type.
- **TbAssetTypeSwitchNode**: Switches logic based on asset type.
- **TbJsSwitchNode**: Switches logic using a JavaScript expression.
- **TbMsgTypeSwitchNode**: Switches logic based on message type.
- **TbOriginatorTypeSwitchNode**: Switches logic based on originator type.

## Utility/Async Nodes
- **SemaphoreWithTbMsgQueue**: Combines concurrency control with message queueing for async processing.
- **EntitiesFieldsAsyncLoader**: Loads entity fields asynchronously for use in rule nodes.

## Integration Nodes (examples, not exhaustive)
- **REST, MQTT, Kafka, RabbitMQ, Mail, SMS, Notification, AI, Edge, etc.**: Each protocol or integration has its own node(s) for sending/receiving data or triggering actions.

---
For detailed specs on each node, see the corresponding `rule-engine-Tb<NodeName>.spec.md` files or the source code in the `rule-engine-components` directory.
