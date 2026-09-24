# O4 Adaptive Pre-arm Hypothesis — 2026-09-24 11:31 KST

HYPOTHESIS_ID: H-O4-EXPERIMENT-AWARE-PREARM

CLAIM:
A universal +840s pre-arm should remain the safe baseline, but experiments that intrinsically require a near-term successor observation may declare a shorter pre-arm derived from the expected useful-work/observation horizon plus a safety margin. The selected offset is fixed for the whole sample/window; it is not re-estimated ad hoc every wake.

PRIMARY_VARIABLE:
`wake_start_prearm_offset_sec` only. Ownership, generation CAS, READY policy, recurrence, and stop gates remain unchanged.

SOURCE_CLASS:
- INTERNAL_EMPIRICAL: Mer O1 reached 5/5 clean handoffs under unchanged +840s, establishing +840s as a safe baseline rather than an optimum.
- AUTHORITATIVE_IMPLEMENTATION: AWS Step Functions Wait supports relative or absolute/data-driven waits rather than requiring one hard-coded duration; task timeouts/heartbeats are explicitly chosen relative to task behavior. These are design priors only, not proof of ChatGPT Automation behavior.

SUPPORTING_PRIORS:
1. Mer O1: five consecutive clean baton handoffs at +840s.
2. AWS Step Functions Wait permits runtime-selected `SecondsPath`/`TimestampPath`, demonstrating that workflow timing can be state/input dependent.
3. AWS recommends reasonable task timeouts and heartbeat intervals rather than a universal timeout for all work.

COUNTER_PRIORS / KNOWN_CONFLICTS:
1. Shorter offsets increase overlap/control overhead and may expose scheduler jitter or missed-wake behavior.
2. Per-wake free-form model estimation would confound the experiment and create parameter churn.
3. O1 evidence validates +840s only; no Mer evidence yet establishes a safe lower bound.

TRANSLATION TO MER:
- `+840s` remains fallback/default.
- A shorter offset is permitted only when an active experiment explicitly declares that the successor wake is itself required for the next discriminating observation.
- The experiment declares `expected_observation_horizon_sec`, `safety_margin_sec`, and a single resulting `wake_start_prearm_offset_sec` before the sample begins.
- Offset remains unchanged for all repetitions in that sample/window.
- Candidate bounds are experimental guardrails, not permanent constants; initial O4 tests must descend conservatively from 840s.

NON_TRANSFERABLE_ASSUMPTIONS:
AWS service scheduling semantics, latency, timeout enforcement, and delivery guarantees do not transfer to ChatGPT Automations. Only the principle of state/task-dependent timing is imported; Mer must measure actual wake behavior.

DISCRIMINATING_TEST:
A/B by fixed windows, not by wake-by-wake retuning.
A = +840s baseline window.
B = one shorter fixed offset selected from observed experiment horizon + explicit margin.
Hold ownership/CAS/handoff semantics constant. For each window measure actual scheduler lead, wake lateness, overlap duration, handoff gap, missed intended wake, scheduler conflict, duplicate authoritative side effect, and useful/control work where observable.

PROMOTION_GATE:
Promote experiment-aware pre-arm only if a shorter declared offset materially reduces experiment turnaround/idle gap without reducing clean-handoff continuity or increasing conflicts/recovery burden relative to +840s baseline. Prefer a fixed simple offset if equivalent.

REJECTION / REVISION RULE:
- REJECT free-form per-wake estimation if it creates uncontrolled parameter churn.
- REVISE upward if shorter offset increases missed/late wakes or unsafe overlap.
- KEEP +840s as default if adaptive timing gives no material utilization/iteration benefit.

IMMEDIATE TEST POLICY:
Do not change the current already-prearmed wake. At the next O4 sample boundary, first reconcile stale `spec/execution.json`, then define a fixed-window baseline and exactly one shorter candidate. No scheduler offset changes inside a sample window.
