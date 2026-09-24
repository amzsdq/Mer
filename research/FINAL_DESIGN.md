# Mer architecture baseline

## Status
ARCHITECTURE_BASELINE_VALIDATED; OVERALL_RELAY_OPTIMIZATION_ACTIVE.

This file defines the durable authority/prompt boundary that later relay experiments must preserve. It is not an overall-completion declaration.

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
A **static bootstrap pointer only**. It points to authoritative program state, durable goal/plan, canonical supervisor prompt, final design and execution slot. It must not duplicate current stage, next step, experiment, generation, or current execution state.

### `status/program.json`
The **single runtime authority** for current stage, next step, active execution, program gates/counters, and final result. A worker may not treat another file as current runtime truth if it conflicts with this file.

### `spec/execution.json`
A reusable execution slot, authoritative only when `status/program.json.active_execution` explicitly points to it. When complete, it becomes an explicit inactive sentinel rather than retaining stale experiment state.

### Evidence and archives
- `evidence/`: compact trial evidence.
- `archive/`: retired runtime snapshots.
- historical state never overrides `status/program.json`.

## Normal bootstrap
1. Read the injected stable prompt.
2. Read `control/prompt-manifest.json`, `control/active.json`, and `status/program.json`.
3. Resolve any prompt version mismatch before substantial work.
4. If `active_execution` is non-null, load exactly that execution object and minimum referenced files.
5. If a dedicated ownership record is declared, validate it before authoritative shared-state mutation.
6. Read historical evidence only for a decision, anomaly, or explicit research task.

## Recovery protocol
1. Attempt `control/active.json`.
2. If missing/unreadable, use prompt-embedded recovery references: `spec/GOAL.md`, `research/MASTER_PLAN.md`, `status/program.json`.
3. Treat `status/program.json` as authoritative for stage/next/active execution.
4. Never reconstruct unknown project state from conversational memory or stale archived files.
5. If authoritative state cannot be validated, emit `BOOTSTRAP_FAULT`, preserve scheduler recoverability, and do not invent work.
6. Treat scheduler write acceptance, verified live state, actual later wake, and resumed work as distinct evidence.

## Scheduler representation hardening
The relay's self-rearm is an explicit control-plane write, not an informal relative-time hint.
1. Normalize one intended absolute future instant.
2. Write the complete recurring VEVENT containing `DTSTART` and `RRULE:FREQ=HOURLY` on the same canonical automation.
3. Preserve `exact_schedule` and `enabled=true`.
4. Do not use `dtstart_offset_json` for relay self-rearm.
5. Live-readback must match same automation ID, enabled state, timing mode, recurrence, and the exact intended DTSTART instant.
6. On mismatch, diagnose representation/timezone/provider rewrite versus failed write before repair; never equate WRITE_OK with STATE_OK.

## Invocation-relative handoff semantics
A handoff is role-relative. When an invocation acquires the next generation, that transition starts its ACTIVE_OWNER tenure; it does not terminate the acquiring invocation. `SUCCESSOR_HANDOFF_COMPLETE` is a normal stop gate only for the predecessor that is actually being replaced. The new owner continues genuine useful work until PROGRAM_COMPLETE or a later distinct successor completes handoff away from it.

## Repair-first finalization
Repeated short nonterminal finalization is a critical reliability incident. The required sequence is failure observed -> root cause identified -> mechanism changed -> differential verification -> resume. A +180s rearm is continuity fallback only when same-turn repair/continuation is genuinely no longer executable; it is not a substitute for diagnosis.

## Finalization protocol
Do **not** model completion as a multi-file atomic transaction; GitHub file updates can be independent and cleanup can partially fail.
1. Persist required final evidence and deliverables.
2. Ensure the last active execution is safely closed.
3. Perform the single authoritative finalization write in `status/program.json`: `current_stage=COMPLETE`, `next_step=NONE`, `active_execution=null`.
4. After that authority write, archive or clean stale helper files.
5. Cleanup failure is a maintenance fault, not runtime split-brain, because no helper file owns current program state.
6. Overall completion is forbidden until the current Master Plan convergence gate is satisfied. Under O8 this includes a directly observed >=600s nonterminal authoritative invocation with multiple genuine useful units, unless a platform limit is evidenced and the long-work objective remains explicitly unresolved.

## Rollout method
1. Persist/verify Goal, Master Plan, authoritative program state and static active pointer.
2. Deploy `control/CANONICAL_PROMPT_SUPERVISOR.md` to the existing recurring automation; do not replace the automation.
3. Preserve recurring scheduler invariants required by the deployed kernel.
4. Run clean bootstrap validation and reversible missing-entrypoint recovery validation.
5. Dynamic project changes update GitHub authoritative state, not the deployed prompt.
6. Change the deployed prompt only when the stable execution/recovery contract changes.

## Rejected alternatives
### PROMPT_HEAVY
Rejected as default because it duplicates dynamic state into the deployed prompt and creates avoidable drift/split-brain surfaces.

### POINTER_ONLY
Rejected as default because loss/drift of the sole entrypoint leaves no project-state recovery route.

### MULTI-FILE DYNAMIC AUTHORITY
Rejected because duplicating current stage/next state across multiple files creates stale truths and makes completion depend on best-effort synchronization.

### ACQUISITION_IMPLIES_INVOCATION_EXIT
Rejected by O8. Ownership acquisition is the beginning of the acquiring invocation's owner tenure, not its terminal event.

### BLIND_CORRECTIVE_RERUN
Rejected. Repeated short wake -> +180s -> same unchanged path masks the failure and wastes duty cycle; same-turn repair-first behavior is required.

## Validated / active gates
- prompt-boundary clean validation: PASS
- missing-entrypoint recovery: PASS
- single dynamic authority cleanup: PASS
- O1 normal ownership handoff: 5/5 clean PASS
- eligible READY successor priority: active invariant at a current OPEN epoch/generation
- cross-stage ownership: normal handoff or one explicit single-use recovery transition
- O7 continuity convergence: PASS, but prior overall completion invalidated
- O8 role-relative continuation: active validation; immediate post-acquisition continuation observed
- O8 >=600s long-wake gate: PENDING
- scheduler explicit-full-VEVENT differential after mismatch incident: PENDING NEXT WAKE

Final boundary:
**prompt = stable execution/recovery semantics; GitHub = dynamic project brain; `status/program.json` = single runtime authority inside that brain.**
