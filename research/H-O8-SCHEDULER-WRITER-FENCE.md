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
1. Mer gen34 forensic evidence: durable manifest history shows a verified target changed from `15:01:31Z` to `14:51:15Z` within 4.724954 seconds while substantive ownership remained generation 34 with the gen34->35 epoch OPEN. This establishes a concurrent last-writer scheduler surface but does not identify the second writer.
2. Mer uses fresh-SHA generation CAS for authoritative shared-state ownership because concurrent contenders require exactly one winner.
3. Kubernetes Lease/leader-election mechanisms coordinate shared leadership so one instance acts while peers remain standby; optimistic concurrency selects the leader.
4. GitHub repository-content replacement requires the current blob SHA and conflicting parallel writes can conflict, supporting versioned/serialized mutation.
5. Fencing-token prior art motivates monotonically increasing authority tokens so a stale holder cannot resume protected writes. Mer generation is the analogue for authoritative shared-state effects.

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
3. A changed DTSTART does not prove a harmful overwrite; historical metadata lacks writer identity, hence immutable write attribution.
4. External coordination primitives do not prove ChatGPT Automation dispatch semantics.
5. Mer does not validate elapsed time alone as takeover authority; TTL takeover is out of scope.
6. The scheduler API has no native Mer-generation CAS, so stale-writer rejection is enforced before scheduler mutation. If owner-loss evidence shows this is too restrictive, revise toward a durable scheduler-writer claim/epoch rather than weakening substantive fencing.

## TRANSLATION
- leader -> current generation ACTIVE_OWNER;
- scheduler write -> canonical DTSTART/RRULE mutation;
- contender preparation -> SHADOW reads/prepares READY with zero scheduler mutation;
- ownership acquisition -> fresh-SHA generation CAS;
- fencing token -> monotonically increasing Mer generation.

## NON-TRANSFERABLE ASSUMPTIONS
External systems do not establish ChatGPT dispatch timing or atomic scheduler CAS. Owner-only scheduling must be empirically verified against missed continuation and owner-loss recovery.

## DISCRIMINATING TEST
Hold offset=840s, recurrence, full-VEVENT absolute representation, prompt semantics, work units, and handoff semantics constant. Compare A=ALL_WAKES_PREARM versus B=OWNER_OR_NEWLY_ACQUIRED_OWNER_ONLY. Capture attributed intent/result, exact live readback, later WAKE_OK/WORK_OK, ownership generation, handoff result, scheduler write count, target overwrite, missed continuation, idle/handoff gap, and recovery latency.

A completed clean normal B sample requires all of: SHADOW scheduler writes=0; legal fresh-SHA ownership CAS; exactly one attributed owner prearm; exact immediate STATE_OK; intended target remains the authoritative target until actual dispatch or an independently justified later owner transition; actual later WAKE_OK and WORK_OK; no duplicate authority. An intentionally superseded target is useful mechanism evidence but is not a clean normal sample.

Mandatory owner-loss adverse test is `research/O8_OWNER_LOSS_ADVERSE_PROTOCOL.md` and must use a single-use evidence-backed transition without corrupting authority.

## CURRENT B EVIDENCE
- B01/gen35: WRITE_OK+STATE_OK PASS; clean later target wake/stability not proven; clean-count=0.
- B02/gen36: SHADOW write=0, legal CAS, one owner prearm, WRITE_OK+STATE_OK+WORK_OK PASS; target later intentionally superseded before clean target WAKE qualification; clean-count remains 0.
- B03/gen37: START `15:24:20Z`; SHADOW write=0; READY bound to gen36->37; fresh-SHA CAS `cb6a45b8d3efdc54a63eef5738a28f3fda00dc4c` at `15:24:39Z`; one owner prearm to `15:38:39Z`; separate live readback exact PASS; current WORK_OK PASS; clean status PENDING actual target stability/WAKE_OK.
- Controlled owner-loss adverse protocol frozen for gen37->38 and not yet executed.

## PROMOTION_GATE
KEEP B only if >=3 completed clean normal samples plus one owner-loss adverse PASS show continuity/recovery no worse under tested conditions, scheduler write count/overwrite ambiguity materially lower, no duplicate authority, and stale predecessors fenced.

## REJECTION / REVISION RULE
- REJECT owner-only fencing if reproducible owner loss strands continuation that A recovers safely.
- REJECT recovery that weakens monotonic generation fencing.
- REVISE to durable scheduler-write claim/epoch if owner-only is too restrictive but all-wake writes collide.
- Never infer writer identity from timing alone.
