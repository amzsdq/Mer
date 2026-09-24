# O5 Matched-Window Evaluation — 2026-09-24

Authority: gen25 recovery owner
Hypothesis: H-O5-READY-TRANSFER-GRANULARITY

A = PREDECESSOR_POST_READY_MAX_AUTHORITATIVE_UNITS=1
- Clean handoffs: 3/3
- READY->CAS latency: 11s, 7s, 11s
- Mean latency: 9.67s
- Duplicate authoritative side effects: 0
- Control writes: 1/sample

B = PREDECESSOR_POST_READY_MAX_AUTHORITATIVE_UNITS=0
- Clean handoffs: 3/3
- READY->CAS latency: 7s, 8s, 8s
- Mean latency: 7.67s
- Duplicate authoritative side effects: 0
- Control writes: 1/sample

Decision: KEEP_A / REVISE_HYPOTHESIS.
Reason: B is 2.00s faster on the three-sample mean, but both windows are equally clean and have equal control-write counts; no durable materiality threshold or sufficiently powered evidence establishes that this latency difference is materially better. Under the project objective, equivalent reliability/utilization favors the simpler established baseline.

Additional finding: gen24 became stranded after the final B sample because the owner ended with a consumed handoff epoch and no next OPEN epoch. This is a liveness defect independent of A-vs-B and is promoted to O6 adverse-recovery work.
