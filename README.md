# Mer

Research workspace for empirically optimizing a ChatGPT Automation relay: scheduler continuity, authority fencing, recovery and sustained useful-work duty cycle.

## Current status
ACTIVE in **Stage O8 — long-wake / repair-first reliability**. `status/program.json` is the single runtime authority.

## Current facts
- O8 role-relative continuation: PASS.
- O8 long-wake gate: PASS. Gen34 GitHub START 14:47:51Z -> END 15:00:07Z = 736s with multiple genuine units.
- Full absolute recurring VEVENT immediate WRITE_OK+STATE_OK: PASS/KEEP representation.
- Writer-fence B is ACTIVE TESTABLE: SHADOW scheduler writes=0; only ACTIVE_OWNER or post-CAS new owner writes.
- B01 NOT_CLEAN; B02 SUPERSEDED_NOT_CLEAN; B03 ATTRIBUTION_INCOMPLETE_NOT_CLEAN. CLEAN B count=0.
- B03 target 15:38:39Z was still intact at GitHub 15:29:15Z and is being used for direct Mer dispatch-topology observation.
- Owner-loss adverse remains pending.
- PROGRAM_COMPLETE=NO.

## Critical evidence correction
The former workwork `OVERLAP-15M-WAKE12M-01` 198-second concurrency claim is **invalidated as temporal evidence**. GitHub server chronology is primary-start commit 16:17:32Z, primary-end commit 16:18:57Z, observer commit 16:29:46Z. The old result depended on model-written clock strings, including a claimed primary end more than 13 minutes after the file containing it had already been committed. This does not prove serialization; it removes the old proof of overlap. Mer now requires direct server/external-clock evidence for same-canonical overlap.

## Authority architecture
Prompt owns stable execution/recovery invariants. GitHub owns dynamic research state. `control/ownership.json` owns substantive generation/owner/epoch. Prompt manifest owns prompt deployment metadata only and must not duplicate live scheduler runtime state.

## Evidence discipline
WRITE_OK, STATE_OK, WAKE_OK and WORK_OK are distinct. GitHub server timestamps are authoritative for measured work/recovery/temporal ordering. Model-written time strings are metadata only.

## Key current documents
- `spec/GOAL.md`
- `research/MASTER_PLAN.md`
- `research/WORKWORK_OVERLAP_INTAKE.md`
- `research/H-O8-SCHEDULER-WRITER-FENCE.md`
- `research/O8_SCHEDULER_WRITER_FENCE_TEST_PLAN.md`
- `research/O8_WRITER_FENCE_DETERMINISTIC_ASSERTIONS.md`
- `research/O8_CLEAN_B_SAMPLE_TEMPLATE.md`
- `research/O8_OWNER_LOSS_ADVERSE_PROTOCOL.md`
- `research/O8_SAME_CANONICAL_DISPATCH_SERIALITY_OBSERVATION.md`
- `research/O8_DISPATCH_TOPOLOGY_DECISION_TREE.md`
- `control/CANONICAL_PROMPT_SUPERVISOR.md`
- `control/ownership.json`
- `status/program.json`
