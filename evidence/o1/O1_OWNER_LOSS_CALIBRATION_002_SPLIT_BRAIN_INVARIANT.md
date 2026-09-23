# O1 OWNER LOSS CALIBRATION 002 — SPLIT-BRAIN INVARIANT

Status: IMMUTABLE_SHADOW_ANALYSIS

Owner-loss recovery cannot guarantee that only one process is physically executing. It must guarantee that only one generation is accepted for authoritative effects.

Operational invariant candidate:
`physical_concurrency MAY exceed 1; authoritative_generation MUST equal exactly one durable ownership generation at mutation time.`

The overlap design intentionally permits physical concurrency (OWNER + SHADOW). Recovery may transiently produce new OWNER + stale predecessor. This is acceptable only if stale generations are mechanically denied authoritative effects.

Therefore 'two invocations alive' is not itself a failure. Duplicate authoritative effects or scheduler writer conflicts are failures.
