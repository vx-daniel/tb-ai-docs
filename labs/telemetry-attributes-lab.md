# Lab 3: Telemetry & Attributes Flow

## Goal

Save timeseries and attributes through a custom node using `RuleEngineTelemetryService`, then read/verify persisted values via DAO or UI.

## Prerequisites

- Access to rule chain editor and ability to add a custom node
- A test device capable of sending telemetry/attributes
- Permissions to read timeseries/attributes via API or Admin UI

## Starting Point

- A rule chain with your custom node connected to the input
- Decide which keys you will save as timeseries vs attributes

## Steps

1. Implement node logic:
   - Use `RuleEngineTelemetryService` to save timeseries for keys `temp`, `humidity`
   - Use `AttributesService` (via `TbContext`) to save attributes in appropriate scope (e.g., `SERVER_SCOPE`)
2. Send test data from the device:
   - Telemetry JSON containing `temp` and `humidity`
   - Optional attributes payload (e.g., `fwVersion`)
3. Verify persistence:
   - Read back timeseries for `temp`, `humidity` via DAO/API
   - Read back attributes from the expected scope

## Success Criteria

- Timeseries keys appear with correct values and timestamps
- Attributes are present in the chosen scope
- No errors in node execution logs during saves

## Example Outputs

- Timeseries query returns expected numeric values
- Attributes query returns expected key-value pairs

## References

- [spec/timeseries-and-attributes-requests.md](../spec/timeseries-and-attributes-requests.md)
- [spec/rule-engine-services-rpc-telemetry-notifications.md](../spec/rule-engine-services-rpc-telemetry-notifications.md)
- [spec/tb-context-and-services.md](../spec/tb-context-and-services.md)
