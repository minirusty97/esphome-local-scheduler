# Verification plan

These cases are planned. Run hardware cases on a bench device with an observable test load.

| Case | Expected result |
| --- | --- |
| Configuration validation | ESPHome accepts a complete, sanitized AOFO fixture and resolves its local JavaScript. |
| Normal scheduling | A daily schedule applies at 15-minute boundaries and through midnight. |
| Warm network outage | Scheduling continues while the device clock remains valid and the API and network are unavailable. |
| Cold boot without time | Each configured KEEP, ON, or OFF fallback is respected. KEEP issues no output command. |
| Time recovery | A valid clock triggers reconciliation to the current slot without replaying missed slots. |
| Manual change | Only the affected output stays overridden for 45 elapsed minutes, then returns to the current slot. |
| Tuya delayed readiness | Output control waits for the device to report readiness. |
| Tuya missing or late confirmation | Retries are bounded, stale requests are discarded, and successful reconciliation clears the fault. |

Use `tests/fixtures/` for synthetic inputs and `tests/bench/` for reproducible bench procedures and results.
