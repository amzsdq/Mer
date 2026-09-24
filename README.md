# Mer

Research workspace for empirically optimizing a ChatGPT Automation relay: prompt/GitHub authority boundary, scheduler continuity, handoff fencing, recovery, and sustained useful-work duty cycle.

## Current status
The research program is ACTIVE in **Stage O8 — long-wake useful-work continuation / repair-first reliability hardening**.

Selected architecture:
**HYBRID stable kernel + GitHub dynamic brain + single authoritative program object.**

### Prompt owns
- identity and writable repository;
- stable execution/recovery invariants;
- same-automation scheduler safety and exact live-verification rules;
- role-relative handoff/stop semantics;
- repair-first finalization guard;
- minimum status/report contract;
- model/reasoning policy.

### GitHub owns
- Goal / Master Plan / current program state;
- dynamic execution and experiment state;
- ownership/generation record when declared by active execution;
- evidence and decisions;
- canonical prompt/version metadata.

### Single runtime authority
`status/program.json` alone owns current stage, next step, active execution pointer, active hypothesis and program gates. `control/active.json` is a static bootstrap pointer. `spec/execution.json` is authoritative only if selected by `status/program.json.active_execution`. Historical status belongs in `evidence/` or `archive/`, never as competing current truth.

## Current relay rules under O8
- Ownership acquisition by a successor starts that invocation's ACTIVE_OWNER tenure; it is not the acquiring invocation's stop gate.
- A normal stop occurs only at verified PROGRAM_COMPLETE or when a later distinct successor completes handoff away from the current invocation.
- Scheduler self-rearm uses the same canonical automation and a complete recurring VEVENT with normalized absolute DTSTART plus `RRULE:FREQ=HOURLY`; exact live readback is required. Relative offset helpers are not used for relay self-rearm.
- Scheduler mutations under incident investigation use immutable write-intent/write-result attribution so concurrent overwrites are not guessed from timing alone.
- Repeated short nonterminal finalization is a critical reliability incident: root-cause repair and differential verification precede nominal retry.
- +180s rearm is fallback continuity only when same-turn continuation/repair is genuinely no longer executable.
- Overall completion additionally requires direct GitHub-server-timestamp evidence of at least one >=600s nonterminal authoritative invocation containing multiple distinct genuine useful-work units, unless a platform limit is evidenced and the objective remains explicitly unresolved.

## Validated / active gates
- prompt-boundary architecture convergence: PASS
- O1 controlled-overlap viability/stabilization: 5/5 clean handoffs PASS
- generation-CAS single-owner handoff and adverse recovery: PASS under tested conditions
- O7 continuity convergence: PASS, but prior overall completion was invalidated because long single-wake work remained unresolved
- O8 role-relative continuation: immediate discriminator PASS; gen34 continued multiple useful units after ownership acquisition
- O8 scheduler representation differential: explicit full-VEVENT absolute write/readback WRITE_OK+STATE_OK PASS; later WAKE_OK/WORK_OK pending
- O8 concurrent scheduler-writer race: forensic overwrite observed; writer-fence hypothesis sourced but not yet active
- O8 >=600s long-wake gate: active gen34 sample
- overall program completion: NO

## Key documents
- `spec/GOAL.md`
- `research/FINAL_DESIGN.md`
- `research/MASTER_PLAN.md`
- `research/H-O8-ROLE-RELATIVE-STOP-GATE.md`
- `research/H-O8-SCHEDULER-WRITER-FENCE.md`
- `research/O8_SCHEDULER_WRITE_ATTRIBUTION_PROTOCOL.md`
- `research/O8_LONG_WAKE_VALIDATION_PROTOCOL.md`
- `research/O8_REJECTED_OR_REVISED_MECHANISMS.md`
- `control/CANONICAL_PROMPT_SUPERVISOR.md`
- `control/prompt-manifest.json`
- `control/ownership.json`
- `status/program.json`
- `spec/execution.json`

## Sources
Prior internal evidence from related repositories is prior evidence only; Mer claims require Mer-side validation or an explicit equivalence argument.
