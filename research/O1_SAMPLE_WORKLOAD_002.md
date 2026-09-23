# O1 Sample 002 — Useful Work Packet

Status: COMPLETED_DURING_SAMPLE_OPEN
Hypothesis: H-SCHED-1
Primary variable preserved: SCHEDULER_WRITE_TIMING

## Purpose
Use the productive window for plan-defined analysis that does not alter the O1 scheduler candidate while the sample awaits retrospective WAKE_OK.

## Evidence-quality review

### What Sample 001 established
- The same recurring automation accepted the immediate-prearm schedule write and later produced a resumed invocation.
- WRITE_OK, STATE_OK, WAKE_OK, and WORK_OK were all eventually observed.
- It did not establish clean utilization because the origin wake voluntarily ended after roughly 36 seconds.
- The successor was observed about 80 seconds before the nominal requested DTSTART, so nominal DTSTART must not be treated as precise wake time.

### Consequence for Sample 002+
A clean O1 baseline sample must preserve all of the following:
1. scheduler strategy unchanged from the declared candidate;
2. one scheduler mutation near wake start;
3. useful work continues after prearm instead of treating scheduler success as session completion;
4. actual later wake is required before WAKE_OK=true;
5. utilization and scheduler-timing observations are recorded separately;
6. an early/late wake does not by itself falsify immediate-prearm continuity; timing precision is a later variable unless it causes overlap, missed work, or material utilization loss.

## Confound taxonomy

C1_EXECUTION_POLICY:
The worker stops early despite admissible useful work. This invalidates utilization inference but can leave scheduler-continuity evidence usable.

C2_SCHEDULER_TIMING_JITTER:
Observed wake differs from requested DTSTART. Record signed lateness/earliness; do not silently rewrite the O1 hypothesis into a precision hypothesis.

C3_STATE_WRITE_CONFLICT:
A GitHub optimistic-concurrency conflict occurs while recording evidence. Re-read current state and reconcile; classify separately from scheduler failure.

C4_PROMPT_ROLLOUT:
Changing the deployed prompt during a scheduler sample can alter worker behavior and contaminate utilization comparison. Avoid representation-format experiments until O2.

C5_MULTIPLE_SCHEDULER_WRITES:
Additional schedule rewrites during the same sample change the primary variable and invalidate a clean immediate-prearm sample unless explicitly declared diagnostic.

## Sample acceptance checklist
A sample can count toward the clean O1 gate only if:
- immediate prearm WRITE_OK=true;
- verified live STATE_OK=true;
- origin wake performs nontrivial plan-defined useful work after prearm unless no safe useful work exists;
- no undeclared second scheduler rewrite occurs;
- successor invocation later establishes WAKE_OK=true and WORK_OK=true;
- no material competing-variable change occurred;
- any jitter/conflict is recorded rather than normalized away.

## Comparator preparation
After the clean baseline gate is met, the comparator should change only scheduler-write timing while retaining:
- same automation identity;
- same recurrence;
- same approximate workload class;
- same evidence definitions;
- same close/admission policy where feasible.

Primary comparison metrics:
- continuation success rate;
- wake-to-schedule-write/control overhead;
- useful work per wake;
- actual idle gap where observable;
- duplicate/overlap/missed occurrence;
- recovery burden.

## Decision discipline
Do not promote immediate prearm merely because it feels safer. Promotion requires repeated clean end-to-end evidence and a comparator or a documented equivalence argument. If continuity is tied, prefer the lower-overhead/simpler mechanism.
