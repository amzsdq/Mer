# Mer architecture baseline

## Status
AUTHORITY/PROMPT BASELINE VALIDATED; OVERALL RELAY OPTIMIZATION ACTIVE.

## Decision
HYBRID: stable execution/recovery kernel in deployed prompt; changing research/runtime state in GitHub; `status/program.json` is the single runtime authority.

## Authority model
- deployed prompt: stable identity/survival/recovery/work rules only;
- `control/active.json`: static bootstrap pointer;
- `status/program.json`: current stage/next step/active execution/hypothesis/gates;
- `spec/execution.json`: selected execution projection only; stale projection is a defect and was repaired in O8;
- `control/ownership.json`: substantive generation/owner/handoff epoch only;
- `control/prompt-manifest.json`: prompt deployment metadata only, not live scheduler runtime state;
- evidence/archive: historical facts, never competing current authority.

## Clock rule
Temporal claims use GitHub server/external timestamps. Model-written clock strings are reporting metadata only. O8 applied this rule retroactively to the old workwork overlap probe and invalidated its 198s concurrency claim: server commits ordered primary START 16:17:32Z -> primary END 16:18:57Z -> observer 16:29:46Z, contradicting the claimed overlap chronology. This removes an overlap proof; it does not prove serialization.

## Scheduler representation
Use same canonical, complete recurring absolute VEVENT, RRULE:FREQ=HOURLY, exact_schedule, enabled=true, exact live readback. WRITE_OK != STATE_OK != WAKE_OK != WORK_OK.

## Invocation-relative authority
Ownership acquisition begins ACTIVE_OWNER tenure; it is not the acquiring invocation's stop gate. A later distinct successor handoff away is a normal stop only if a distinct successor actually exists and legally acquires authority.

## Scheduler writer authority — active experiment
B candidate: SHADOW scheduler writes=0; only ACTIVE_OWNER or post-CAS new owner writes. Every future clean sample requires protocol-complete pre-mutation attribution. B01 NOT_CLEAN; B02 SUPERSEDED_NOT_CLEAN; B03 ATTRIBUTION_INCOMPLETE_NOT_CLEAN. B04+ uses strict template. Three CLEAN samples plus owner-loss adverse recovery are required before promotion.

## Dispatch topology — unresolved
Same-canonical overlap is no longer treated as proven prior. B03 exact target `15:38:39Z` is a direct Mer observation point. If overlap is observed with server timestamps, retain concurrent handoff as candidate. If dispatch serializes/defers while owner remains active, test the simpler serialized policy: long useful owner invocation -> close-relative short verified rearm, with hourly recurrence retained as fallback. Do not promote either topology from model clocks or undocumented provider assumptions.

## Repair-first finalization
Repeated unchanged failure is invalid. Failure observed -> concrete cause -> mechanism change -> differential verification -> resume. +180s is abnormal fallback only when same-turn continuation cannot execute.

## Finalization
Persist evidence; close safe in-flight unit; verify terminal gate; update `status/program.json` once; cleanup afterward. Overall completion requires Master Plan convergence, not a subtest pass.

## Validated / active gates
- prompt/GitHub authority boundary: PASS
- single runtime authority: PASS after stale projection/runtime-duplication repairs
- generation-CAS ownership/handoff mechanics: retained under tested conditions
- old workwork same-canonical overlap claim: INVALIDATED_BY_SERVER_CLOCK_AUDIT
- O7 continuity convergence: PASS, overall completion invalidated
- O8 role-relative continuation: PASS
- O8 >=600s long-wake: PASS, 736s
- full-VEVENT representation WRITE_OK+STATE_OK: PASS/KEEP
- writer-fence B: ACTIVE, CLEAN count 0, adverse pending
- same-canonical dispatch topology: ACTIVE OBSERVATION
- PROGRAM_COMPLETE: NO

Final boundary: **prompt = stable kernel; GitHub = dynamic brain; `status/program.json` = single runtime authority.**
