# O7 Final Convergence Matrix — initial authoritative pass

Owner: MER-O7-GEN28-20260924T2228KST / generation 28

| Gate | Evidence | Current verdict |
|---|---|---|
| Scheduler strategy specified | O4 retained fixed/default 840s comparator; adaptive shortening only for concrete successor-dependent boundaries | PASS |
| Prompt/canonical sync specified | deployed/canonical 2.2.10-EARLY-EXIT-RETRY / MER-OPT-2M, manifest fast path | PASS |
| Work/stop rule specified | no duration target; normal stops only PROGRAM_COMPLETE or SUCCESSOR_HANDOFF_COMPLETE; recoverable blocker is work | PASS |
| Authority fencing | generation + fresh-SHA CAS; exactly one substantive owner | PASS |
| Repeated clean handoffs | O1 stabilization 5/5; O4 A840 3/3; O4 B720 3/3; O5 A 3/3; O5 B 3/3 | PASS for historical tested windows |
| Duplicate authoritative side effects | zero in admitted O4/O5/O6 evidence | PASS to observed scope |
| Retrospective WAKE_OK / WORK_OK | live 22:11:35 target -> 22:11:41 actual wake (+6s), durable work resumed | PASS |
| Adverse recovery | O6 orphan suite 5/5; observed-state suite 4/4; live wake reconciliation PASS | PASS_WITH_PROVIDER_MISS_RATE_UNKNOWN |
| Simpler comparator | 720s not promoted over 840s; O5 zero-post-READY unit candidate not promoted over established A | PASS |
| Rejected/revised hypotheses ledger | O4 720s superiority REVISED/not promoted; O5 immediate-zero-unit superiority REVISED/not promoted; gen24 stranded-owner behavior rejected as acceptable liveness | PASS, consolidate below |
| Measured duty cycle / idle gap | direct comparable quantitative useful-work-duty-cycle evidence was explicitly absent at O4; current wake-to-wake schedule/wake points alone do not equal useful-work duty cycle | OPEN |
| Repeated unchanged clean wakes under final candidate | current final candidate includes later kernel hardening (2.2.10), so older clean handoffs are necessary prior evidence but do not by themselves establish repeated unchanged final-kernel wakes | OPEN |
| Material continuity/utilization uncertainty still testable? | yes: final-kernel repeated wake/handoff and directly observable useful/control/idle accounting | OPEN |

## Rejected / revised hypothesis ledger
1. H-O4 shorter fixed 720s is materially superior to fixed 840s: REVISE / NOT PROMOTED. Reliability tied 3/3 vs 3/3 and no material utilization advantage was measured.
2. H-O5 post-READY max authoritative units=0 is materially superior to established max=1: REVISE / NOT PROMOTED. Both 3/3 clean, equal control writes, observed mean READY->CAS advantage 2.00s without established materiality.
3. Implicit hypothesis that a consumed handoff epoch with no next legal successor path is an acceptable nonterminal owner exit: REJECT. Gen24 incident stranded authority; owner-exit guard and exact-generation recovery were added.
4. Hypothesis that scheduler WRITE_OK/STATE_OK can stand in for later WAKE_OK/WORK_OK: REJECT. O6 reconciliation keeps these evidence states separate.

## O7 decision at this boundary
NOT COMPLETE. Do not disable the canonical automation. The remaining discriminating work is not provider miss-rate estimation; it is (a) repeated unchanged final-kernel clean wake/handoff samples and (b) direct useful/control/idle accounting where observable. Provider miss-rate remains UNKNOWN unless actual miss evidence accumulates.

## Next sample contract
Hold primary candidate semantics unchanged: fixed/default 840s; generation CAS; post-READY max authoritative units=1; kernel 2.2.10. On the next actual wake, retrospectively classify the current verified prearm as WAKE_OK/WORK_OK, complete a normal gen28->gen29 handoff, and record timestamps needed to separate scheduler lead, wake lateness, handoff latency, useful work, control work, and inter-turn idle where directly observable. Do not infer useful work from total elapsed time.
