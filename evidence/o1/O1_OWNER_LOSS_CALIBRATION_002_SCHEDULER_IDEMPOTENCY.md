# O1 OWNER LOSS CALIBRATION 002 — SCHEDULER IDEMPOTENCY REQUIREMENT

Status: IMMUTABLE_SHADOW_ANALYSIS

Because scheduler mutation is external to GitHub ownership CAS, recovery safety improves if competing/stale scheduler writes converge to the same intended state rather than creating distinct continuations.

For the current same-automation hourly recurrence, desirable property is idempotent update of the same automation identity, never creation of a replacement. This is already a hard invariant and materially limits duplicate-scheduler blast radius.

However, timing fields can still conflict. A stale writer could overwrite a newer intended DTSTART/strategy. Therefore owner-loss canary should compare live scheduler state after takeover and treat any stale overwrite as scheduler-writer conflict even if automation identity remains singular.
