# H-O8-SCHEDULER-WRITER-FENCE

Status: TESTABLE — B ARM ACTIVE
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
- DISTRIBUTED_SYSTEMS_FORMAL/ENGINEERING_PRIOR

## SUPPORTING PRIORS
1. Mer gen34 forensic evidence: durable manifest history shows a verified target changed from `15:01:31Z` to `14:51:15Z` within 4.724954 seconds while substantive ownership remained generation 34 with the gen34->35 epoch OPEN. This directly establishes a concurrent last-writer scheduler surface; it does not identify the second writer.
2. Mer already uses fresh-SHA generation CAS for authoritative shared-state ownership because concurrent contenders require exactly one winner.
3. Kubernetes official Lease documentation states that distributed systems use leases to lock shared resources/coordinate members and that Kubernetes uses Lease objects for leader election so one instance acts as leader while peers remain standby. Coordinated Leader Election further uses optimistic concurrency on the Lease resource so one candidate becomes leader.
4. GitHub official repository-contents documentation requires the current blob `sha` when replacing a file and warns that parallel conflicting content operations can conflict, supporting explicit versioned/serialized mutation rather than unconstrained concurrent writers.
5. Kleppmann's distributed-lock analysis shows why lease ownership alone is insufficient for correctness if a stale paused holder can resume: protected writes need monotonically increasing fencing tokens. Mer's generation is the fencing-token analogue for authoritative shared-state effects and must remain monotonic even if scheduler recovery policy changes.

## AUTHORITATIVE / TECHNICAL REFERENCES
- Kubernetes Leases: https://kubernetes.io/docs/concepts/architecture/leases/
- Kubernetes Coordinated Leader Election: https://kubernetes.io/docs/concepts/cluster-administration/coordinated-leader-election/
- Kubernetes Lease v1 API: https://kubernetes.io/docs/reference/kubernetes-api/coordination/lease-v1/
- GitHub REST repository contents: https://docs.github.com/en/rest/repos/contents
- Martin Kleppmann, distributed locking/fencing analysis: https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html
- Mer synthesis notes: `research/O8_WRITER_FENCE_EXTERNAL_PRIOR_ART_20260924.md`, `research/O8_WRITER_FENCE_FENCING_TOKEN_NOTE_20260924.md`.

## COUNTER PRIORS / KNOWN CONFLICTS
1. ALL_WAKES_PREARM was introduced as crash insurance: a SHADOW can secure a later wake before ownership transfer.
2. If the current owner dies before transfer and no already-armed future occurrence exists, forbidding all shadow writes could increase recovery latency.
3. The observed changed DTSTART does not by itself prove the overwrite was harmful; it may have been an intentional recovery mutation. Historical automation metadata lacks writer identity, which is why O8 added immutable write-intent/result attribution.
4. Kubernetes/GitHub primitives do not prove ChatGPT Automation dispatch semantics; they justify the coordination hypothesis, not the product-specific outcome.
5. Kubernetes has explicit lease expiry/renewal takeover semantics. Mer currently does not validate elapsed time alone as takeover authority. Do not import TTL takeover without a separate hypothesis/test.
6. The scheduler API does not expose Mer generation as a native fencing token. Therefore stale-writer rejection must be enforced before scheduler mutation; if owner-loss testing needs independent scheduler liveness, REVISE toward a separate durable scheduler-writer claim/epoch rather than weakening substantive generation fencing.

## TRANSLATION
- Lease holder / leader -> current generation ACTIVE_OWNER.
- Scheduler write -> mutation of the single canonical automation's DTSTART/RRULE state.
- Contender preparation -> SHADOW reads durable state and prepares READY evidence without scheduler mutation.
- Ownership acquisition -> fresh-SHA generation CAS; after success the new owner may write its one scheduler prearm.
- resourceVersion/blob SHA -> Mer's generation/current-SHA fencing analogue, not an assumed scheduler-native CAS.
- fencing token -> monotonically increasing Mer generation for authoritative shared-state side effects.

## NON-TRANSFERABLE ASSUMPTIONS
- Kubernetes/GitHub do not establish ChatGPT Automation dispatch timing or guarantee atomic scheduler CAS.
- Mer must empirically verify owner-only/new-owner-only scheduling against missed continuation and owner-loss recovery.
- External prior art does not convert a Mer hypothesis into Mer truth.

## DISCRIMINATING TEST
Hold offset=840s, recurrence, full-VEVENT absolute representation, prompt semantics, work units, and handoff semantics constant. Compare:
A. current ALL_WAKES_PREARM;
B. OWNER_OR_NEWLY_ACQUIRED_OWNER_ONLY, where a shadow performs no scheduler mutation unless/until it wins generation CAS.

For each sample capture immutable write intent/result, intended DTSTART, exact live readback, actual later WAKE_OK/WORK_OK, ownership generation, handoff result, scheduler write count, overwritten-target observations, missed continuation, idle/handoff gap, and recovery latency. Use deterministic oracle `research/O8_WRITER_FENCE_DETERMINISTIC_ASSERTIONS.md`. Run the mandatory owner-loss adverse case in `research/O8_WRITER_FENCE_OWNER_LOSS_ADVERSE_PROTOCOL.md` before promotion.

## CURRENT B EVIDENCE
- B01/gen35: immediate WRITE_OK+STATE_OK PASS; completed clean-sample status unresolved because later WAKE/stability was not established before the next legal transfer.
- B02/gen36: successor began SHADOW, performed zero scheduler writes, created READY bound to gen35->36, won fresh-SHA CAS, and only then performed one attributed owner full-VEVENT +840s write to `15:30:20Z`; separate live readback exact PASS and current WORK_OK PASS. Later WAKE_OK/stability remains pending. Original B02 intent omitted some attribution metadata; history was not rewritten, and an immutable addendum records the limitation. B03 must satisfy the full attribution schema before mutation.
- Owner-loss adverse protocol/template prepared but not executed.

## PROMOTION_GATE
KEEP B only if >=3 completed clean normal samples plus one owner-loss adverse sample show continuity/recovery no worse under tested conditions, scheduler write count/overwrite ambiguity materially lower, no duplicate authority, and stale predecessors fenced from authoritative effects.

## REJECTION / REVISION RULE
- REJECT owner-only fencing if a reproducible owner-loss case strands continuation that ALL_WAKES_PREARM recovers safely.
- REJECT any recovery that requires weakening monotonic generation fencing.
- REVISE to a durable scheduler-write claim/epoch if owner-only is too restrictive but unconstrained all-wake writes demonstrably collide.
- Do not infer writer identity from timing alone; require attributed intent/result evidence.
