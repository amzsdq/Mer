# O1 OWNER LOSS CALIBRATION 002 — SCHEDULER FENCE RISK

Status: IMMUTABLE_SHADOW_ANALYSIS

## Finding
GitHub generation CAS can serialize ownership-record changes, but the external automation scheduler is a separate state domain. A stale invocation could theoretically fresh-read ownership, then lose ownership, then mutate scheduler before observing the new generation. This is a cross-system TOCTOU window.

## Impact
The lease/CAS recovery hypothesis is not sufficient by itself to prove zero scheduler-writer conflicts. Promotion gate explicitly requires zero conflicts, so the canary must exercise this race rather than assume it away.

## Candidate mitigations to test later
- owner-mediated scheduler mutation only on healthy path;
- after takeover, successor verifies live scheduler state before relying on it;
- minimize interval between fresh ownership check and scheduler mutation;
- make scheduler writes idempotent/convergent where possible;
- if available, carry expected generation into a durable scheduler-intent record and reconcile rather than issuing unconstrained direct writes.

No mitigation is promoted here because changing scheduler protocol would introduce another primary variable. This risk is recorded for the recovery canary acceptance criteria.
