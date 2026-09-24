# O7 Final Convergence Matrix — accounting coverage package 1

Current owner: MER-O7-GEN31-COVERAGE1-20260924T2252KST / generation 31

| Gate | Evidence | Current verdict |
|---|---|---|
| Scheduler strategy specified | O4 retained fixed/default 840s comparator; adaptive shortening only for concrete successor-dependent boundaries | PASS |
| Prompt/canonical sync specified | deployed/canonical 2.2.10-EARLY-EXIT-RETRY / MER-OPT-2M, manifest fast path | PASS |
| Work/stop rule specified | no duration target; normal stops only PROGRAM_COMPLETE or SUCCESSOR_HANDOFF_COMPLETE; recoverable blocker is work | PASS |
| Authority fencing | generation + fresh-SHA CAS; exactly one substantive owner | PASS |
| Repeated clean handoffs | final-kernel samples gen28->29, gen29->30, and accounting coverage gen30->31 all clean | PASS_REPEATED_3 |
| Duplicate authoritative side effects | zero in admitted O4/O5/O6 evidence and final-kernel/accounting handoffs | PASS |
| Retrospective WAKE_OK / WORK_OK | prior live +6s wake; sample2 early overlap; coverage1 actual wake resumed work and completed fresh-SHA gen30->31 transfer | PASS |
| Adverse recovery | O6 orphan suite 5/5; observed-state suite 4/4; live wake reconciliation PASS | PASS_WITH_PROVIDER_MISS_RATE_UNKNOWN |
| Simpler comparator | 720s not promoted over 840s; O5 zero-post-READY-unit candidate not promoted over established A | PASS |
| Rejected/revised hypotheses ledger | O4/O5 revisions plus stranded-owner, WRITE/STATE inference, and target-delta-as-idle rejection retained | PASS |
| Direct duty-cycle / idle accounting | method active. Sample2: direct USEFUL=10s. Coverage1: direct CONTROL 13:54:45Z->13:54:51Z=6s; direct USEFUL 13:54:56Z->13:55:04Z=8s. Unbounded intervals remain UNKNOWN. No true IDLE interval is claimed without predecessor END + successor START non-overlap proof. | PASS_ADDITIONAL_COVERAGE |
| Repeated unchanged clean wakes under final candidate | 3 clean transfers under unchanged fixed/default 840s + generation CAS + post-READY max1 + kernel 2.2.10 | PASS_REPEATED_3 |
| Material continuity/utilization uncertainty still testable? | yes, but narrowed: direct accounting now spans two final-candidate wakes and includes both USEFUL and CONTROL classes. A defensible long-run duty-cycle estimate still lacks enough bounded coverage, and provider miss rate remains unknown. Neither is evidence of a continuity failure. | OPEN_MEASUREMENT_COVERAGE |

## Coverage package 1
- Wake START GitHub timestamp: 2026-09-24T13:54:24Z.
- Wake-start prearm live readback: same canonical, enabled=true, exact_schedule, hourly RRULE, DTSTART 2026-09-24 23:06:51 KST.
- Successor READY: evidence/o7/O7_ACCOUNTING_GEN30_TO_GEN31_COVERAGE1_READY_20260924T2253KST.json.
- Fresh-SHA ownership CAS: gen30 -> gen31 succeeded.
- CONTROL interval: evidence/o7/O7_COVERAGE1_CONTROL_START_20260924T2254KST.json -> O7_COVERAGE1_CONTROL_END_20260924T2254KST.json = 6s by GitHub server timestamps.
- USEFUL interval: evidence/o7/O7_COVERAGE1_USEFUL_START_20260924T2255KST.json -> O7_COVERAGE1_USEFUL_END_20260924T2255KST.json = 8s by GitHub server timestamps.
- Directly classified coverage in this package: USEFUL=8s, CONTROL=6s, IDLE=0s proven; all other elapsed time remains UNKNOWN rather than being allocated to USEFUL.
- Duplicate authoritative side effects observed: 0.

## Rejected / revised hypothesis ledger
1. H-O4 shorter fixed 720s is materially superior to fixed 840s: REVISE / NOT PROMOTED.
2. H-O5 post-READY max authoritative units=0 is materially superior to established max=1: REVISE / NOT PROMOTED.
3. Consumed handoff epoch with no next legal successor path is acceptable nonterminal exit: REJECT.
4. Scheduler WRITE_OK/STATE_OK can stand in for later WAKE_OK/WORK_OK: REJECT.
5. Scheduler target-to-start delta can always be interpreted as idle/lateness: REJECT; direct overlap evidence exists.

## Decision
NOT COMPLETE YET. Core continuity/recovery/fencing/simplicity gates are converged. Continue only material measurement coverage: add directly bounded USEFUL/CONTROL intervals and claim IDLE only from actual non-overlap evidence. Do not inflate UNKNOWN. Provider miss rate remains UNKNOWN unless actual miss evidence accumulates.
