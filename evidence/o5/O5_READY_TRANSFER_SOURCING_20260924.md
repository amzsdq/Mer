# O5 READY-to-transfer sourcing record

HYPOTHESIS_ID: H-O5-READY-TRANSFER-GRANULARITY
STATUS: TESTABLE
CLAIM: Once an eligible successor is READY on the current OPEN epoch/generation, reducing the predecessor's remaining authoritative atomic unit before yielding should reduce READY-to-CAS transfer latency without increasing duplicate authoritative side effects or continuity failures.
PRIMARY_VARIABLE: PREDECESSOR_POST_READY_MAX_AUTHORITATIVE_UNITS

SOURCE_CLASS:
- INTERNAL_EMPIRICAL: Mer O4 demonstrated repeated clean generation-CAS handoffs under READY-priority semantics (A840 3/3 and B720 3/3); O5 stage-entry gen17->18 also completed normally. This establishes viability, not optimal granularity.
- AUTHORITATIVE_IMPLEMENTATION: Kubernetes Lease leader election uses a shared coordination record and optimistic concurrency/resourceVersion so only one contender update succeeds; Lease tracks holder identity and transitions. Official docs: https://kubernetes.io/docs/concepts/cluster-administration/coordinated-leader-election/ and https://kubernetes.io/docs/reference/kubernetes-api/coordination/lease-v1/
- AUTHORITATIVE_IMPLEMENTATION / COUNTER-PRIOR: Redis distributed-lock guidance frames minimum guarantees as mutual exclusion plus liveness/deadlock freedom and fault tolerance, and warns that timing/validity windows matter. Official docs: https://redis.io/docs/latest/develop/clients/patterns/distributed-locks/

SUPPORTING_PRIORS:
1. Kubernetes coordination shows that a single versioned coordination object plus optimistic concurrency can serialize leadership transfer among concurrent candidates.
2. Mer's O4 clean handoffs show generation-CAS + READY binding can work repeatedly in this runtime.

COUNTER_PRIORS / KNOWN_CONFLICTS:
1. Immediate yielding is not automatically superior: too-fine atomic units can increase coordination/checkpoint overhead and reduce useful-work duty cycle.
2. External lease/lock systems have stronger runtime primitives and clocks than ChatGPT Automations + GitHub; their latency numbers do not transfer.
3. Redis lock guidance emphasizes liveness as well as exclusion; minimizing transfer latency must not sacrifice recoverability or create deadlock/stale-owner behavior.

TRANSLATION:
- Kubernetes resourceVersion competition maps to Mer fresh-SHA generation CAS.
- holderIdentity/leaseTransitions maps to active_invocation_id/generation transitions.
- Redis mutual-exclusion+liveness framing maps to Mer duplicate-authoritative-side-effect=0 plus successful continuation/handoff.

NON_TRANSFERABLE_ASSUMPTIONS:
- No assumption of Kubernetes API-server latency, watch delivery, lease expiry, synchronized clocks, or Redis TTL semantics.
- GitHub commit timestamps remain Mer's measurement authority.

DISCRIMINATING_TEST:
Hold scheduler offset=840s, generation-CAS semantics, READY binding, and workload class constant. Compare fixed post-READY predecessor budgets:
A: MAX_AUTHORITATIVE_UNITS_AFTER_READY=1 (current baseline: finish current atomic unit only)
B: MAX_AUTHORITATIVE_UNITS_AFTER_READY=0 for boundaries where the current unit is already complete when READY is observed (yield before admitting any new authoritative unit).
Measure READY_EVIDENCE commit timestamp -> ownership CAS commit timestamp, duplicate authoritative side effects, handoff success, and control-write count. Use 3 clean samples per window. Do not compare wall-clock work duration as a stop target.

PROMOTION_GATE:
Promote B only if continuity/recoverability is no worse, duplicate authoritative side effects remain zero, and median READY-to-CAS latency or control overhead is materially lower. If equivalent, keep simpler current baseline A.

REJECTION / REVISION RULE:
- Any duplicate authoritative side effect or continuity regression: REJECT B.
- No material latency/overhead improvement after matched clean windows: KEEP A.
- Ambiguous timestamp attribution or non-comparable boundaries: REVISE instrumentation before retry; do not repeat unchanged.

SEARCH_STOPPING_RULE:
Satisfied: plausible mechanism, competing/failure interpretation, and discriminating test are all defined.
