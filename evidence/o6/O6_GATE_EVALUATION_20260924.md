# O6 adverse-recovery gate evaluation — 2026-09-24

## Evidence admitted
- Exact-generation orphan-recovery deterministic adverse suite: 5/5 PASS, including stale shadow, open-epoch bypass rejection, conflicting CAS single-winner, and unreconstructable-authority fail-closed while scheduler remains enabled.
- Observed-state reconciliation deterministic suite: 4/4 PASS; prompt mismatch detected, missing bootstrap recovered from declared stable refs, WRITE_OK/STATE_OK did not imply WAKE_OK/WORK_OK, and nominal state caused zero unnecessary corrective writes.
- Live retrospective scheduler evidence: prior verified DTSTART 22:11:35 KST produced an actual automation execution at 22:11:41 KST (+6s). This invocation independently classified WAKE_OK from actual execution, created READY evidence, completed fresh-SHA gen26->27 ownership transfer, and resumed durable work; therefore WORK_OK is also directly observed. Duplicate authoritative side effects observed: 0.

## Scope limits
- Provider-specific probability/frequency of an accepted scheduler write followed by an absent wake remains UNKNOWN; one successful live wake cannot estimate a miss rate.
- Deterministic fixtures establish recovery semantics, not empirical provider failure frequency.
- O7 still requires repeated unchanged clean wakes, measured duty-cycle/idle-gap evidence where observable, simpler-comparator accounting, and a consolidated rejected-hypothesis ledger.

## Decision
O6_STAGE_GATE = PASS_WITH_PROVIDER_MISS_RATE_UNKNOWN.
The unknown provider miss rate is not a reason to keep O6 open because O6's required recovery behavior has been discriminated and the live WRITE_OK/STATE_OK -> later WAKE_OK/WORK_OK separation is now directly demonstrated. Carry the frequency uncertainty into O7 rather than inventing it.

Next: normal stage-transition handoff into O7 final convergence; do not strand gen27 ownership.
