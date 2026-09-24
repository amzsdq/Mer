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

Current result: PASS. Gen34 START `14:47:51Z`, qualification `14:57:53Z` = 602s, END `15:00:07Z` = final WORKED 736s. Multiple distinct genuine repair/research units were durably recorded. Overall program remains active because scheduler incident convergence is still open.

### O8C — Work packing / admission optimization
Gen34 proved >=600s feasibility without padding, so work packing is not required to prove basic long-wake feasibility. Retain as a future optimization only if duty-cycle evidence later shows material benefit.

### O8D — Scheduler representation and writer authority
A scheduler incident exposed two distinct questions.

1. Representation differential: normalized complete recurring VEVENT + exact live readback + immutable intent/result attribution achieved repeated WRITE_OK+STATE_OK. KEEP this representation for the writer-authority experiment. The gen34 close-reserve target was `15:02:29Z`. On the next inspection, automation `last_run_time=15:00:40Z`, so an exact actual wake attributable to `15:02:29Z` is NOT_PROVEN; do not fabricate WAKE_OK. Current invocation nevertheless proves WORK_OK and durable progress.
2. Writer authority: at gen35 bootstrap the live canonical target was `00:16:30 KST` = `15:16:30Z`, +841s relative to the attributed `15:02:29Z` target, with no matching attributed writer identified in the inspected evidence. Combined with the earlier `15:01:31Z -> 14:51:15Z` overwrite, this strengthens the need to test scheduler writer authority rather than retry representation unchanged. Writer identity remains UNATTRIBUTED; timing alone is not attribution.
3. H-O8-SCHEDULER-WRITER-FENCE is now ACTIVE TESTABLE. Primary variable only: `ALL_WAKES_PREARM` versus `OWNER_OR_NEWLY_ACQUIRED_OWNER_ONLY`; full-VEVENT representation, RRULE, exact_schedule, enabled, +840s normal offset, generation CAS, role-relative stop semantics, and work admission remain fixed.
4. Gen35 legally consumed `O8_LONGWAKE_GEN34_TO_GEN35_001` with READY + fresh-SHA CAS and became generation35 ACTIVE_OWNER. Prompt `2.2.13-OWNER-FENCED-SCHEDULER / MER-OPT-2Q` activates B: SHADOW does not scheduler-write; current/newly-acquired owner writes after authority resolution.
5. B01 owner-fenced sample wrote exactly one attributed normal prearm to `00:17:18 KST` = `15:17:18Z`; immediate live readback matched same ID, enabled=true, exact_schedule, RRULE, exact DTSTART. WRITE_OK+STATE_OK PASS; later target stability and WAKE_OK remain pending.
6. Promotion still requires minimum three clean B samples and the mandatory owner-loss adverse case. Reject B if owner-loss strands continuation or materially worsens recovery without compensating reliability gain.

### O8 repair-first incident rule
Repeated unchanged failure is invalid work. Sequence: FAILURE_OBSERVED -> ROOT_CAUSE_IDENTIFIED -> MECHANISM_CHANGED -> DIFFERENTIAL_VERIFICATION -> RESUME. +180s is continuity fallback only when same-turn continuation/repair is genuinely unavailable; it is not a substitute for diagnosis.

## Current convergence gate
Do not mark PROGRAM_COMPLETE while writer-fence B lacks its clean-sample minimum/adverse owner-loss evidence or while unexplained scheduler overwrite remains material. Reassess final convergence using continuity, long-wake utilization, recovery, duplicate-side-effect, scheduler-overwrite, and simplicity evidence together.
