# Mer Master Plan v3 — Empirical Relay Optimizer

Status: ACTIVE
Current stage: dynamic; authoritative current stage is `status/program.json`.
Primary objective: continuity first, then maximum long-run useful-work utilization, then simplicity.

## 0. Research discipline
Each wake follows `status/program.json.next_step`. Do not promote theory directly into production policy.

### Hypothesis sourcing gate — REQUIRED
Before a new hypothesis becomes TESTABLE, follow `research/HYPOTHESIS_SOURCING_POLICY.md`. Use internal empirical evidence, authoritative implementation references, applicable formal work, and competing/counter evidence. A candidate is KEEP/REVISE/REJECT only after a discriminating test. Dynamic runtime authority remains `status/program.json`; execution/evidence files are authoritative only when referenced there.

## Stage O0 — Evidence intake and reopened objective — COMPLETE
Prior tEST/workwork evidence remains prior evidence, not automatic Mer truth.

## Stage O1 — Controlled-overlap / baton baseline — COMPLETE
Reference: `research/WORKWORK_OVERLAP_INTAKE.md`.

Validated historical comparator:
- same recurring automation; `RRULE:FREQ=HOURLY`; `exact_schedule`; enabled;
- no voluntary work-duration cutoff;
- O1 used one verified scheduler pre-arm at wake +840s;
- exactly one substantive ACTIVE_OWNER fenced by durable generation/CAS;
- successor begins SHADOW_SUCCESSOR and may prepare/read/write immutable own evidence;
- predecessor may not voluntarily close before transfer; late/missing successor means predecessor continues useful work;
- clean transfer requires actual successor wake/READY plus durable generation increment, no duplicate authoritative side effect and no checkpoint loss.

O1A 3/3 PASS; O1B stabilization 5/5 clean handoffs PASS. O1 also exposed an owner-loss liveness hole; Mer validated one explicit program-authorized single-use recovery transition for a known stuck generation. Automatic time-expiry recovery remained separate.

Historical note: O1 allowed SHADOW scheduler prearm as crash insurance. O8 later observed that an unconstrained all-wake scheduler lane can overwrite the single canonical target while substantive ownership remains unchanged. Therefore SHADOW prearm is retained only as a historical comparator until O8D resolves scheduler-writer authority.

## Stage O2 — Prompt enforcement / representation / versioned sync
Stable invariants belong in deployed prompt plus canonical GitHub source; dynamic runtime state remains GitHub-only. Compare representation/anchors only with semantics held constant. Version mismatch uses prepare/deploy/verify/activate; version match uses cheap manifest fast path. During rollout or suspected drift, exact deployed content must be verified against the prepared canonical; version/id equality alone is insufficient.

## Stage O3 — Productive-window / completion-envelope observation
There is no fixed voluntary work-duration completion rule. Measure platform survival boundary, productive window, close overhead, and interruption behavior observationally. A duration threshold may be a validation gate without becoming a stop gate.

## Stage O4 — Wake-prearm offset optimization
Keep +840s as fixed baseline and test adaptive pre-arm candidates only when a concrete successor-dependent boundary warrants it. Hold selected offset constant within each sample unless offset is the primary variable. Measure actual wake lateness, overlap, handoff gap, missed occurrence, recovery, scheduler overhead, and useful-work duty cycle. Prefer simpler fixed policy if reliability/utilization are equivalent.

## Stage O5 — Handoff optimization
An ELIGIBLE READY successor bound to the current OPEN epoch/generation has transfer priority: predecessor finishes only its current atomic authoritative unit, starts no new authoritative unit, and yields to fresh-SHA generation CAS. READY without current epoch/generation binding grants no priority. Substantive ownership remains generation fenced.

## Stage O6 — Adverse recovery tests
Test missed intended wake, predecessor termination, stale state, duplicate/competing actor, prompt mismatch, missing bootstrap, and accepted scheduler write followed by absent wake. Reject fast candidates without demonstrated recovery.

## Stage O7 — Final convergence — CONTINUITY SUBGATE PASS, OVERALL COMPLETION INVALIDATED
O7 established repeated clean continuity/recovery evidence but overall completion was invalidated because long single-wake useful work remained unresolved. Preserve O7 evidence; do not reuse its prior terminal conclusion.

## Wake utilization rule
A wake is a work session, not a one-step callback. A handoff stop is invocation-role-relative: acquisition starts the acquiring invocation's ACTIVE_OWNER tenure; only a later distinct successor handoff away can stop that owner. Continue genuine plan-defined safe units until verified PROGRAM_COMPLETE or such a later handoff away. Abnormal platform/safety interruption is not successful voluntary completion. Never sleep, pad, repeat converged work, or invent work. Repeated materially short nonterminal wakes with runnable work are `SHORT_CYCLE_ANOMALY` and a repair target.

## Cross-stage ownership rule
A research-stage transition is not complete merely because `status/program.json.current_stage` changes. Ownership must also cross the boundary. Prefer normal handoff; if the prior-stage owner is already gone, use one explicitly authorized single-use recovery transition tied to the exact stuck generation, consume it once, and pre-open the next normal handoff epoch.

## Stage O8 — Long-wake useful-work continuation / repair-first reliability
The prior O7 completion was invalidated because continuity converged while observed invocations remained materially short. O8 addresses the unresolved original utilization objective and scheduler-control failures exposed during repair.

### O8A — Role-relative stop-gate validation
Validate that a wake which acquires ownership does not stop because that inherited handoff completed. The new owner continues through multiple distinct useful units until PROGRAM_COMPLETE or a later distinct successor takes ownership away.

Current result: gen34 immediate discriminator PASS. The same invocation acquired ownership and continued through multiple distinct durable useful units.

### O8B — Long-wake empirical gate
Overall Mer completion requires at least one nonterminal authoritative invocation with GitHub-server-timestamp WORKED >=600 seconds and multiple distinct genuine useful-work units, no sleep/padding/repetition, and continuity secured. This is a validation gate, not a stop trigger.

Current result: PASS. Gen34 START commit `bec5bd2755eb7819181f38c99b66f3b4abcdb8ee` at `14:47:51Z`; qualification commit `95568a93bc3bba66c4cced35f8bf50e7db98ce27` at `14:57:53Z`; exact elapsed 602s. Overall program remains active because scheduler incident convergence is still open.

### O8C — Work packing / admission optimization
If role-relative correction alone does not materially extend work, test one primary variable at a time in work-package sizing, next-step breadth, unit chaining, close reserve, and successor-boundary timing. Gen34's 602s sample means work packing is not currently required to prove basic >=600s feasibility; retain as a future optimization only if duty-cycle evidence later shows material benefit.

### O8D — Scheduler representation and writer authority
A scheduler incident exposed two distinct questions that must not be conflated.

1. Representation differential: use one normalized absolute DTSTART encoded in a complete recurring VEVENT, exact live readback, and immutable write-intent/result attribution. Gen34 recovery target `15:08:14Z` has WRITE_OK+STATE_OK PASS; later WAKE_OK/WORK_OK remain pending and must be observed separately.
2. Writer authority: gen34 forensic history proved the canonical DTSTART moved from `15:01:31Z` to `14:51:15Z` within ~4.7s while substantive ownership remained generation 34. This proves a concurrent last-writer scheduler surface, not the exact writer identity. `H-O8-SCHEDULER-WRITER-FENCE` is sourced but must not become the primary variable until the representation sample's later wake/work result is known. When tested, hold full-VEVENT representation constant and compare ALL_WAKES_PREARM against OWNER_OR_NEWLY_ACQUIRED_OWNER_ONLY, including owner-loss adverse recovery.

### O8 repair-first incident rule
Repeated unchanged failure is invalid work. Sequence: FAILURE_OBSERVED -> ROOT_CAUSE_IDENTIFIED -> MECHANISM_CHANGED -> DIFFERENTIAL_VERIFICATION -> RESUME. +180s is continuity fallback only when same-turn continuation/repair is genuinely unavailable; it is not a substitute for diagnosis.

## Current convergence gate
Do not mark PROGRAM_COMPLETE while the attributed full-VEVENT sample lacks later WAKE_OK/WORK_OK or while the scheduler-writer candidate remains materially testable. After those are resolved, reassess final convergence using continuity, long-wake utilization, recovery, duplicate-side-effect, scheduler-overwrite, and simplicity evidence together.
