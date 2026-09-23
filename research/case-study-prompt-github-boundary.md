# Case study — prompt vs durable repository boundary

## User hypothesis under test
Keep only wake-critical instructions in the reservation prompt (self-update, status/reporting, bootstrap behavior), while storing Goal/Plan/project execution context in GitHub. Store the canonical reservation prompt in GitHub as well so running agents can propagate or repair it.

## Finding
**Adopt with one refinement.** The reservation prompt should be treated as a bootloader/caller, not as the full runtime program. GitHub should hold the canonical runtime kernel and mutable project state.

### Why
A wake begins with the reservation prompt already injected, so instructions needed *before the first trustworthy GitHub read* have privileged value there. Everything else is better versioned in GitHub where it can change without repeatedly mutating the scheduler prompt.

The dangerous failure mode is pointer-only bootstrap: if GitHub cannot be read or the pointer is corrupt, the agent may not know the scheduler invariants needed to fail safely. Therefore the bootstrap prompt must retain a small safety kernel.

## External analogues

### Kubernetes controller pattern
Kubernetes separates desired state (`spec`) from observed state (`status`) and uses stable controller logic to reconcile them. Mer should mirror this:
- stable execution kernel = controller logic
- `spec/` = desired Goal/Plan/policy
- `status/` = observed current state/results
- wake loop = reconciliation pass

### GitOps / Argo CD
GitOps stores desired state declaratively and versioned in Git, while agents pull it and reconcile live state. This supports putting project plan/policy/configuration in GitHub rather than duplicating them in the reservation prompt.

### GitHub reusable workflows
A small caller can invoke centrally maintained reusable workflow logic and pass inputs. The analogous Mer architecture is:
- reservation prompt = small caller
- canonical prompt/kernel in GitHub = reusable workflow
- active spec/status = inputs/state
This reduces duplicated logic and makes one canonical version auditable.

### Temporal durable execution
Temporal's durable-execution model emphasizes persisted execution state/history and resumption after interruption. The relevant lesson is not to depend on conversational memory for continuation-critical state; checkpoint and next-action state must be durable and reconstructable.

## Boundary decision

### Keep in reservation prompt
Only items that are required to safely reach and interpret GitHub:
1. ROLE / SELF_AUTOMATION_ID / writable REPO.
2. Exact bootstrap entrypoint path (for example `control/active.json`).
3. Canonical prompt path and prompt version/hash field.
4. Source-of-truth precedence rule.
5. Same-automation scheduler invariants:
   - no replacement automation during normal continuation
   - complete recurring VEVENT
   - enabled recurring state
   - no stale/past DTSTART
6. Minimum self-update/recovery sequence.
7. Minimum terminal/report status contract.
8. What to do when GitHub state cannot be read or validated.
9. MODEL_POLICY / REASONING_POLICY when this must survive repository failure.

### Put in GitHub
Everything expected to evolve:
- Goal
- Plan
- current task / next action
- project context
- active experiment
- runtime target/cutoff/margins
- external evidence watermarks
- measurement schema
- detailed protocols
- decision history
- evidence ledger
- status/current observed state
- canonical reservation prompt
- prompt changelog and rollout/rollback metadata

## Important refinement: canonical prompt propagation
Do not let each worker invent or rewrite the prompt independently.

Use:
- `control/CANONICAL_PROMPT.md`
- `control/prompt-manifest.json`

Manifest should include at minimum:
- `prompt_version`
- `prompt_sha256` or equivalent immutable content identifier
- `compatible_schema_version`
- `rollout_state`
- `previous_good_version`

At wake:
1. Bootstrap prompt reads manifest.
2. Compare deployed prompt version with canonical version.
3. If mismatch, classify PROMPT_DRIFT.
4. Only a defined sync procedure may update the automation prompt.
5. Re-read live automation state after sync.
6. Keep previous known-good prompt metadata for rollback.

The deployed prompt should contain enough logic to detect drift even when it is one version behind.

## Main risk
If dynamic Goal/Plan/status are duplicated in both GitHub and the scheduler prompt, two authorities emerge. This recreates stale-state bugs. Dynamic data must have **one owner: GitHub**.

## Current leading candidate
HYBRID_BOOTSTRAP.
