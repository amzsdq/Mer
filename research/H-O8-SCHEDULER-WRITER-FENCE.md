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

## SUPPORTING PRIORS
1. Mer gen34 forensic evidence: durable manifest history shows a verified target changed from `15:01:31Z` to `14:51:15Z` within 4.724954 seconds while substantive ownership remained generation 34 with the gen34->35 epoch OPEN. This directly establishes a concurrent last-writer scheduler surface; it does not identify the second writer.
2. Mer already uses fresh-SHA generation CAS for authoritative shared-state ownership because concurrent contenders require exactly one winner.
3. Kubernetes official Lease documentation states that distributed systems use leases to lock shared resources/coordinate members and that Kubernetes uses Lease objects for leader election so one instance acts as leader while peers remain standby. Coordinated Leader Election further uses optimistic concurrency on the Lease resource so one candidate becomes leader.
4. GitHub official repository-contents documentation requires the current blob `sha` when replacing a file and warns that parallel conflicting content operations can conflict, supporting explicit versioned/serialized mutation rather than unconstrained concurrent writers.

## AUTHORITATIVE REFERENCES
- Kubernetes Leases: https://kubernetes.io/docs/concepts/architecture/leases/
- Kubernetes Coordinated Leader Election: https://kubernetes.io/docs/concepts/cluster-administration/coordinated-leader-election/
- GitHub REST repository contents: https://docs.github.com/en/rest/repos/contents

## COUNTER PRIORS / KNOWN CONFLICTS
1. ALL_WAKES_PREARM was introduced as crash insurance: a SHADOW can secure a later wake before ownership transfer.
2. If the current owner dies before transfer and no already-armed future occurrence exists, forbidding all shadow writes could increase recovery latency.
3. The observed changed DTSTART does not by itself prove the overwrite was harmful; it may have been an intentional recovery mutation. Historical automation metadata lacks writer identity, which is why O8 added immutable write-intent/result attribution.
4. Kubernetes/GitHub primitives do not prove ChatGPT Automation dispatch semantics; they justify the coordination hypothesis, not the product-specific outcome.

## TRANSLATION
- Lease holder / leader -> current generation ACTIVE_OWNER.
- Scheduler write -> mutation of the single canonical automation's DTSTART/RRULE state.
- Contender preparation -> SHADOW reads durable state and prepares READY evidence without scheduler mutation.
- Ownership acquisition -> fresh-SHA generation CAS; after success the new owner may write its one scheduler prearm.
- resourceVersion/blob SHA -> Mer's generation/current-SHA fencing analogue, not an assumed scheduler-native CAS.

## NON-TRANSFERABLE ASSUMPTIONS
- Kubernetes/GitHub do not establish ChatGPT Automation dispatch timing or guarantee atomic scheduler CAS.
- Mer must empirically verify owner-only/new-owner-only scheduling against missed continuation and owner-loss recovery.

## DISCRIMINATING TEST
Hold offset=840s, recurrence, full-VEVENT absolute representation, prompt semantics, work units, and handoff semantics constant. Compare:
A. current ALL_WAKES_PREARM;
B. OWNER_OR_NEWLY_ACQUIRED_OWNER_ONLY, where a shadow performs no scheduler mutation unless/until it wins generation CAS.

For each sample capture immutable write intent/result, intended DTSTART, exact live readback, actual later WAKE_OK/WORK_OK, ownership generation, handoff result, scheduler write count, overwritten-target observations, missed continuation, idle/handoff gap, and recovery latency. Run the mandatory owner-loss adverse case in `research/O8_SCHEDULER_WRITER_FENCE_TEST_PLAN.md` before promotion.

## PROMOTION_GATE
KEEP B only if continuity/recovery is no worse under tested conditions and scheduler write count/overwrite ambiguity materially decreases. Prefer A if B increases missed-wake or owner-loss recovery risk without compensating reliability gain.

## REJECTION / REVISION RULE
- REJECT owner-only fencing if a reproducible owner-loss case strands continuation that ALL_WAKES_PREARM recovers safely.
- REVISE to a durable scheduler-write claim/epoch if owner-only is too restrictive but unconstrained all-wake writes demonstrably collide.
- Do not infer writer identity from timing alone; require attributed intent/result evidence.
