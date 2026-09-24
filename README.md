# Mer

Research workspace for empirically optimizing a ChatGPT Automation relay: prompt/GitHub authority boundary, scheduler continuity, handoff fencing, recovery, and sustained useful-work duty cycle.

## Current status
ACTIVE in **Stage O8 — long-wake useful-work continuation / repair-first reliability hardening**.

Selected authority architecture: **HYBRID stable kernel + GitHub dynamic brain + single authoritative program object.**

### Prompt owns
Identity/write scope; stable execution/recovery invariants; same-canonical scheduler safety/live verification; role-relative stop semantics; repair-first guard; reporting; model/reasoning policy.

### GitHub owns
Goal/Master Plan/current program; dynamic experiment state; ownership/generation record; evidence/decisions; canonical prompt/version metadata.

### Single runtime authority
`status/program.json` owns current stage/next step/active execution/active hypothesis/program gates. `control/active.json` is static bootstrap. `spec/execution.json` is the selected execution projection only while referenced; O8 repaired a stale gen34 projection after gen37 became active. Historical status stays in evidence/archive.

## Current O8 rules
- Ownership acquisition starts ACTIVE_OWNER tenure; it is not that invocation's stop gate.
- Normal stop is PROGRAM_COMPLETE or later distinct successor handoff away.
- Self-rearm uses same canonical, complete absolute VEVENT + RRULE:FREQ=HOURLY, exact live readback.
- Writer-fence B is ACTIVE TESTABLE: SHADOW scheduler writes=0; only ACTIVE_OWNER or post-CAS new owner writes.
- Every future scheduler mutation must have protocol-complete immutable pre-mutation attribution; post-hoc addenda preserve history but cannot satisfy CLEAN-sample attribution.
- Repeated short nonterminal finalization is repair-first critical work; +180s is abnormal fallback only.
- Same-canonical dispatch overlap/serialization is now a Mer-side observational diagnostic, not an imported assumption.

## Validated / active gates
- prompt-boundary architecture: PASS
- O1 controlled-overlap historical comparator: 5/5 clean handoffs PASS under tested conditions
- generation-CAS single-owner handoff/recovery: PASS under tested conditions
- O7 continuity convergence: PASS; prior overall completion invalidated
- O8 role-relative continuation: PASS
- O8 >=600s long-wake gate: PASS, gen34 START 14:47:51Z -> END 15:00:07Z = 736s with multiple useful units
- full-VEVENT representation immediate WRITE_OK+STATE_OK: PASS/KEEP representation
- writer-fence B01: NOT_CLEAN, later target wake/stability not proven
- writer-fence B02: SUPERSEDED_NOT_CLEAN
- writer-fence B03/gen37: SHADOW zero-write + fresh-SHA CAS + one owner prearm exact STATE_OK; target 15:38:39Z intact at 15:29:15Z; ATTRIBUTION_INCOMPLETE_NOT_CLEAN because original intent needed a post-hoc addendum; target stability/dispatch-topology observation remains useful
- CLEAN B count: 0; B04+ strict template required
- owner-loss adverse: pending
- overall program completion: NO

## Key documents
- `spec/GOAL.md`
- `research/FINAL_DESIGN.md`
- `research/MASTER_PLAN.md`
- `research/H-O8-SCHEDULER-WRITER-FENCE.md`
- `research/O8_SCHEDULER_WRITER_FENCE_TEST_PLAN.md`
- `research/O8_WRITER_FENCE_DETERMINISTIC_ASSERTIONS.md`
- `research/O8_CLEAN_B_SAMPLE_TEMPLATE.md`
- `research/O8_OWNER_LOSS_ADVERSE_PROTOCOL.md`
- `research/O8_SAME_CANONICAL_DISPATCH_SERIALITY_OBSERVATION.md`
- `research/O8_DISPATCH_TOPOLOGY_DECISION_TREE.md`
- `control/CANONICAL_PROMPT_SUPERVISOR.md`
- `control/prompt-manifest.json`
- `control/ownership.json`
- `status/program.json`
- `spec/execution.json`

## Evidence discipline
Related-repo evidence is prior only; Mer truth requires Mer-side validation or explicit equivalence. WRITE_OK, STATE_OK, WAKE_OK and WORK_OK are distinct. GitHub server timestamps are the time authority for measured work and recovery intervals.
