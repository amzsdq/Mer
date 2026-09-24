# O8 Writer-Fence Owner-Loss Adverse Protocol

Status: PREPARED — NOT YET AUTHORIZED TO EXECUTE
Hypothesis: `H-O8-SCHEDULER-WRITER-FENCE`
Primary variable remains scheduler writer authority; do not change representation, recurrence, offset, or substantive generation fencing.

## Purpose
Test the strongest counterargument to owner-only scheduler writes: if the current owner disappears before graceful transfer, does forbidding SHADOW scheduler writes strand continuation or materially worsen recovery?

## Prior Mer constraint
O1 already diagnosed that graceful OWNER-opened handoff cannot recover an owner that is already gone. The minimal legal abnormal primitive was a program-authorized, single-use recovery epoch scoped to the exact stuck generation; elapsed time or contender readiness alone must not authorize takeover. O8 reuses that already-validated authority shape rather than inventing automatic expiry.

## Safety / non-corruption rule
Do not simulate loss by corrupting `control/ownership.json`, deleting state, disabling the canonical, or fabricating an END. Do not let a SHADOW write the scheduler. The adverse condition must be created by an explicit test authorization that says the current owner is intentionally withheld from further authoritative work for this one sample while its last verified recurring fallback remains intact.

## Preconditions
1. Current generation G is ACTIVE_OWNER with one exact verified future recurring fallback.
2. No unresolved scheduler write mismatch exists at sample start.
3. A next-generation handoff epoch is OPEN.
4. A durable single-use adverse-test authorization names exactly G, target G+1, the canonical automation ID, and the expected recovery route.
5. Baseline timestamps are captured: owner last progress, fallback target, successor wake/READY, recovery authorization, CAS, new-owner prearm, resumed work.

## Procedure
1. Freeze predecessor owner mutations by consuming the explicit adverse-test authorization; do not alter scheduler state.
2. Successor wakes as SHADOW and MUST perform zero scheduler writes.
3. Successor creates READY bound to the exact adverse recovery epoch and generation G.
4. Successor fresh-reads ownership and authorization, then attempts exactly one fresh-SHA CAS G->G+1 under the single-use recovery rule.
5. CAS loser fails closed. CAS winner becomes ACTIVE_OWNER.
6. Only the new owner performs one attributed full-VEVENT +840s scheduler write and exact live readback.
7. New owner resumes genuine work and records WORK_OK.
8. Mark the recovery authorization CONSUMED and open the next normal handoff epoch.

## PASS
All must hold:
- SHADOW scheduler writes = 0;
- exactly one recovery CAS winner;
- old generation fenced;
- new owner scheduler write = 1 and exact STATE_OK;
- actual resumed WORK_OK;
- no duplicate authoritative side effect;
- no stranded continuation;
- measured recovery latency is bounded and not materially worse than the relevant A-arm comparator without compensating overwrite-reliability benefit.

## FAIL / REVISE
- FAIL B if no legal recovery can occur before continuation is stranded.
- FAIL B if recovery requires a SHADOW scheduler write before authority acquisition.
- REVISE to a separate durable scheduler-writer claim/epoch if owner-only substantive authority is sound but scheduler liveness needs an independently recoverable writer lease.

## Evidence
Persist immutable authorization, READY, CAS result, scheduler intent/result, resumed-work marker, and recovery close. GitHub server timestamps are the measurement clock. Timing alone never attributes a scheduler writer.
