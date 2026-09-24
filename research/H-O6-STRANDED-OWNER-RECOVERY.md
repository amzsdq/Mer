# H-O6-STRANDED-OWNER-RECOVERY

STATUS: TESTABLE
STAGE: O6_ADVERSE_RECOVERY_TESTS

## Incident seed
O5 exposed a durable liveness defect: gen24 completed its final sample with the handoff epoch CONSUMED and no legal gen24->25 successor path. The next wake could prearm the scheduler but could not lawfully mutate authoritative state. A user-authorized exact-generation single-use recovery moved authority gen24->25, after which gen25 opened a normal gen25->26 stage-entry epoch.

## Internal evidence
- control/ownership.json history: stranded gen24, exact-generation recovery to gen25, then normal OPEN gen25->26.
- evidence/o5/O5_MATCHED_WINDOW_EVALUATION_20260924.md records the defect as independent of the A/B comparator.
- Kernel 2.2.10 now contains OWNER_EXIT_GUARD, SCHEDULER_NONSTOP_GUARD, PREARM_LIVE_VERIFY, and EARLY_EXIT_CORRECTIVE_REARM.

## External implementation references and contrary evidence
1. Amazon Builders' Library, "Leader election in distributed systems": leader election/leases simplify coordination but introduce failure modes; systems must ensure a leader acts only while holding authority, keep an audit trail of leadership changes, make work durable before announcing completion, and use idempotency/optimistic locking to tolerate ambiguous predecessor completion. It also warns that exactly-one-leader assumptions are difficult under failures.
   Reference: https://aws.amazon.com/builders-library/leader-election-in-distributed-systems/
2. Kubernetes Lease API: HA control-plane components use Lease objects so one instance is active while others stand by; liveness/leadership is represented durably rather than inferred from process existence alone.
   Reference: https://kubernetes.io/docs/concepts/architecture/leases/

Contrary/competing interpretation: automatically repairing every missing successor path can conceal a genuinely still-live predecessor or corrupted authority record. Therefore recovery must not be a generic "if stuck then advance generation" rule. It must require a reconstructable exact source generation/owner, a nonterminal program, no legal OPEN successor epoch, and a fresh-SHA fenced transition. Ambiguous authority remains fail-closed for substantive mutation while scheduler continuity stays enabled.

## Hypothesis
H-O6: A deterministic exact-generation orphan-recovery rule can restore liveness after a stranded durable owner without duplicate authoritative side effects, provided recovery admission is fenced by exact current generation/owner + nonterminal state + absence of a legal OPEN successor path, and the recovered owner immediately opens a normal successor epoch before voluntary exit.

## Primary variable
RECOVERY_MODE only.
- A = MANUAL_SINGLE_USE_EXACT_GENERATION_RECOVERY (observed incident baseline)
- B = DETERMINISTIC_EXACT_GENERATION_ORPHAN_RECOVERY using the same authority predicates and fresh-SHA CAS.

Scheduler prearm remains 840s; generation fencing, prompt version, state schema, and post-recovery next-step are held fixed.

## Admission predicate for B
All must hold:
1. program is nonterminal;
2. control/ownership.json is reconstructable and current;
3. current generation G and active owner invocation are exact and stable on fresh read;
4. current handoff epoch is absent or CONSUMED and there is no legal OPEN G->G+1 path;
5. no eligible READY successor already exists for a current OPEN epoch;
6. recovery CAS is attempted once against the fresh ownership blob SHA;
7. winner becomes G+1 owner and immediately opens the next normal G+1->G+2 epoch before voluntary close.

## Rejection / safety rules
- Any ambiguity in generation/owner or competing OPEN/READY evidence => no recovery mutation.
- CAS conflict => re-read and classify; never blind retry unchanged.
- Scheduler must remain enabled regardless of recovery admission result.
- No claim of improved reliability from a single successful repair; O6 requires adverse tests.

## Adverse tests
T1: exact reproduction of stranded-owner shape; B must recover once, produce one generation increment, and open a normal next epoch.
T2: stale shadow attempts recovery after authority already advanced; must produce zero authoritative mutation.
T3: OPEN epoch exists but successor is late/not READY; recovery must not bypass the normal handoff path.
T4: conflicting fresh-SHA contenders; exactly one may win, loser re-reads and remains SHADOW.
T5: malformed/unreconstructable ownership; substantive recovery fails closed but scheduler remains enabled.

## Promotion gate
Promote deterministic recovery only if all adverse tests preserve: duplicate authoritative side effects=0; generation monotonicity; exactly one winning recovery where eligible; zero recovery when ineligible; scheduler enabled throughout; and a legal successor path exists after each successful recovery. Otherwise REVISE or REJECT.
