# Mer Master Plan v3 — Empirical Relay Optimizer

Status: ACTIVE
Current stage: authoritative in `status/program.json`.
Primary objective: continuity first, then long-run useful-work utilization, then simplicity.

## 0. Research discipline
Follow `status/program.json.next_step`. New hypotheses obey `research/HYPOTHESIS_SOURCING_POLICY.md`. GitHub server/external timestamps, not model-written clocks, govern temporal claims.

## Stage O0 — Evidence intake — COMPLETE WITH LATER CORRECTION
Prior tEST/workwork evidence is prior only. O8 later invalidated the old workwork 198s overlap claim after GitHub commit timestamps contradicted the model-written duration/order; see corrected `research/WORKWORK_OVERLAP_INTAKE.md`.

## Stage O1 — Baton/ownership baseline — HANDOFF EVIDENCE RETAINED; OVERLAP TIMING CLAIM WITHDRAWN
O1 established useful generation/CAS, READY, exactly-one-owner and single-use recovery mechanics under tested conditions. O1A/O1B handoff records remain historical state-machine evidence unless separately invalidated.

However the prior justification that workwork had empirically proven 198s same-canonical overlap is withdrawn. Server chronology for that probe is primary-start commit 16:17:32Z, primary-end commit 16:18:57Z, observer commit 16:29:46Z. The claimed 01:32:40 primary end existed in a file committed at 01:18:57, so it cannot be clock authority. Do not use O1/workwork as proof that a successor can overlap a live predecessor.

## Stage O2 — Prompt enforcement / representation / versioned sync
Stable invariants live in deployed prompt plus canonical GitHub source; dynamic runtime state remains GitHub-only. Version mismatch uses prepare/deploy/verify/activate. Prompt manifest must not duplicate live scheduler runtime state.

## Stage O3 — Productive-window observation
No fixed voluntary work-duration completion rule. Measure survival/productive window, close overhead and interruption behavior externally. Duration thresholds are validation gates, not stop gates.

## Stage O4 — Wake-prearm offset optimization
840s remains fixed baseline during current writer-fence experiment. Offset changes require their own primary-variable test. Measure actual wake, idle gap, missed occurrence, recovery and useful-work duty cycle rather than nominal DTSTART alone.

## Stage O5 — Handoff optimization
Eligible READY bound to current OPEN epoch/generation has transfer priority only when concurrent successor existence is actually established. Fresh-SHA generation CAS fences substantive authority. Do not assume provider overlap merely because the state machine supports it.

## Stage O6 — Adverse recovery
Test missed wake, owner termination, stale state, duplicate contender, prompt mismatch, missing bootstrap, accepted scheduler write followed by absent wake, and owner-loss under writer fencing.

## Stage O7 — Continuity subgate PASS; overall completion invalidated
Preserve continuity/recovery evidence, but prior PROGRAM_COMPLETE was invalid because long single-wake work remained unresolved.

## Wake utilization rule
A wake is a work session. Acquisition starts ACTIVE_OWNER tenure; it is not the acquiring invocation's stop gate. Continue genuine safe work until PROGRAM_COMPLETE or a later distinct successor handoff away if such a successor actually exists. Never sleep/pad/repeat converged work.

## Stage O8 — Long-wake / repair-first reliability

### O8A — Role-relative stop gate
PASS.

### O8B — Long-wake gate
PASS. Gen34 GitHub START `14:47:51Z`, END `15:00:07Z`, WORKED=736s with multiple genuine units.

### O8C — Work packing
Long work is feasible. Further packing optimization only if duty-cycle evidence warrants it.

### O8D — Scheduler representation / writer authority / dispatch topology
Full absolute recurring VEVENT + exact readback is KEEP for representation. WRITE_OK, STATE_OK, WAKE_OK and WORK_OK remain separate.

Writer-fence B changes only scheduler writer eligibility: SHADOW zero-write; ACTIVE_OWNER or post-CAS new owner writes. Promotion requires deterministic oracle WF-N0..N6 + WF-S1..S4 for >=3 CLEAN normal samples plus owner-loss adverse WF-A1..A6.

B evidence:
- B01/gen35: immediate WRITE_OK+STATE_OK; later target wake/stability not proven => NOT_CLEAN.
- B02/gen36: SHADOW zero-write, CAS, one owner prearm, immediate WRITE/STATE/WORK PASS; target intentionally superseded => SUPERSEDED_NOT_CLEAN.
- B03/gen37: START 15:24:20Z; CAS 15:24:39Z; one owner prearm target 15:38:39Z; exact immediate STATE_OK; target still intact at GitHub 15:29:15Z. Original pre-mutation intent omitted protocol-required fields and needed post-hoc addendum => WF-N0 FAIL, ATTRIBUTION_INCOMPLETE_NOT_CLEAN. B03 remains useful for stability and dispatch-topology observation.
- B04+ must use `research/O8_CLEAN_B_SAMPLE_TEMPLATE.md`.

### O8E — Same-canonical dispatch topology observation
ACTIVE. The old workwork overlap proof is invalidated by server-clock audit, and bounded official documentation did not specify same-task concurrency. B03 target 15:38:39Z is therefore a direct Mer discriminator. Use only server/external ordering; interpretation is precommitted in `research/O8_DISPATCH_TOPOLOGY_DECISION_TREE.md`.

### O8F — Owner-loss adverse
NEXT after the B03 topology observation. Use `research/O8_OWNER_LOSS_ADVERSE_PROTOCOL.md` and `research/O8_OWNER_LOSS_TRANSITION_SCHEMA.md`; no authority corruption, no SHADOW scheduler write, one explicit single-use transition, fresh-SHA recovery CAS, post-CAS owner rearm, measured recovery latency.

## Repair-first rule
Repeated unchanged failure is invalid work: FAILURE_OBSERVED -> ROOT_CAUSE_IDENTIFIED -> MECHANISM_CHANGED -> DIFFERENTIAL_VERIFICATION -> RESUME. +180s is fallback only when same-turn continuation truly cannot execute.

## Current convergence gate
PROGRAM_COMPLETE=NO. Resolve same-canonical dispatch topology if material, obtain valid CLEAN writer-fence samples/adverse evidence or reject/revise that candidate, retain long-wake PASS, and compare the surviving policy against a simpler competitor on actual continuity/recovery/duty-cycle evidence.
