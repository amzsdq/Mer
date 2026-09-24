# H-O8-SCHEDULER-WRITER-FENCE

Status: SOURCED_TESTABLE_CANDIDATE
Stage: O8_LONG_WAKE_USEFUL_WORK_CONTINUATION

## HYPOTHESIS_ID
H-O8-SCHEDULER-WRITER-FENCE

## CLAIM
Allowing every concurrent invocation to mutate the same canonical scheduler before substantive ownership resolution creates an unnecessary last-writer-wins race. A simpler scheduler-writer fence — current ACTIVE_OWNER writes, or a successor writes only after it successfully acquires the next generation — should preserve continuity while reducing schedule overwrite/collision risk.

## PRIMARY_VARIABLE
Scheduler write authority: ALL_WAKES_PREARM versus OWNER_OR_NEWLY_ACQUIRED_OWNER_ONLY.

## SOURCE_CLASS
- INTERNAL_EMPIRICAL
- AUTHORITATIVE_IMPLEMENTATION

## SUPPORTING_PRIORS
1. Current Mer live evidence: this gen34 invocation verified a future wake-start prearm, yet later live scheduler state showed a different earlier DTSTART while `control/ownership.json` still remained generation 34 OWNER_ACTIVE and the gen34->35 epoch remained OPEN. The scheduler is therefore observably a shared last-writer surface independent of substantive ownership.
2. Mer already uses generation/CAS single-owner fencing for authoritative shared-state side effects because concurrent writers require an explicit winner.
3. Kubernetes-style leader election and lease-holder patterns separate contenders from the active writer: contenders may prepare, but leader-only mutations avoid concurrent control-plane writers.
4. GitHub optimistic concurrency patterns similarly require a current version/SHA for authoritative replacement, illustrating the general single-writer/fenced-write principle.

## COUNTER_PRIORS / KNOWN CONFLICTS
1. The existing ALL_WAKES_PREARM rule was introduced as crash-insurance: a SHADOW can secure a later wake before ownership transfer.
2. If the current owner dies before transfer and no already-armed future occurrence exists, forbidding all shadow writes could increase recovery latency.
3. The observed changed DTSTART does not by itself prove harmful collision; another invocation may have intentionally applied a valid recovery rearm. Raw writer identity is not exposed by automation metadata.

## TRANSLATION
- Leader/lease holder -> current generation ACTIVE_OWNER.
- Scheduler write -> mutation of the single canonical automation's DTSTART/RRULE state.
- Contender preparation -> SHADOW reads durable state and prepares READY evidence without scheduler mutation.
- Ownership acquisition -> fresh-SHA generation CAS; after success the new owner may write its one scheduler prearm.

## NON_TRANSFERABLE_ASSUMPTIONS
- Kubernetes/GitHub do not establish ChatGPT Automation dispatch timing or guarantee atomic scheduler CAS.
- Mer must empirically verify that owner-only/new-owner-only scheduling does not create missed continuation after owner loss.

## DISCRIMINATING_TEST
Hold offset=840s, recurrence, prompt, work units, and handoff semantics constant. Compare:
A. current ALL_WAKES_PREARM;
B. OWNER_OR_NEWLY_ACQUIRED_OWNER_ONLY, where a shadow performs no scheduler mutation unless/until it wins the generation CAS.
For each sample capture intended DTSTART, live readback after every permitted write, actual later wake, ownership generation, handoff result, number of scheduler writes, overwritten-target observations, missed continuation, and recovery latency. Inject/observe one owner-loss adverse case before promotion.

## PROMOTION_GATE
KEEP B only if continuity/recovery is no worse under tested conditions and scheduler write count/overwrite ambiguity materially decreases. Prefer A if B increases missed-wake or owner-loss recovery risk without compensating reliability gain.

## REJECTION / REVISION RULE
- REJECT owner-only fencing if a reproducible owner-loss case strands continuation that ALL_WAKES_PREARM recovers safely.
- REVISE to a durable scheduler-write claim/epoch if owner-only is too restrictive but unconstrained all-wake writes demonstrably collide.
- Do not infer causality from one changed live DTSTART; capture writer-intent/readback evidence in the controlled sample.
