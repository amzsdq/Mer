# Mer final design

## Decision
Use HYBRID: stable execution/recovery kernel in the deployed automation prompt; all changing project/research state in GitHub.

## Repository schema
- `control/active.json`: primary bootstrap pointer; references authoritative current program/workload/execution files.
- `control/CANONICAL_PROMPT_SUPERVISOR.md`: canonical stable deployed kernel.
- `spec/GOAL.md`: durable goal and recovery reference.
- `research/MASTER_PLAN.md`: durable staged plan, gates, stop rules and recovery reference.
- `status/program.json`: authoritative current stage/next step/gate counters and recovery reference.
- `spec/execution.json`: changing execution generation/marker/action.
- `evidence/`: compact immutable trial records.

## Recovery protocol
1. Attempt `control/active.json`.
2. If missing/unreadable, use prompt-embedded canonical references: `spec/GOAL.md`, `research/MASTER_PLAN.md`, `status/program.json`.
3. Follow authoritative `status/program.json.next_step`; load only referenced state needed for it.
4. Never reconstruct unknown project state from memory.
5. If authoritative state still cannot be validated, emit `BOOTSTRAP_FAULT`, preserve the same recurring continuation, and do not invent work.
6. Treat scheduler update acceptance, verified live scheduler state, and a later observed wake as distinct evidence. A worker cannot prove its own missing future wake; independent liveness observation is required for that failure class.

## Rollout method
1. Persist/verify GitHub Goal, Master Plan, program state and active pointer first.
2. Deploy `control/CANONICAL_PROMPT_SUPERVISOR.md` to the existing recurring automation; do not replace the automation.
3. Preserve recurring hourly RRULE, exact scheduling and enabled state.
4. Run clean bootstrap validation and a reversible missing-entrypoint recovery validation.
5. Only after both pass, use the candidate for normal continuation.
6. Dynamic project changes update GitHub state, not the deployed prompt. Change the prompt only when the stable execution/recovery contract itself changes.

## Rejected alternatives
### PROMPT_HEAVY
Rejected as the default. It minimizes bootstrap reads, but duplicates dynamic state into the deployed prompt. Dynamic generation/marker changes therefore require prompt mutation and create an avoidable drift/split-brain surface.

### POINTER_ONLY
Rejected as the default. It keeps dynamic state cleanly in GitHub and tolerated repeated GitHub-only churn without prompt mutation, but a sole-entrypoint failure leaves no project-state recovery path. It can fail safe but cannot self-recover project context without stable recovery references.

## Validated gates
- final clean wakes: 3/3 PASS
- missing-entrypoint recovery: 1/1 PASS

The selected boundary is therefore: prompt = stable execution/recovery semantics; GitHub = dynamic project brain.