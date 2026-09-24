# O4 fixed-window evaluation — 840s vs 720s

## Direct Mer evidence
- A840: 3/3 CLEAN handoffs.
- B720: 3/3 CLEAN handoffs.
- Both windows preserved the same generation-fenced ownership/READY/CAS semantics and reported no duplicate authoritative side effect in the matched sample gate.
- Directly observable evidence currently establishes continuity equivalence at the tested sample size.
- The durable evidence does not contain a comparable quantitative useful-work-duty-cycle or control-overhead measurement sufficient to establish a material utilization advantage for 720s.

## Decision
KEEP 840s as the fixed/default comparator. Do NOT promote 720s as superior.

Reason: the O4 promotion rule requires the shorter offset to have continuity no worse AND materially better utilization/turnaround. Continuity passed, but material utilization improvement is not demonstrated. The project objective and Master Plan therefore prefer the simpler/safer fixed baseline when measured reliability/utilization are equivalent or indistinguishable.

## Model update
H-O4-ADAPTIVE-PREARM = REVISE.
The tested 720s fixed window is viable but not promoted. Adaptive shortening remains a hypothesis only for concrete successor-dependent boundaries and needs a discriminating measurement of actual wake lateness, handoff gap, and useful/control duty cycle before promotion.

## Next research boundary
Advance to O5 handoff optimization while retaining 840s as the default scheduler comparator. First O5 test should change one variable: READY-to-transfer safe-unit granularity / transfer latency, with scheduler offset held at 840s. Compare immediate yield after current atomic unit against the existing behavior, measuring READY->CAS latency and duplicate-side-effect safety.
