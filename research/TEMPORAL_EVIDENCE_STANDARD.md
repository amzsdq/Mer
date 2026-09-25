# Mer Temporal Evidence Standard

Status: ACTIVE RESEARCH METHOD

## Authority
Temporal ordering, elapsed work, overlap, recovery latency, wake latency and idle-gap claims require one coherent external/server clock per sample. Preferred authority is GitHub server `created_at`/commit timestamp for durable markers when that marker path is healthy. If GitHub marker mutation is blocked or unavailable, the same-canonical automation server clock is an admitted failover: START is the live `last_run_time` for the distinct wake and END is the server `updated_at` from the final verified scheduler mutation. Model-written clock strings are metadata only.

## Clock-source isolation
A duration sample MUST use exactly one clock source. Never combine a GitHub-marker START with an automation-server END, or vice versa. Cross-source comparisons require explicit calibration and are not assumed equivalent.

## Duration
Under GitHub-marker authority, WORKED = END_MARKER.server_time - START_MARKER.server_time. Under automation-server failover, WORKED = final verified scheduler mutation `updated_at` - distinct-wake live `last_run_time`. If either endpoint required by the selected source is missing, WORKED is UNCONFIRMED; do not substitute narrative timestamps.

## Overlap
To claim invocation A and B overlapped, require external/server evidence satisfying A.START < B.START < A.END; or, while A.END is absent, require B.START followed by a later A.PROGRESS/END server timestamp proving A remained active after B started. Also prove distinct invocation identity and relevant canonical identity.

## Serialization/defer
One missed overlap sample does not prove a universal provider contract. Record exact target, predecessor active evidence, successor START absence/presence, last-run metadata and eventual successor timing. Repeated authority-valid target-passage reproductions with no target-compatible successor may classify concurrent same-canonical overlap as CONSTRAINED without claiming universal provider impossibility.

## Retrospective audit
Historical results whose key temporal conclusion depends on model-written times are downgraded until external timestamps are recovered. If server chronology contradicts narrative clocks, the narrative temporal conclusion is invalidated; preserve original artifacts and add correction rather than rewriting history.

## Scheduler states
WRITE_OK, STATE_OK, WAKE_OK and WORK_OK are distinct. A scheduled target is not a wake; a wake is not resumed useful work.

## Unknowns
Do not interpolate missing intervals. Mark UNKNOWN/UNCONFIRMED and design the next discriminating measurement.
