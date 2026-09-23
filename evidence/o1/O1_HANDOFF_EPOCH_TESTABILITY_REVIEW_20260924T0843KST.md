# O1 handoff-epoch testability review

## Result
REVISE, not yet TESTABLE under current authoritative execution.

## Current-state conflict
- status/program.json still names H-O1-OWNER-LOSS-RECOVERY and expiry calibration as next_step.
- spec/execution.json generation 12 still requires >=5 genuine owner renewal gaps before expired-owner CAS.
- control/ownership.json generation 2 still names a terminated invocation as OWNER_ACTIVE.
- Therefore this SHADOW cannot open an authoritative handoff epoch, change the experiment, or run the proposed first-READY CAS without violating the current owner/generation fence.

## Candidate refinement
Separate graceful handoff intent from failure detection:
1. Current OWNER alone may open `handoff_epoch={generation:G,state:OPEN}` as an authoritative side effect.
2. A contender is admissible only after verified wake-start pre-arm, durable checkpoint reconstruction, and immutable READY evidence bound to G/epoch.
3. Admissible contenders fresh-read the epoch/ownership SHA and attempt exactly one CAS G->G+1. One succeeds; losers become stale SHADOWs.
4. Winner immediately fresh-reads and must pass generation fencing before any authoritative side effect.
5. Epoch CLOSED/absent rejects ordinary takeover.
6. Dead-owner recovery remains a separate lease/failure-detector experiment; an ordinary handoff epoch must not silently become a dead-owner detector.

## Prior-art translation
Kubernetes Lease/LeaseCandidate separates candidate identity/readiness from the shared Lease and uses optimistic concurrency so only one candidate acquires leadership. Kubernetes also treats expiry/renewal as a separate liveness mechanism. etcd election similarly binds leadership to a lease and exposes a revision that can be tested transactionally for ownership. Translation to Mer: keep a monotonic generation/fresh-read fence and use CAS to serialize contenders; do not infer liveness from CAS itself.

## Counter-prior / non-transferable assumption
Kubernetes/etcd have continuously running processes and server-backed lease primitives. ChatGPT Automation invocations are ephemeral and Mer currently has no validated per-invocation successor channel or calibrated lease. Therefore their timeout values and liveness semantics are not transferable.

## Discriminating test once authoritative revision is legal
Primary variable: OWNER_OPENED_HANDOFF_EPOCH_WITH_FIRST_READY_CAS.
Sequence under unchanged candidate:
A. one READY contender -> exactly one G->G+1 transfer;
B. two READY contenders -> exactly one winner, one stale loser;
C. non-READY contender -> rejected;
D. CLOSED/absent epoch -> rejected;
E. predecessor stale after transfer -> fresh-read generation fence rejects side effect.
Promotion requires all five without duplicate authoritative side effects or scheduler conflicts. Any unauthorized acquisition REJECTS the candidate.

## Immediate decision
Do not mutate status/program.json, spec/execution.json, or control/ownership.json from this SHADOW. The existing calibration path is structurally stalled, but replacing it requires an explicitly authorized authoritative transition or a validated dead-owner recovery primitive. Continue immutable evidence only until that transition is legal.
