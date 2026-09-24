# O7 Final Convergence Matrix — authoritative pass after final-kernel sample 2

Current owner: MER-O7-GEN30-SAMPLE2-20260924T2248KST / generation 30

| Gate | Evidence | Current verdict |
|---|---|---|
| Scheduler strategy specified | O4 retained fixed/default 840s comparator; adaptive shortening only for concrete successor-dependent boundaries | PASS |
| Prompt/canonical sync specified | deployed/canonical 2.2.10-EARLY-EXIT-RETRY / MER-OPT-2M, manifest fast path | PASS |
| Work/stop rule specified | no duration target; normal stops only PROGRAM_COMPLETE or SUCCESSOR_HANDOFF_COMPLETE; recoverable blocker is work | PASS |
| Authority fencing | generation + fresh-SHA CAS; exactly one substantive owner | PASS |
| Repeated clean handoffs | final-kernel sample 1 gen28->29 clean; sample 2 gen29->30 clean | PASS_REPEATED_2 |
| Duplicate authoritative side effects | zero in admitted O4/O5/O6 evidence and final-kernel samples 1-2 | PASS |
| Retrospective WAKE_OK / WORK_OK | prior live 22:11:35 target -> 22:11:41 actual wake (+6s); sample2 started 32s before prior intended 22:49:15 target, so that delta is overlap, not lateness/idle | PASS_WITH_EARLY_OVERLAP_OBSERVED |
| Adverse recovery | O6 orphan suite 5/5; observed-state suite 4/4; live wake reconciliation PASS | PASS_WITH_PROVIDER_MISS_RATE_UNKNOWN |
| Simpler comparator | 720s not promoted over 840s; O5 zero-post-READY-unit candidate not promoted over established A | PASS |
| Rejected/revised hypotheses ledger | O4/O5 revisions plus stranded-owner and WRITE/STATE inference rejection retained | PASS |
| Direct duty-cycle / idle accounting | direct method active; sample2 useful unit bounded by GitHub commits 13:49:03Z -> 13:49:13Z = 10s. Wake START was 13:48:43Z; prior target 13:49:15Z, proving target delta must not be counted as idle. Remaining unbounded intervals stay UNKNOWN, not USEFUL. | PASS_METHOD_AND_FIRST_OBSERVATION |
| Repeated unchanged clean wakes under final candidate | samples 1 and 2 clean under unchanged fixed/default 840s + generation CAS + post-READY max1 + kernel 2.2.10 | PASS_REPEATED_2 |
| Material continuity/utilization uncertainty still testable? | yes: direct accounting coverage is still sparse for a defensible long-run duty-cycle estimate; provider miss rate remains unknown. These are measurement uncertainties, not demonstrated continuity failures. | OPEN_MEASUREMENT_COVERAGE |

## Final-kernel sample 2
- Candidate unchanged: fixed/default 840s, generation CAS, post-READY max authoritative units=1, kernel 2.2.10.
- Successor START GitHub timestamp: 2026-09-24T13:48:43Z.
- Prior intended scheduler target: 2026-09-24T13:49:15Z; successor began 32s early. Classification: EARLY_OVERLAP_NOT_IDLE.
- Wake-start prearm live readback: same canonical, enabled=true, exact_schedule, hourly RRULE, DTSTART 2026-09-24 23:02:25 KST.
- Successor READY: evidence/o7/O7_FINAL_KERNEL_GEN29_TO_GEN30_SAMPLE2_READY_20260924T2248KST.json.
- Fresh-SHA ownership CAS: gen29 -> gen30 succeeded.
- Direct useful interval: evidence/o7/O7_SAMPLE2_USEFUL_UNIT_START_20260924T2249KST.json -> evidence/o7/O7_SAMPLE2_USEFUL_UNIT_END_20260924T2249KST.json = 10s by GitHub server timestamps.
- Duplicate authoritative side effects observed: 0.

## Rejected / revised hypothesis ledger
1. H-O4 shorter fixed 720s is materially superior to fixed 840s: REVISE / NOT PROMOTED.
2. H-O5 post-READY max authoritative units=0 is materially superior to established max=1: REVISE / NOT PROMOTED.
3. Consumed handoff epoch with no next legal successor path is acceptable nonterminal exit: REJECT.
4. Scheduler WRITE_OK/STATE_OK can stand in for later WAKE_OK/WORK_OK: REJECT.
5. Scheduler target-to-start delta can always be interpreted as idle/lateness: REJECT; sample2 directly observed early overlap.

## Decision
NOT COMPLETE YET. Do not disable the canonical automation. Core final-kernel continuity gates are now repeated-clean. Continue only the remaining material measurement gate: obtain additional directly bounded USEFUL/CONTROL/IDLE observations sufficient to characterize duty cycle without allocating UNKNOWN time to USEFUL. Provider miss rate remains UNKNOWN unless actual miss evidence accumulates.
