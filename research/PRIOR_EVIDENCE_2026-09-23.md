# Prior Evidence Intake — 2026-09-23

Status: PRIOR_ONLY_NOT_PROMOTED

## Rule
Evidence from other repositories is a source of hypotheses, protocols, and adverse-test ideas. It is not automatically authoritative for Mer. A candidate becomes Mer policy only after Mer-side reproduction or a justified equivalence argument recorded in evidence.

## amzsdq/tEST candidates
1. Same-automation recurring RRULE self-shift appears technically viable, but schedule-write acceptance must be separated from live state and actual wake delivery.
2. Near-term DTSTART reliability is uncertain and must be measured by actual final-write-to-wake lead, not nominal requested interval alone.
3. Relay success has four distinct layers: WRITE_OK, STATE_OK, WAKE_OK, WORK_OK.
4. One primary experimental variable at a time; repeated samples plus adverse testing before promotion.
5. Simpler scheduler mutation counts should be preferred when reliability/recovery are equivalent.

## amzsdq/workwork candidates
1. S1 ACTIVE_OWNER_IMMEDIATE_PREARM: after minimal bootstrap, secure the next wake before substantive work.
2. Separate SURVIVAL_BOUNDARY, PRODUCTIVE_WINDOW, and COMPLETION_ENVELOPE.
3. Derive close reserve from observed close overhead; do not invent a constant.
4. Optimize planned gap only after continuity is stable; candidate sequence 3m -> 2m -> 1m when evidence supports it.
5. Task-admission can use elapsed + estimated_next_unit + close_reserve + safety_margin < hard_cap.
6. Intentional overlap can support SHADOW_SUCCESSOR handoff and near-zero idle, but introduces authority and scheduler-writer races; test only after a simpler baseline is stable.

## Existing Mer evidence retained as prior
1. Direct prompt-vs-repo conflict selected the embedded prompt marker in the observed test.
2. Explicit prompt delegation caused the GitHub dynamic value to be selected.
3. Pointer-only dynamic state stayed fresh but sole-entrypoint recovery was weaker.
4. These are observed routing results, not a universal undocumented platform precedence theorem.

## Initial Mer hypotheses
H-PROMPT-1: Stable behavioral invariants enforced directly in the deployed prompt are followed more reliably than repo-only invariants.
H-SYNC-1: Versioned canonical prompt + deployed-copy comparison at wake prevents silent prompt drift with negligible steady-state overhead.
H-SCHED-1: Immediate prearm before substantive work improves continuation survival versus close-time rearm without materially lowering useful-work duty cycle.
H-GAP-1: Once continuity is stable, reducing planned idle gap improves duty cycle until scheduler jitter/overlap/missed-wake risk materially rises.
H-RUNTIME-1: A measured admission rule using runtime boundary + close reserve yields higher long-run useful work than a fixed arbitrary 600s target.
H-OVERLAP-1: Shadow-successor overlap may further reduce idle, but should be adopted only if it beats the best non-overlap policy after race/duplicate costs.

## Disposition rule
For every hypothesis: DEFINE -> BASELINE -> CONTROLLED TEST -> CLASSIFY -> KEEP / REVISE / REJECT -> next discriminating hypothesis.
A failed hypothesis must update the model; it must not simply be retried unchanged without a diagnostic reason.
