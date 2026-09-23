# O1 OWNER LOSS CALIBRATION 002 — HYPOTHESIS UPDATE

Status: IMMUTABLE_SHADOW_EVIDENCE
Hypothesis: H-O1-OWNER-LOSS-RECOVERY
Verdict: KEEP_REVISED

## Revised claim
Bounded server-observed non-renewal plus generation CAS remains a viable owner-loss fallback, but the renewal source should not be assumed to require dedicated heartbeats. First test whether already-required OWNER checkpoints provide a bounded valid liveness cadence; use dedicated renewal only if they do not.

## Why revised
- arbitrary write latency and hourly wake spacing were shown to be wrong calibration variables;
- SHADOW useful-work writes can be frequent but cannot renew OWNER liveness;
- piggybacking may remove steady-state control writes if OWNER checkpoint cadence is bounded;
- cross-system scheduler fencing remains an unresolved acceptance risk.

## Not promoted
No expiry threshold, renewal cadence, takeover, or scheduler protocol is promoted by this wake. Authoritative program state remains untouched because current invocation is SHADOW.
