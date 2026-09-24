# O7 Final Convergence Matrix — accounting coverage package 2

Current owner: MER-O7-GEN32-COVERAGE2-20260924T2302KST / generation 32

| Gate | Evidence | Current verdict |
|---|---|---|
| Scheduler strategy specified | O4 retained fixed/default 840s comparator; adaptive shortening only for concrete successor-dependent boundaries | PASS |
| Prompt/canonical sync specified | deployed/canonical 2.2.10-EARLY-EXIT-RETRY / MER-OPT-2M, manifest fast path | PASS |
| Work/stop rule specified | no duration target; normal stops only PROGRAM_COMPLETE or SUCCESSOR_HANDOFF_COMPLETE; recoverable blocker is work | PASS |
| Authority fencing | generation + fresh-SHA CAS; exactly one substantive owner | PASS |
| Repeated clean handoffs | final-kernel gen28->29, gen29->30, coverage1 gen30->31, coverage2 gen31->32 all clean | PASS_REPEATED_4 |
| Duplicate authoritative side effects | zero in admitted O4/O5/O6 evidence and final-kernel/accounting handoffs | PASS |
| Retrospective WAKE_OK / WORK_OK | prior live +6s wake; later final-kernel/accounting wakes resumed durable work and completed fresh-SHA transfers | PASS |
| Adverse recovery | O6 orphan suite 5/5; observed-state suite 4/4; live wake reconciliation PASS | PASS_WITH_PROVIDER_MISS_RATE_UNKNOWN |
| Simpler comparator | 720s not promoted over 840s; O5 zero-post-READY-unit candidate not promoted over established A | PASS |
| Rejected/revised hypotheses ledger | O4/O5 revisions plus stranded-owner, WRITE/STATE inference, target-delta-as-idle rejection retained | PASS |
| Direct duty-cycle / idle accounting | method active. Sample2 USEFUL=10s. Coverage1 CONTROL=6s, USEFUL=8s. Coverage2 CONTROL=6s, USEFUL=7s. Unbounded intervals remain UNKNOWN; true IDLE is claimed only from predecessor END -> successor START non-overlap. | METHOD_PASS_BOUNDED_CLASSIFICATION |
| Repeated unchanged clean wakes under final candidate | 4 clean transfers under unchanged fixed/default 840s + generation CAS + post-READY max1 + kernel 2.2.10 | PASS_REPEATED_4 |
| Material continuity/utilization uncertainty still testable by the current micro-coverage method? | No discriminating candidate decision remains. More tiny marker windows would increase classified seconds but cannot identify long-run duty cycle while most elapsed time remains UNKNOWN. A prospective observation-window/coverage criterion would be a new measurement experiment, not evidence needed to choose among the already-converged candidate mechanisms. Provider miss rate remains unknown but no observed miss exists and it does not discriminate the current candidate. | NO_MATERIAL_DISCRIMINATING_UNCERTAINTY |

## Coverage package 2
- Wake START GitHub timestamp: 2026-09-24T14:02:30Z.
- Wake-start prearm live readback: same canonical, enabled=true, exact_schedule, hourly RRULE, DTSTART 2026-09-24 23:16:03 KST.
- Successor READY: `evidence/o7/O7_ACCOUNTING_GEN31_TO_GEN32_COVERAGE2_READY_20260924T2302KST.json`.
- Fresh-SHA ownership CAS: gen31 -> gen32 succeeded.
- CONTROL interval: 2026-09-24T14:02:53Z -> 14:02:59Z = 6s.
- USEFUL interval: 2026-09-24T14:03:07Z -> 14:03:14Z = 7s.
- Directly classified coverage in this package: USEFUL=7s, CONTROL=6s, IDLE=0s proven; all other elapsed time remains UNKNOWN.
- Duplicate authoritative side effects observed: 0.
- Materiality result: repeated micro-coverage no longer discriminates the final candidate. Long-run duty-cycle estimation would require a separately designed prospective coverage experiment rather than indefinitely repeating short marker windows.

## Rejected / revised hypothesis ledger
1. H-O4 shorter fixed 720s is materially superior to fixed 840s: REVISE / NOT PROMOTED.
2. H-O5 post-READY max authoritative units=0 is materially superior to established max=1: REVISE / NOT PROMOTED.
3. Consumed handoff epoch with no next legal successor path is acceptable nonterminal exit: REJECT.
4. Scheduler WRITE_OK/STATE_OK can stand in for later WAKE_OK/WORK_OK: REJECT.
5. Scheduler target-to-start delta can always be interpreted as idle/lateness: REJECT; direct overlap evidence exists.
6. Repeating arbitrarily small accounting windows until a long-run duty-cycle number emerges: REJECT as an identification strategy; UNKNOWN must remain UNKNOWN absent prospective observation coverage.

## Decision
FINAL CONVERGENCE GATES SATISFIED FOR THE CURRENT MER RELAY OPTIMIZATION PROGRAM. The selected candidate is fixed/default 840s wake-start prearm, generation-CAS single substantive ownership, post-READY max one current atomic authoritative unit, prompt 2.2.10 nonstop/recovery/early-exit guards, observed-state reconciliation, and exact-generation orphan recovery primitive. Remaining provider miss-rate and long-run duty-cycle estimation questions are observational follow-up research, not material discriminating uncertainty among current candidate mechanisms.
