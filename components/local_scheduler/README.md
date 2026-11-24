# Local Scheduler v0.2.0 design

This branch contains the design for an ESPHome external component. The Python schema and C++ runtime are pending.

- Bind scheduler outputs to user-supplied ESPHome switches and use a configured ESPHome time source. Keep AOFO hardware mappings outside the component.
- Store and validate schedules locally. Reconcile the current 15-minute slot after boot, clock synchronization or correction, schedule edits, and override expiry. Do not replay missed events.
- Continue with a valid device clock during network outages. Review ESPHome Wi-Fi and API reboot timeouts for the offline bench fixture. When time is invalid, apply per-output KEEP, ON, or OFF fallback after the output becomes ready. KEEP cancels pending scheduler writes.
- Track manual overrides independently per output for 45 elapsed minutes using a monotonic clock. Define overrides as volatile across reboots for the first implementation. Use explicit command provenance where available. Treat ambiguous Tuya reports as observations and test delayed acknowledgments separately.
- Isolate Tuya confirmation handling behind an output adapter. Allow one in-flight command per output and confirm against a device report where supported. Use a timeout, bounded retries, and a fault after exhaustion. Supersede stale targets after slot, fallback, or override changes. Avoid retry bursts and suppress scheduler writes during override.
- Keep local controls independent of remote assets. Publish diagnostics upon meaningful state changes and retain the data needed by the local UI.

See the [verification plan](../../tests/README.md) for acceptance cases and [ESPHome External Components](https://esphome.io/components/external_components/) for packaging conventions.
