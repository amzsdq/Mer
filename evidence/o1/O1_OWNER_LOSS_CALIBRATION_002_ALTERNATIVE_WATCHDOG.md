# O1 OWNER LOSS CALIBRATION 002 — ALTERNATIVE WATCHDOG HYPOTHESIS

Status: CANDIDATE_NOT_PROMOTED

## Competing idea
Instead of owner heartbeat writes, a separate already-running wake/watchdog could judge staleness from existing durable owner progress markers and authorize recovery.

## Potential advantage
No extra heartbeat write cadence if useful work already emits sufficiently frequent server-timestamped durable checkpoints.

## Problem
Useful work/checkpoint cadence is workload-dependent. Silence may mean a long unit rather than owner death. Reusing arbitrary progress commits as liveness renewals silently couples failure detection to workload structure and can create false takeover.

## Testable refinement
If every bounded useful unit already persists a server-timestamped checkpoint and the maximum legitimate unit duration is itself bounded/observable, those checkpoints could double as renewal evidence. This may remove dedicated heartbeat writes.

## Decision
Do not adopt yet. Preserve as a lower-overhead competing hypothesis. During calibration, record whether normal required evidence writes are frequent and regular enough to satisfy the renewal model without additional writes. If yes, prefer piggybacked renewal over dedicated heartbeat because reliability/utilization equivalent with less control overhead.
