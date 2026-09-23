# O1 OWNER LOSS CALIBRATION 002 — FAULT INJECTION PLAN

Status: PREPARED_NOT_AUTHORIZED

## Faults to inject separately
F1 Owner stops renewing before READY consumption.
F2 Owner resumes after candidate takeover.
F3 Two SHADOW candidates become eligible simultaneously.
F4 GitHub CAS conflict on ownership update.
F5 Scheduler mutation attempt from stale owner after takeover.
F6 Missing/ambiguous server timestamp evidence.

## Isolation rule
Do not combine F1–F6 into one first sample. Establish basic F1 recovery first, then add one race/fault at a time. Compound races are reserved for stabilization after base mechanism passes.

## Expected model updates
- F1 failure to recover => REVISE liveness mechanism.
- F2 duplicate authoritative effect => REJECT safety model.
- F3 multiple winners => REJECT CAS/authority model.
- F4 retry unchanged without diagnosis => prohibited; classify conflict path.
- F5 scheduler conflict => REVISE cross-system fencing.
- F6 takeover despite ambiguity => REJECT fail-closed behavior.
