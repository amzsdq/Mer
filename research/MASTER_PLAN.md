# Mer Master Plan v3 — Empirical Relay Optimizer

Status: ACTIVE
Primary objective: continuity first, then maximum long-run useful-work utilization, then simplicity.

## 0. Research discipline

Each wake follows `status/program.json.next_step`.
Do not promote theory directly into production policy.

### Hypothesis sourcing gate — REQUIRED
Before a new hypothesis becomes TESTABLE, follow `research/HYPOTHESIS_SOURCING_POLICY.md`.
Use internal empirical evidence, authoritative implementation references, applicable formal work, and competing/counter evidence. A candidate is KEEP/REVISE/REJECT only after a discriminating test. Dynamic runtime authority remains `status/program.json`; execution/evidence files are authoritative only when referenced there.

## Stage O0 — Evidence intake and reopened objective — COMPLETE
Prior tEST/workwork evidence remains prior evidence, not automatic Mer truth.

## Stage O1 — Controlled-overlap / baton baseline — ACTIVE

Reference: `research/WORKWORK_OVERLAP_INTAKE.md`.
Prior fact: workwork observed predecessor/successor concurrency. This establishes overlap feasibility only, not clean handoff.

### Current baseline candidate
- same recurring automation; `RRULE:FREQ=HOURLY`; `exact_schedule`; enabled
- no work-duration target or voluntary time cutoff
- every invocation performs exactly one verified scheduler pre-arm immediately on wake: `wake + 840s`; no later scheduler mutation in that wake
- scheduler pre-arm is continuity plumbing and is independent of substantive ownership; SHADOW may perform this one write
- exactly one substantive ACTIVE_OWNER at a time, fenced by durable generation/CAS
- successor begins as SHADOW_SUCCESSOR and may prepare/read/write immutable own evidence
- normal voluntary stop gates are only `PROGRAM_COMPLETE` or `SUCCESSOR_HANDOFF_COMPLETE`
- `SUCCESSOR_HANDOFF_COMPLETE` requires actual successor WAKE_OK/READY plus durable generation-increment ownership transfer
- predecessor may not voluntarily close before transfer; late/missing successor means predecessor continues genuine useful work
- after transfer predecessor stops authoritative work and closes; successor continues prepared work as sole substantive owner

### Evidence required per baton sample
Record predecessor/successor invocation ids, actual wake evidence, verified +840s pre-arm, READY evidence, generation before/after, transfer/accept evidence, authoritative work around transfer, handoff gap, duplicate authoritative side effects, scheduler conflicts, checkpoint loss, WRITE_OK/STATE_OK/WAKE_OK/WORK_OK, useful work/control overhead where observable.

### Clean sample
CLEAN requires actual successor wake while predecessor remains active; exactly one substantive owner before and after transfer; successor READY before/at transfer; successful generation/CAS transfer; no predecessor authoritative side effect after transfer; successor accepts fresh generation and resumes useful work; no duplicate authoritative side effect, scheduler conflict, or checkpoint loss. Concurrent wake without clean transfer is not a clean handoff.

### Gate O1A / O1B
O1A requires at least 3 clean handoffs. O1B requires at least 5 clean end-to-end handoffs under an unchanged candidate before timing optimization. Preserve prior non-overlap evidence as comparator.

### Active failure domain — owner loss
The first owner-mediated sample exposed a liveness hole when the durable OWNER disappears before consuming a READY successor. `H-O1-OWNER-LOSS-RECOVERY` owns this subproblem. Do not count that sample CLEAN. Prefer a simpler non-time-based takeover if it demonstrates equivalent safety/recovery; retain per-side-effect generation fencing because takeover/leader election alone is not fencing.

## Stage O2 — Prompt enforcement / representation / versioned sync
Retain existing prompt-boundary research as planned work. Stable invariants belong in deployed prompt plus canonical GitHub source; dynamic runtime state remains GitHub-only. Compare representation/anchors only with semantics held constant. Version mismatch uses prepare/deploy/verify/activate; version match uses cheap manifest fast path.

## Stage O3 — Productive-window / completion-envelope observation
There is no fixed work-duration completion rule in the current baton candidate. Measure platform survival boundary, productive window, close overhead, and interruption behavior observationally. A duration threshold may be tested later only if evidence shows it improves continuity/utilization; it must not silently become a stop gate.

## Stage O4 — Wake-prearm offset optimization
After baton handoff is viable, keep +840s as the fixed baseline and test an adaptive pre-arm candidate. Adaptive policy defaults to 840s; when a concrete successor-dependent experiment/verification boundary is expected sooner, select PREARM_OFFSET_SEC as expected useful-work time to that boundary plus handoff/jitter margin, normally bounded to 180..840s. Do not shorten merely to increase wake frequency. Hold the selected offset constant within each sample unless offset itself is the primary variable. Measure actual wake lateness, overlap, handoff gap, missed occurrence, recovery, scheduler overhead, and useful-work duty cycle. Compare adaptive behavior against fixed-offset controls; prefer the simpler fixed policy if reliability/utilization are equivalent.

## Stage O5 — Handoff optimization
Optimize READY policy, safe-unit granularity near transfer, ownership recovery, and fencing only after O1 viability. An ELIGIBLE READY successor that is durably bound to the current OPEN handoff epoch and generation has transfer priority: the predecessor finishes only its current atomic authoritative unit, starts no new authoritative unit, and yields to the handoff CAS. READY alone without current epoch/generation binding does not grant priority. Scheduler has one wake-start write per invocation; substantive ownership remains generation fenced. Compare complex recovery against the simplest safe alternative.

## Stage O6 — Adverse recovery tests
Test missed intended wake, predecessor termination, stale state, duplicate/competing actor, prompt mismatch, missing bootstrap, and accepted scheduler write followed by absent wake. Reject fast candidates without demonstrated recovery.

## Stage O7 — Final convergence
Final candidate must specify scheduler strategy, prompt/canonical sync, work/stop rule, checkpoint ordering, recovery/fallback, and authority fencing. Require repeated unchanged clean wakes, zero duplicate authoritative side effects, retrospective WAKE_OK/WORK_OK, adverse recovery success, measured duty cycle/idle gap where observable, a simpler comparator, and documented rejected hypotheses. Do not mark COMPLETE while material continuity/utilization uncertainty remains testable.

## Wake utilization rule
A wake is a work session, not a one-step callback. Continue genuine plan-defined safe units until `PROGRAM_COMPLETE` or `SUCCESSOR_HANDOFF_COMPLETE`; abnormal platform/safety interruption is not successful voluntary completion. Never sleep, pad, repeat converged work, or invent work. Continuation is secured by the current one-write wake-start pre-arm strategy. If repeated nonterminal wakes remain materially short despite the plan permitting substantially longer continuous useful work, no short-cycle experiment requiring rapid successor turnover is active, and safe useful work remains available, record and report `SHORT_CYCLE_ANOMALY` rather than silently normalizing the pattern.


## Cross-stage ownership rule
A research-stage transition is not complete merely because `status/program.json.current_stage` changes. Ownership must also cross the boundary. Prefer a normal handoff into the new stage; if the prior-stage OWNER is already gone, use one explicitly authorized single-use recovery transition tied to the exact stuck generation, consume it once, and pre-open the next normal handoff epoch. Never leave a completed-stage dead OWNER as authority for the next stage.
