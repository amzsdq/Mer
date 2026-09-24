# O7 Final Convergence Matrix — authoritative pass after final-kernel sample 1

Current owner: MER-O7-GEN29-20260924T2241KST / generation 29

| Gate | Evidence | Current verdict |
|---|---|---|
| Scheduler strategy specified | O4 retained fixed/default 840s comparator; adaptive shortening only for concrete successor-dependent boundaries | PASS |
| Prompt/canonical sync specified | deployed/canonical 2.2.10-EARLY-EXIT-RETRY / MER-OPT-2M, manifest fast path | PASS |
| Work/stop rule specified | no duration target; normal stops only PROGRAM_COMPLETE or SUCCESSOR_HANDOFF_COMPLETE; recoverable blocker is work | PASS |
| Authority fencing | generation + fresh-SHA CAS; exactly one substantive owner | PASS |
| Repeated clean handoffs | historical O1/O4/O5 windows clean; final-kernel sample 1 gen28->29 clean | PASS_TO_CURRENT_SCOPE |
| Duplicate authoritative side effects | zero in admitted O4/O5/O6 evidence and final-kernel sample 1 | PASS_TO_CURRENT_SCOPE |
| Retrospective WAKE_OK / WORK_OK | prior live 22:11:35 target -> 22:11:41 actual wake (+6s); this invocation also resumed the unchanged 2.2.10 candidate and performed durable work | PASS |
| Adverse recovery | O6 orphan suite 5/5; observed-state suite 4/4; live wake reconciliation PASS | PASS_WITH_PROVIDER_MISS_RATE_UNKNOWN |
| Simpler comparator | 720s not promoted over 840s; O5 zero-post-READY-unit candidate not promoted over established A | PASS |
| Rejected/revised hypotheses ledger | O4/O5 revisions plus stranded-owner and WRITE/STATE inference rejection retained | PASS |
| Direct duty-cycle / idle accounting | total elapsed remains insufficient; current sample separates scheduler target/readback and durable START but lacks a trustworthy direct useful-vs-control interval decomposition | OPEN |
| Repeated unchanged clean wakes under final candidate | sample 1 clean; require at least one additional unchanged final-kernel wake/handoff before calling this repeated | OPEN_SAMPLE_2 |
| Material continuity/utilization uncertainty still testable? | yes: sample 2 plus direct accounting method/evidence | OPEN |

## Final-kernel sample 1
- Candidate held unchanged: fixed/default 840s, generation CAS, post-READY max authoritative units=1, kernel 2.2.10.
- Wake-start prearm live readback: same canonical, enabled=true, exact_schedule, hourly RRULE, DTSTART 2026-09-24 22:55:02 KST.
- Successor READY: evidence/o7/O7_FINAL_KERNEL_GEN28_TO_GEN29_SAMPLE1_READY_20260924T2241KST.json.
- START marker: evidence/o7/O7_GEN29_START_MARKER_20260924T2241KST.json.
- Fresh-SHA ownership CAS: gen28 -> gen29 succeeded.
- Owner-exit guard: gen29 -> gen30 sample-2 epoch opened in the same authoritative ownership update.
- Duplicate authoritative side effects observed: 0.

## Rejected / revised hypothesis ledger
1. H-O4 shorter fixed 720s is materially superior to fixed 840s: REVISE / NOT PROMOTED.
2. H-O5 post-READY max authoritative units=0 is materially superior to established max=1: REVISE / NOT PROMOTED.
3. Consumed handoff epoch with no next legal successor path is acceptable nonterminal exit: REJECT.
4. Scheduler WRITE_OK/STATE_OK can stand in for later WAKE_OK/WORK_OK: REJECT.

## Decision
NOT COMPLETE. Do not disable the canonical automation. Run unchanged final-kernel sample 2 and continue constructing direct accounting that does not equate elapsed wall time with useful work. Provider miss rate remains UNKNOWN unless actual miss evidence accumulates.
