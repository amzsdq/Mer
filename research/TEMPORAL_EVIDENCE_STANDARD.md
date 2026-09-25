# Mer Temporal Evidence Standard

Status: ACTIVE RESEARCH METHOD
Policy epoch: 2.2.19-CONTINUITY-AUTHORITY-SEPARATION

## Authority
Temporal ordering, elapsed work, overlap, recovery latency, wake latency and idle-gap claims require one coherent external/server clock per sample. Preferred authority is GitHub server `created_at`/commit timestamp for durable markers. When healthy GitHub START/END markers are unavailable, same-canonical automation metadata is an allowed failover only when both endpoints have defined semantics and come from that same clock domain. Model-written clock strings are metadata only.

## Duration
Primary: WORKED = END_MARKER.server_time - START_MARKER.server_time.
Allowed failover: WORKED = END.automation_server_time - START.automation_server_time, where START is the distinct wake's live-read same-canonical `last_run_time` and END is a final same-canonical scheduler mutation/readback `updated_at`. Never mix GitHub and automation timestamps inside one duration sample. If either endpoint is missing or its semantics are ambiguous, WORKED is UNCONFIRMED.

## Overlap
To claim invocation A and B overlapped, require external/server evidence satisfying A.START < B.START < A.END; or, while A.END is absent, require B.START followed by a later A.PROGRESS/END server timestamp proving A remained active after B started. Also prove distinct invocation identity and relevant canonical identity.

## Serialization/defer
One missed overlap sample does not prove a universal provider contract. Record exact target, predecessor active evidence, successor START absence/presence, last_run metadata and eventual successor timing. Promote only after reproducible Mer evidence or explicit provider documentation.

## Scheduler states
WRITE_OK, STATE_OK, WAKE_OK and WORK_OK are distinct. A scheduled target is not a wake; a wake is not resumed useful work. Continuity-only scheduler repair is not substantive owner authority.

## Retrospective audit
Historical results whose key temporal conclusion depends on model-written times are downgraded until external timestamps are recovered. If server chronology contradicts narrative clocks, preserve original artifacts and add correction rather than rewriting history.

## Unknowns
Do not interpolate missing intervals. Mark UNKNOWN/UNCONFIRMED and design the next discriminating measurement.
