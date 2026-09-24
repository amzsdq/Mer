# O8 Writer-Fence External Prior Art — 2026-09-24

Status: SOURCING ADDENDUM
Hypothesis: `H-O8-SCHEDULER-WRITER-FENCE`

## Authoritative implementation reference
Kubernetes Lease / coordinated leader election documentation provides a close implementation analogue for the separation Mer is testing:
- replicas/candidates may coexist, but one holder is active leader;
- leader identity and transitions are durable coordination state;
- candidates use optimistic concurrency (`resourceVersion`) so concurrent acquisition attempts have one accepted update;
- lease expiry/renewal is an explicit liveness/recovery mechanism, not merely 'who woke first';
- leader transition latency and crash recovery are first-class parameters.

Sources consulted:
- Kubernetes Leases: https://kubernetes.io/docs/concepts/architecture/leases/
- Kubernetes Coordinated Leader Election: https://kubernetes.io/docs/concepts/cluster-administration/coordinated-leader-election/
- Kubernetes Lease v1 API: https://kubernetes.io/docs/reference/kubernetes-api/coordination/lease-v1/

## What transfers to Mer
1. `control/ownership.json` fresh-SHA generation CAS is structurally analogous to optimistic-concurrency leader acquisition: contenders may exist, one accepted versioned update establishes authority.
2. Scheduler-writer fencing should follow authority acquisition if scheduler mutation is an active-leader side effect. This supports the B-arm shape: SHADOW prepares, CAS establishes owner, owner writes.
3. Recovery cannot be hand-waved. Kubernetes has explicit lease duration/renewal/transition semantics. Mer B must therefore pass the owner-loss adverse sample; merely reducing overwrite races is insufficient.

## Important non-equivalence / contrary pressure
Kubernetes candidates can independently observe a Lease and attempt acquisition after an explicit lease-expiry rule. Mer currently does NOT have a validated automatic time-expiry takeover rule; O1 specifically rejected time alone as sufficient authority. Therefore Kubernetes prior art does not justify adding automatic TTL takeover to Mer without a separate hypothesis/test.

Likewise, Kubernetes leader election coordinates a dedicated lock object. Mer currently couples scheduler-writer permission to substantive ownership. If owner-loss testing shows unacceptable scheduler-liveness regression, the likely REVISE path is not 'let every SHADOW write again' but a separate durable scheduler-writer claim/epoch with explicit recovery semantics.

## Effect on active hypothesis
This prior art strengthens the rationale for testing owner-fenced writes but also strengthens the adverse-recovery requirement. No promotion decision changes yet: B remains TESTABLE/PENDING multi-sample and owner-loss evidence.
