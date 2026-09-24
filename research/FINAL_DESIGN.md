# Mer architecture baseline

## Status
ARCHITECTURE_BASELINE_VALIDATED; OVERALL_RELAY_OPTIMIZATION_ACTIVE.

This file defines the durable authority/prompt boundary that later relay experiments must preserve. It is not an overall-completion declaration.

## Decision
Use HYBRID: a stable execution/recovery kernel in the deployed automation prompt; all changing project/research state in GitHub.

The critical refinement is **single dynamic authority**. GitHub being the dynamic brain is not enough if several GitHub files independently duplicate the current stage/next step.

## Authority model

### Deployed prompt
Owns only stable execution/recovery semantics: identity/write scope, GitHub delegation, recovery references, scheduler survival invariants, fail-safe behavior, work-session discipline and reporting.

### `control/active.json`
Static bootstrap pointer only. It must not duplicate current stage, next step, experiment, generation, or current execution state.

### `status/program.json`
Single runtime authority for current stage, next step, active execution, program gates/counters, and final result.

### `spec/execution.json`
Reusable execution slot, authoritative only while `status/program.json.active_execution` points to it. It must be synchronized when the active experiment changes; stale retained experiment state is a control-plane defect, not a second authority. O8 repaired one such gen34->gen37 stale projection while keeping `status/program.json` authoritative.

### `control/ownership.json`
Dedicated authority for current substantive owner/generation and handoff epoch only. It does not own program stage/next step.

### Evidence and archives
`evidence/` stores trial evidence; historical evidence never overrides current authority.

## Normal bootstrap
1. Read injected stable prompt.
2. Read prompt manifest, static active pointer, and authoritative program state.
3. Resolve prompt mismatch before substantial work.
4. Load only the active execution object and minimum referenced files.
5. Validate dedicated ownership before authoritative mutation.
6. Historical evidence is loaded only for a decision/anomaly/research task.

## Recovery protocol
If active pointer is unavailable, recover from Goal, Master Plan and `status/program.json`. Never invent unknown state from chat memory. Scheduler WRITE_OK, STATE_OK, WAKE_OK and WORK_OK remain distinct.

## Scheduler representation hardening
1. Normalize one absolute future instant.
2. Write complete recurring VEVENT with DTSTART and RRULE:FREQ=HOURLY on the same canonical.
3. Preserve exact_schedule and enabled=true.
4. Do not use dtstart_offset_json for relay self-rearm.
5. Live readback must exactly match ID/enabled/timing/recurrence/DTSTART.
6. Diagnose mismatch before repair; WRITE_OK is not STATE_OK.

O8 result: full-VEVENT absolute representation has repeated WRITE_OK+STATE_OK and is retained while writer authority is the active primary variable. Later clean target WAKE_OK still requires direct evidence.

## Invocation-relative handoff semantics
Ownership acquisition starts the acquiring invocation's ACTIVE_OWNER tenure and is not its stop gate. Only PROGRAM_COMPLETE or a later distinct successor handoff away normally stops that owner.

## Scheduler writer authority — active O8 experiment
The canonical scheduler is a shared mutable resource. O8 observed an unattributed target overwrite while substantive ownership remained unchanged. Current B candidate fences normal scheduler writes to ACTIVE_OWNER or a successor only after fresh-SHA generation CAS. SHADOW performs zero scheduler writes. This is TESTABLE, not yet promoted: B03 immediate WRITE_OK+STATE_OK is provisional until target stability and actual later WAKE/WORK are observed; three clean samples and owner-loss adverse recovery are required.

A separate observational diagnostic now checks whether the same canonical actually overlaps an active invocation or serializes/defer dispatch. If Mer directly reproduces serialization, overlap-dependent handoff architecture must be revised rather than preserved by assumption.

## Repair-first finalization
Repeated short nonterminal finalization is a critical reliability incident. Required sequence: failure observed -> root cause identified -> mechanism changed -> differential verification -> resume. +180s is fallback only when same-turn continuation truly cannot execute.

## Finalization protocol
1. Persist required final evidence/deliverables.
2. Safely close last active execution.
3. Single authoritative finalization write in `status/program.json`: COMPLETE/NONE/null.
4. Archive/clean helpers after authority write.
5. Cleanup failure is maintenance, not split-brain.
6. Overall completion is forbidden until Master Plan convergence is satisfied.

## Rollout method
Persist Goal/Plan/program state/static pointer; deploy canonical prompt to same recurring automation; preserve scheduler invariants; validate bootstrap/recovery; keep dynamic changes in GitHub.

## Rejected alternatives
- PROMPT_HEAVY: dynamic-state duplication/drift.
- POINTER_ONLY: weak recovery.
- MULTI_FILE_DYNAMIC_AUTHORITY: stale competing truths.
- ACQUISITION_IMPLIES_INVOCATION_EXIT: rejected by O8.
- BLIND_CORRECTIVE_RERUN: rejected; repair-first required.
- UNFENCED_SCHEDULER_WRITERS: not yet formally rejected, but active B experiment tests whether owner-fencing is superior without continuity regression.

## Validated / active gates
- prompt-boundary clean validation: PASS
- missing-entrypoint recovery: PASS
- single dynamic authority cleanup: PASS, with O8 stale execution-projection repair recorded
- O1 normal ownership handoff: 5/5 clean PASS
- cross-stage ownership: normal handoff or explicit single-use recovery transition
- O7 continuity convergence: PASS, prior overall completion invalidated
- O8 role-relative continuation: PASS
- O8 >=600s long-wake gate: PASS, 736s GitHub-server-clock invocation
- scheduler full-VEVENT representation WRITE_OK+STATE_OK: PASS/KEEP representation
- scheduler writer-fence B: ACTIVE TESTABLE; B03 provisional, clean-count=0, owner-loss adverse pending
- same-canonical dispatch seriality: ACTIVE OBSERVATION
- overall program complete: NO

Final boundary:
**prompt = stable execution/recovery semantics; GitHub = dynamic project brain; `status/program.json` = single runtime authority inside that brain.**
