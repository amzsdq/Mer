# Mer Master Plan v3 — Empirical Relay Optimizer

Status: ACTIVE
Primary objective: continuity first, then maximum long-run useful-work utilization, then simplicity.

## 0. Research discipline

Each wake follows `status/program.json.next_step`.
Do not promote theory directly into production policy.

### Hypothesis sourcing gate — REQUIRED

Before a new hypothesis becomes TESTABLE, follow `research/HYPOTHESIS_SOURCING_POLICY.md`.

At minimum perform a bounded scan of:
- relevant Mer/tEST/workwork empirical evidence;
- authoritative implementation references for the same primitive;
- academic/formal research when an established literature exists;
- at least one competing interpretation, counterexample, or failure mode.

Maintain reusable references in `research/REFERENCE_BASE.md`.

Every new hypothesis must state:
- supporting priors;
- counter-priors / known conflicts;
- what is transferable to Mer;
- what cannot be assumed to transfer;
- the exact discriminating test and rejection/revision condition.

If no useful prior art is found after a bounded search, label the hypothesis `EXPLORATORY_UNSOURCED`; do not silently treat intuition as established engineering knowledge.

For every candidate:
1. DEFINE hypothesis and metric.
2. Establish baseline/control.
3. Change one primary variable at a time.
4. Run repeated samples where the outcome is stochastic.
5. Separate WRITE_OK / STATE_OK / WAKE_OK / WORK_OK.
6. CLASSIFY evidence.
7. KEEP, REVISE, or REJECT the hypothesis.
8. Use the failure/success result to choose the next discriminating hypothesis.

Do not repeat a converged or failed sample unchanged without a diagnostic reason.

External/repository research is PRIOR evidence:
- `amzsdq/tEST`
- `amzsdq/workwork`
- mature distributed-systems case studies
It supplies hypotheses/protocols, not automatic truth.

Dynamic runtime authority remains singular:
- `status/program.json` owns current_stage, next_step, active_execution and the active hypothesis.
- `control/active.json` is static bootstrap indirection.
- execution/evidence files are authoritative only when referenced by program state.

## Stage O0 — Evidence intake and reopened objective — COMPLETE

Imported useful prior findings from tEST/workwork into:
`research/PRIOR_EVIDENCE_2026-09-23.md`.

Previous prompt/GitHub-boundary work is retained as prior evidence, not treated as completion of the top-level goal.

## Stage O1 — Immediate-prearm continuity baseline — ACTIVE

### Hypothesis H-SCHED-1
After minimal bootstrap/control reads, securing the next wake before substantive work improves continuation survival compared with close-time rearm, without materially reducing useful-work duty cycle.

Baseline candidate:
- SAME automation
- recurring RRULE:FREQ=HOURLY
- exact_schedule
- enabled=true
- one scheduler writer
- prearm once near wake start, before substantive work
- no normal close-time scheduler rewrite
- recurrence retained as cold fallback

Measure:
- wake_to_schedule_write_sec
- WRITE_OK
- STATE_OK
- actual later WAKE_OK
- WORK_OK
- useful_work_sec
- control_overhead_sec
- actual_idle_gap_sec when directly observable
- duplicate/concurrent invocation
- missed intended wake

Gate:
- obtain repeated clean end-to-end samples sufficient to establish a baseline;
- at least one actual later wake must retrospectively validate each promoted sample;
- then compare against a close-time/post-bootstrap scheduling control or justify from direct prior Mer evidence if equivalent.

Failure handling:
- if immediate prearm causes overlap, lost work, or materially worse utilization, revise timing/lead rather than assuming the hypothesis.

## Stage O2 — Prompt enforcement, representation, and versioned sync

Reference:
- `research/PROMPT_REPRESENTATION_PRIORS.md`

### O2A — Authority placement

#### H-PROMPT-1
Stable behavioral invariants in the injected prompt are followed more reliably than repo-only copies.

Use existing Mer Stage-1 conflict/delegation results as prior.
Replicate only the discriminating conflict/delegation cases needed to confirm the result under the current optimizer kernel/model.

### O2B — Prompt representation and rule encoding

Do not conflate syntax format with semantic rigidity.

#### O2B-1 — Representation syntax
Primary variable: section/serialization syntax only.

Candidates:
- F1_MARKDOWN_DIRECT
- F2_FLAT_KV_WRAPPER
- F3_JSON_WRAPPER
- F4_XML_WRAPPER

Hold the same atomic propositions, ordering, authority channel, model/reasoning policy, fixtures, and dynamic state constant.
Do not change a nuanced rule into a simpler enum merely to fit a format.

Measure:
- critical invariant adherence;
- task completion quality;
- omission/misinterpretation;
- control/token overhead;
- parse/boundary confusion;
- repair/sync burden.

#### O2B-2 — Lexical action naming / anchors
Primary variable: how the same action/invariant is named, while surrounding semantics remain fixed.

Examples for the same scheduling action:
- natural-language Korean/English label: `예약 갱신` / `update the schedule`
- canonical symbolic name: `SCHEDULE_UPDATE`
- XML-style semantic tag: `<schedule_update>...</schedule_update>`
- function/command-like anchor: `schedule.update(...)`

Test whether explicit canonical action names/tags improve retrieval, disambiguation, and compliance or merely add ceremony.
Do not mix this test with changes to the underlying rule semantics or scheduler behavior.

#### O2B-3 — Rule encoding rigidity
Run after syntax and lexical-anchor baselines.

Primary variable: how the same rule is encoded.
Compare, within the selected representation/anchor convention:
- natural-language imperative;
- declarative key/value or enum/boolean constants where semantically lossless;
- hybrid: constants/identifiers as machine-like fields, nuanced behavioral rules as direct natural language.

Do not hardcode dynamic runtime state.

Measure whether exact constants improve compliance without losing conditional/behavioral meaning.

#### O2B-4 — Relay canary
Promote only the best materially distinct candidates from O2B-1/O2B-2/O2B-3 to real relay canary testing.
If candidates are operationally tied, choose the simpler/shorter/more maintainable representation.

### O2C — Versioned canonical/deployed sync

#### H-SYNC-1
GitHub canonical source + deployed prompt copy + cheap version comparison prevents silent drift at negligible steady-state cost.

Test:
- version match fast path;
- version mismatch -> PREPARE/DEPLOY/VERIFY/ACTIVATE;
- stale/failed deploy rollback or safe stop;
- version/id changed without unnecessary full canonical reread when already matched.

Promotion:
- stable invariants may intentionally exist in both GitHub canonical and deployed prompt;
- changing runtime state remains GitHub-only;
- representation format is selected by O2B evidence, not by preference.

## Stage O3 — Runtime / productive-window / completion-envelope optimization

Use workwork evidence as prior, then validate Mer-specific behavior.

Separate:
1. SURVIVAL_BOUNDARY
2. PRODUCTIVE_WINDOW
3. COMPLETION_ENVELOPE

Measure direct close overhead; do not invent a reserve.

Candidate policies, in increasing complexity:
- P1 fixed threshold
- P2 soft cutoff + hard cap
- P3 estimated-next-unit admission
- P4 adaptive admission

Prefer the simplest policy that achieves statistically/operationally indistinguishable continuity and duty cycle.

The old ~600s target is only a starting probe value, not a completion rule.

## Stage O4 — Planned-gap / wake-lead optimization

After the prearm/runtime policy is stable enough, vary one timing parameter at a time.

Start from an evidence-supported safe region, then test shorter gaps, e.g.:
3m -> 2m -> 1m, with intermediate values if the boundary lies between them.

Measure actual final scheduler-write-to-wake lead, wake lateness, idle gap, overlap, and missed occurrence.

Choose the smallest gap/lead region that does not materially worsen continuity/recovery.

## Stage O5 — Overlap / shadow successor only if justified

H-OVERLAP-1:
A shadow successor that wakes before owner close may hide bootstrap/idle cost and improve long-run duty cycle.

Do not adopt by elegance.
Only test if the best non-overlap policy leaves material idle/bootstrap loss.

Required controls:
- exactly one ACTIVE_OWNER
- CAS/generation authority transfer
- one scheduler writer
- immutable per-invocation evidence
- zero duplicate authoritative side effects

Compare against best non-overlap candidate on:
- authoritative useful-work duty cycle
- handoff gap
- overlap waste
- duplicate/race failures
- complexity/overhead

Reject overlap if benefit is negligible or reliability worsens.

## Stage O6 — Adverse recovery tests

Inject or safely simulate:
- missed intended near-term wake while recurrence remains
- turn termination after checkpoint but before normal finish
- stale durable state
- duplicate/competing actor
- prompt version mismatch
- missing bootstrap entrypoint
- scheduler write accepted but later wake absent

A fast candidate without demonstrated recovery is rejected.

## Stage O7 — Final convergence

A final candidate must include:
- scheduler strategy
- prompt/canonical sync strategy
- operating runtime/admission rule
- close reserve/safety margin method
- planned gap / successor lead
- checkpoint ordering
- recovery/fallback
- authority fencing if overlap is used

Final validation requires, at minimum:
- repeated clean end-to-end wakes under the unchanged candidate;
- zero duplicate authoritative side effects in validation;
- retrospective WAKE_OK/WORK_OK evidence;
- adverse recovery success;
- measured useful-work duty cycle and idle gap where directly observable;
- comparison against at least one plausible simpler/competing policy;
- documented rejected hypotheses and why they failed.

Do not mark COMPLETE while a material uncertainty in continuity or utilization remains untested and testable.

## Wake utilization rule

A wake is a work session, not a one-step callback.
Continue plan-defined safe units while runtime allows.
Never sleep, pad, or generate meaningless work.
Reserve enough time for durable state close.
Scheduler continuation should be secured according to the currently declared experimental strategy before taking risks that could strand the relay.
