# O1 OWNER LOSS CALIBRATION 002 — CONTROL OVERHEAD OBSERVATION

Status: IMMUTABLE_SHADOW_EVIDENCE

This wake deliberately continued useful SHADOW work instead of closing after the first calibration result. The produced units were distinct: cross-wake measurement, expiry semantics, simpler comparator, canary spec, failure-detector/safety split, renewal schema, overhead model, piggyback comparator, decision matrix, cadence sample, threshold guardrails, stale-owner race, scheduler fence risk, acceptance checklist, and owner-admissible next transition.

This demonstrates the intended duration-gate behavior qualitatively: waiting for a future owner/wake did not force early termination. However, the 600-second execution gate is evaluated only from server timestamps and has not yet been reached at this marker.

The evidence volume also reveals a cost: forcing many tiny immutable files is itself control overhead. Future wakes should prefer larger bounded useful units where possible, while still producing enough server-observable boundaries to enforce duration and liveness. Duration enforcement must not incentivize fragmentation.
