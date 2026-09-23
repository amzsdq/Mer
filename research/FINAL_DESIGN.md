# Mer final design

## Decision
Use HYBRID: a stable execution/recovery kernel in the deployed automation prompt; all changing project/research state in GitHub.

The critical refinement is **single dynamic authority**. GitHub being the dynamic brain is not enough if several GitHub files independently duplicate the current stage/next step.

## Authority model

### Deployed prompt
Owns only stable execution/recovery semantics:
- identity, repository and write scope;
- explicit delegation contract: GitHub owns dynamic program/project state;
- canonical recovery references;
- scheduler survival invariants;
- fail-safe behavior;
- work-session discipline and compact reporting contract.

### `control/active.json`
A **static bootstrap pointer only**.
It points to:
- the authoritative program state;
- durable goal/plan;
- canonical supervisor prompt;
- final design and execution slot.

It must not duplicate:
- current stage;
- next step;
- experiment;
- generation;
- current execution state.

### `status/program.json`
The **single runtime authority** for:
- `current_stage`;
- `next_step`;
- `active_execution`;
- program gates/counters;
- final result.

A worker may not treat another file as current runtime truth if it conflicts with this file.

### `spec/execution.json`
A reusable execution slot. It is authoritative only when
`status/program.json.active_execution` explicitly points to it.
When the program is complete, it remains an explicit inactive sentinel rather than retaining stale experiment state.

### Evidence and archives
- `evidence/`: compact trial evidence.
- `archive/`: retired runtime snapshots.
- historical state never overrides `status/program.json`.

## Normal bootstrap
1. Read the injected stable prompt.
2. Read `control/active.json`.
3. Read `status/program.json`.
4. If `active_execution` is non-null, load exactly that execution object and the minimum referenced files.
5. If `current_stage=COMPLETE` and `active_execution=null`, do not revive an old experiment from historical files.
6. Read historical evidence only for a decision, anomaly or explicit research task.

## Recovery protocol
1. Attempt `control/active.json`.
2. If missing/unreadable, use prompt-embedded recovery references: `spec/GOAL.md`, `research/MASTER_PLAN.md`, `status/program.json`.
3. Treat `status/program.json` as authoritative for stage/next/active execution.
4. Never reconstruct unknown project state from conversational memory or stale archived files.
5. If authoritative state cannot be validated, emit `BOOTSTRAP_FAULT`, preserve scheduler recoverability, and do not invent work.
6. Treat scheduler update acceptance, verified live scheduler state, and a later observed wake as distinct evidence.

## Finalization protocol
Do **not** model completion as a multi-file atomic transaction; GitHub file updates can be independent and cleanup can partially fail.

Instead:
1. Persist required final evidence and deliverables.
2. Ensure the last active execution is safely closed.
3. Perform the single authoritative finalization write in `status/program.json`:
   - `current_stage = COMPLETE`
   - `next_step = NONE`
   - `active_execution = null`
4. After that authority write, archive or clean stale helper files.
5. Cleanup failure is a maintenance fault, not a runtime split-brain, because no helper file owns current program state.

## Rollout method
1. Persist/verify Goal, Master Plan, authoritative program state and static active pointer.
2. Deploy `control/CANONICAL_PROMPT_SUPERVISOR.md` to the existing recurring automation; do not replace the automation.
3. Preserve recurring scheduler invariants required by the deployed kernel.
4. Run clean bootstrap validation and a reversible missing-entrypoint recovery validation.
5. Dynamic project changes update GitHub authoritative state, not the deployed prompt.
6. Change the deployed prompt only when the stable execution/recovery contract changes.

## Rejected alternatives
### PROMPT_HEAVY
Rejected as the default. It minimizes bootstrap reads but duplicates dynamic state into the deployed prompt. Dynamic changes therefore require prompt mutation and create an avoidable drift/split-brain surface.

### POINTER_ONLY
Rejected as the default. It keeps dynamic state cleanly in GitHub but loss/drift of the sole entrypoint leaves no project-state recovery route. It can fail safe but cannot self-recover project context without stable recovery references.

### MULTI-FILE DYNAMIC AUTHORITY
Rejected after finalization cleanup exposed the defect directly. Duplicating current stage/next state across `program.json`, `active.json`, `status/current.json`, and `spec/execution.json` creates multiple stale truths and makes completion depend on best-effort multi-file synchronization.

## Validated gates
- final clean wakes: 3/3 PASS
- missing-entrypoint recovery: 1/1 PASS
- final authority cleanup: COMPLETE

Final boundary:
**prompt = stable execution/recovery semantics; GitHub = dynamic project brain; `status/program.json` = single runtime authority inside that brain.**
