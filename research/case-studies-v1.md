# Case studies

This document maps external patterns to explicit Mer invariants and to the experiments that tested them. The external systems are analogues, not proofs that the same implementation is optimal for ChatGPT Automation.

## 1. Kubernetes controllers — desired state, observed state, reconciliation

### External pattern
Kubernetes controllers are control loops that observe current state and move it toward desired state. Kubernetes objects separate `spec` (desired state) from `status` (current/observed state).

Sources:
- https://kubernetes.io/docs/concepts/architecture/controller/
- https://kubernetes.io/docs/concepts/overview/working-with-objects/

### Mer invariant derived from the pattern
1. Desired program state and observed execution evidence must not be conflated.
2. A worker must reconcile against an authoritative desired state instead of treating the last observed file as a new instruction.
3. Completion requires semantic consistency, not merely matching a numeric generation.
4. Historical status must not be able to reactivate old desired work.
5. One control loop should know exactly which object owns the desired state it is reconciling.

### Mer experiments / evidence
- malformed dynamic state test: rejected structurally invalid delegated state instead of guessing;
- generation mismatch test: refused false reconciliation when desired and observed generations differed;
- semantic-conflict test: refused COMPLETE when desired/observed semantics contradicted each other despite matching generation;
- final cleanup defect: exposed that having several GitHub files duplicate stage/next state violates the same controller principle.

### Design consequence
`status/program.json` is the single runtime authority. `control/active.json` is only a bootstrap pointer. `spec/execution.json` becomes active only through `program.active_execution`.

---

## 2. OpenGitOps — versioned desired state pulled and reconciled

### External pattern
OpenGitOps defines four core principles: desired state is declarative, versioned/immutable, automatically pulled by software agents, and continuously reconciled against actual state.

Source:
- https://opengitops.dev/

### Mer invariant derived from the pattern
1. Dynamic Goal/Plan/program state belongs in versioned GitHub, not duplicated in the deployed prompt.
2. A wake should pull fresh dynamic state rather than trust conversational memory.
3. Prompt changes and dynamic program changes are separate classes of mutation.
4. Drift must be detectable before a stale deployed copy is promoted as canonical.
5. Git history/evidence provides auditability, but historical versions do not become current merely because they exist.

### Mer experiments / evidence
- direct prompt vs GitHub authority experiment;
- explicit prompt delegation to GitHub;
- prompt-drift PREPARE -> DEPLOY -> VERIFY -> ACTIVATE experiment;
- PROMPT_HEAVY mutation test showing dynamic changes require prompt mutation;
- HYBRID dynamic-state tests showing changing state can remain GitHub-only.

### Design consequence
The deployed prompt is an executable stable kernel/caller. GitHub is canonical for changing project state. Prompt rollout is versioned and verified, while normal project progression is a GitHub state transition.

---

## 3. GitHub reusable workflows — thin caller, centralized deterministic logic

### External pattern
GitHub reusable workflows let small caller workflows reference centrally maintained workflow logic rather than copying the same implementation into every caller. GitHub also documents commit-SHA references as the safest way to pin an exact reusable workflow version.

Sources:
- https://docs.github.com/en/actions/concepts/workflows-and-actions/reusing-workflow-configurations
- https://docs.github.com/en/actions/how-tos/reuse-automations/reuse-workflows

### Mer invariant derived from the pattern
1. Keep the injected caller small and stable.
2. Centralize reusable logic/configuration instead of copying frequently changing values into every deployed prompt.
3. Keep explicit version metadata for the canonical prompt.
4. A deployed prompt is a copy/reference of canonical logic, not a second equal source of dynamic truth.
5. Stable identity/safety constraints stay close to the caller because they are required before dynamic loading succeeds.

### Mer experiments / evidence
- PROMPT_HEAVY: lowest read cost but highest mutation coupling;
- HYBRID: bounded bootstrap reads with dynamic state decoupled from deployed prompt;
- POINTER_ONLY: smallest caller but inadequate recovery when the only pointer fails;
- prompt-drift test: verified need for versioned PREPARE/DEPLOY/VERIFY/ACTIVATE semantics.

### Design consequence
Mer chooses a hybrid caller:
- embed stable safety/recovery semantics;
- delegate changing work state to GitHub;
- store canonical prompt/version metadata in GitHub;
- mutate the deployed prompt only for changes to the stable contract.

---

## 4. Temporal durable execution — progress survives worker loss

### External pattern
Temporal describes durable execution as resuming work from where it left off after crashes, network failures, or infrastructure outages. The useful analogue is that execution continuity must live outside an individual ephemeral worker.

Sources:
- https://docs.temporal.io/
- https://docs.temporal.io/develop/worker-performance

### Mer invariant derived from the pattern
1. A ChatGPT worker/section is disposable; project progress must be reconstructable from durable state.
2. The next valid action must not depend on private conversational memory.
3. Recovery state must distinguish durable program truth from transient worker observations.
4. Failure of an optimization path must fall back to a durable recovery path instead of losing continuation.
5. The system should optimize latency/utilization only after recoverability is preserved.

### Mer experiments / evidence
- missing-entrypoint adverse test;
- missing/malformed dynamic-state tests;
- final recovery wake validation;
- scheduler acceptance vs live state vs later-wake separation;
- HYBRID recovery references retained in the stable prompt.

### Design consequence
Cold start is a normal operating mode. A new worker reads the stable bootstrap contract, recovers the authoritative program state from GitHub, and resumes only the selected active execution. If no active execution exists, it does not resurrect historical work.

---

## Cross-case synthesis

The four cases converge on the same architecture boundary but for different reasons:

- Kubernetes contributes **single desired-state authority + reconciliation semantics**.
- OpenGitOps contributes **versioned/pulled dynamic state + drift control**.
- GitHub reusable workflows contribute **thin caller + centralized stable logic/version pinning**.
- Temporal contributes **durable progress independent of an individual worker**.

Mer's resulting invariant is:

> The injected prompt contains the minimum stable kernel required to locate, validate and safely execute the durable control plane. GitHub owns changing project state. Inside GitHub, one program object owns current runtime truth; pointers, execution slots, status snapshots and evidence have narrower roles and may never compete with that authority.

## Direct experiment mapping

| External pattern | Mer invariant | Mer test |
|---|---|---|
| Kubernetes spec/status | desired and observed must be distinct | malformed, generation mismatch, semantic conflict |
| GitOps pull/reconcile | fresh versioned GitHub state is canonical for dynamic work | delegation, churn, prompt drift |
| Reusable workflow caller/callee | thin stable caller, centralized mutable logic | PROMPT_HEAVY vs HYBRID vs POINTER_ONLY |
| Temporal durable execution | worker loss must not erase progress/recovery context | missing-entrypoint + recovery validation |
| Single controller authority | current stage/next cannot have multiple equal owners | final stale-state cleanup / program.json authority |
