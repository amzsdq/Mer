# O8 Scheduler Writer Fence Test Plan

Status: ACTIVE — B ARM SAMPLING
Primary hypothesis: `research/H-O8-SCHEDULER-WRITER-FENCE.md`.

## Activation rationale
Gen34 separated two observations: full-VEVENT absolute representation can achieve exact WRITE_OK+STATE_OK, while the canonical scheduler can later change without a matching attributed writer. Writer authority is therefore the active single primary variable while representation and other scheduler semantics stay fixed.

## A/B variable
A — ALL_WAKES_PREARM: every invocation may perform one scheduler prearm regardless of substantive ownership.
B — OWNER_OR_NEWLY_ACQUIRED_OWNER_ONLY: SHADOW reads/prepares/READY with zero scheduler mutation; only ACTIVE_OWNER or a successor after successful fresh-SHA generation CAS writes the canonical.

Hold constant: same canonical ID; full absolute VEVENT; RRULE:FREQ=HOURLY; exact_schedule; enabled=true; offset=840s; generation-CAS substantive authority; role-relative stop semantics; work admission; attribution instrumentation.

## Metrics
Scheduler write count; exact readback; later WAKE_OK/WORK_OK; target overwrite; duplicate authority; READY->CAS latency; idle/handoff gap; owner-loss recovery latency; stranded continuation.

## CLEAN normal sample
Minimum three CLEAN B samples before promotion. CLEAN is defined by `research/O8_WRITER_FENCE_DETERMINISTIC_ASSERTIONS.md`: WF-N0..N6 + WF-S1..S4 all PASS. In particular, complete attribution must exist before scheduler mutation, exact target must survive until qualifying later dispatch, and actual WAKE_OK/WORK_OK must be observed. Immediate WRITE_OK+STATE_OK is only provisional. Intentional target supersession or post-hoc attribution repair cannot count CLEAN.

## Current sequence
- B01/gen35: immediate WRITE_OK+STATE_OK only; clean later target wake/stability not proven => NOT_CLEAN.
- B02/gen36: SHADOW write=0; legal CAS; one owner prearm; immediate WRITE_OK+STATE_OK+WORK_OK. Target later intentionally superseded => SUPERSEDED_NOT_CLEAN.
- B03/gen37: SHADOW write=0; legal CAS `cb6a45b8d3efdc54a63eef5738a28f3fda00dc4c`; one owner prearm target `15:38:39Z`; immediate exact readback and current WORK_OK PASS. Pre-target stability observation at GitHub `15:29:15Z` found target intact. However original intent omitted several attribution-protocol fields and required a post-hoc immutable addendum, so WF-N0 fails and B03 cannot become CLEAN even if the later target dispatch succeeds. Its remaining value is mechanism/stability and same-canonical dispatch-topology evidence.
- B04+ must use `research/O8_CLEAN_B_SAMPLE_TEMPLATE.md` before mutation.

## Same-canonical dispatch observation
B03 target is also an observational diagnostic under `research/O8_SAME_CANONICAL_DISPATCH_SERIALITY_OBSERVATION.md`. It changes no B03 primary variable. Interpretation is precommitted in `research/O8_DISPATCH_TOPOLOGY_DECISION_TREE.md`.

## Adverse owner-loss sample — mandatory
Use `research/O8_OWNER_LOSS_ADVERSE_PROTOCOL.md`. Do not corrupt authority. SHADOW writes remain zero; explicit single-use transition plus fresh-SHA recovery CAS is required. Functional recovery can PASS while latency non-regression remains UNKNOWN if no directly comparable A-arm latency exists.

## Promotion decision
KEEP B only if >=3 CLEAN normal samples plus adverse functional/recovery assertions pass, continuity/recovery is not materially worse, scheduler writes/overwrite ambiguity decrease, and no duplicate authority appears. Otherwise KEEP A or REVISE to a durable scheduler-write claim/epoch.

## Evidence discipline
Every scheduler mutation follows `research/O8_SCHEDULER_WRITE_ATTRIBUTION_PROTOCOL.md`. A changed live DTSTART without matching durable intent is an instrumentation gap, not proof of writer identity.
