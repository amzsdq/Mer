# O8 Dispatch Topology Decision Tree

Status: PRECOMMITTED INTERPRETATION RULE
Purpose: prevent post-hoc architecture changes after the B03 same-canonical dispatch observation.

## Observation input
Use only Mer-side durable evidence from `research/O8_SAME_CANONICAL_DISPATCH_SERIALITY_OBSERVATION.md`. Evidence from BDO/workwork/tEST may motivate but cannot decide Mer truth without explicit equivalence.

## Case 1 — OVERLAP_OBSERVED
A distinct successor START occurs after intended target while predecessor has no prior END.
- Keep concurrent handoff topology testable.
- Continue writer-fence B: SHADOW zero-write, READY, fresh-SHA CAS, post-CAS owner prearm.
- Measure READY->CAS and overlap/handoff gap.
- Do not infer that overlap always occurs; reliability remains bounded to samples.

## Case 2 — SERIALIZED_OR_DEFERRED_CANDIDATE
Target passes while predecessor remains active, with no distinct successor START and no qualifying last_run advance.
- Mark overlap-dependent handoff as a suspect assumption.
- Do not keep shortening prearm to force overlap; that would repeat an unchanged failed mechanism.
- Next discriminating hypothesis becomes serialized same-canonical continuation: long useful owner invocation -> PRE_CLOSE/END-relative short rearm -> verified next wake.
- Preserve hourly recurrence as crash fallback.
- Compare actual idle gap and useful-work duty cycle against overlap design rather than optimizing nominal DTSTART alone.
- Require >=2 Mer reproductions before promoting provider seriality from candidate to operating assumption unless one run has explicit platform metadata proving serialization.

## Case 3 — DISPATCH_AFTER_OWNER_END
Distinct successor appears only after predecessor END.
- Treat same-canonical overlap as unsupported for that sample.
- Prefer testing serialized close-relative rearm next.
- Writer-fence may simplify: there is no simultaneous SHADOW scheduler writer if provider serializes the canonical, but substantive generation fencing remains useful for stale/recovery cases.

## Case 4 — INDETERMINATE
- Preserve current candidate without promotion.
- Fix instrumentation or run a clean observation; do not interpret absence of evidence as serialization.

## Simplicity rule
If serialized same-canonical continuation matches or improves measured continuity/recovery and duty cycle, prefer it over an overlap/handoff state machine because it removes READY/CAS overlap choreography from the normal path. Keep generation fencing only where it protects real stale/recovery concurrency.

## Failure rule
If serialized continuation materially increases idle gaps or recovery loss and concurrent overlap is directly reproducible, retain the more complex overlap mechanism with owner-fenced scheduler writes.


## S01 direct follow-up
Serialized S01 produced a distinct successor after predecessor END. GitHub server chronology: predecessor END `15:41:51Z`, intended target `15:43:21Z`, successor START `15:43:48Z`. This is Case 3 evidence for S01. It is one clean serialized sample, not yet a universal provider contract. Continue S02/S03 before promotion.
