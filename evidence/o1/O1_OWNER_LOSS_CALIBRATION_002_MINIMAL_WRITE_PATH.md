# O1 OWNER LOSS CALIBRATION 002 — MINIMAL WRITE PATH

Status: IMMUTABLE_SHADOW_SYNTHESIS

## Goal
Minimize extra GitHub control writes while retaining recoverability.

## Candidate path
Healthy case:
- owner required work checkpoints may double as liveness renewals if validity/cadence gate passes;
- successor READY doubles as fresh server-time observation;
- normal owner-mediated transfer remains unchanged.

Recovery case:
- if READY/next meaningful observation proves P+G non-renewal, candidate fresh-reads ownership and performs one CAS takeover;
- no dedicated high-frequency time probes;
- dedicated heartbeat only if required owner checkpoints cannot provide bounded liveness cadence.

## Expected write overhead
Best case adds zero steady-state heartbeat writes beyond already-required work/checkpoint and READY evidence. Recovery adds only the takeover write plus existing verification/evidence.

## Decision
This is preferable to unconditional dedicated heartbeat if future OWNER measurements validate bounded checkpoint cadence. It directly implements the simplicity/utilization objective and should be the first comparator tested.
