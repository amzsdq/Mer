# Mer Temporal Evidence Standard

Status: ACTIVE RESEARCH METHOD

## Authority
Temporal ordering, elapsed work, overlap, recovery latency, wake latency and idle-gap claims require an external/server clock. Preferred authority is GitHub server `created_at`/commit timestamp for durable markers. Automation metadata timestamps may support dispatch observations when their semantics are clear. Model-written clock strings are metadata only.

## Duration
WORKED = END_MARKER.server_time - START_MARKER.server_time. If either authoritative endpoint is missing, WORKED is UNCONFIRMED. Do not substitute narrative timestamps.

## Overlap
To claim invocation A and B overlapped, require external/server evidence satisfying A.START < B.START < A.END; or, while A.END is absent, require B.START followed by a later A.PROGRESS/END server timestamp proving A remained active after B started. Also prove distinct invocation identity and relevant canonical identity.

## Serialization/defer
One missed overlap sample does not prove a universal provider contract. Record exact target, predecessor active evidence, successor START absence/presence, last_run metadata and eventual successor timing. Promote only after reproducible Mer evidence or explicit provider documentation.

## Retrospective audit
Historical results whose key temporal conclusion depends on model-written times are downgraded until external timestamps are recovered. If server chronology contradicts narrative clocks, the narrative temporal conclusion is invalidated; preserve original artifacts and add correction rather than rewriting history.

## Scheduler states
WRITE_OK, STATE_OK, WAKE_OK and WORK_OK are distinct. A scheduled target is not a wake; a wake is not resumed useful work.

## Unknowns
Do not interpolate missing intervals. Mark UNKNOWN/UNCONFIRMED and design the next discriminating measurement.
